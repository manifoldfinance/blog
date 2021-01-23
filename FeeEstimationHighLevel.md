# High level description Bitcoin Core's fee estimation algorithm

> surce: https://gist.github.com/morcos/d3637f015bc4e607e1fd10d8351e9f41

The algorithm takes as input a target which represents a number of blocks within which you would like your transaction to be included in the blockchain. It returns a fee rate that you should use on your transaction in order to achieve this.

The algorithm is conceptually very simple and does not attempt to have any predictive power over future conditions. It only looks at some recent history of transactions and returns the lowest fee rate such that in that recent history a very high fraction of transactions with that fee rate were confirmed in the block chain in less than the target number of blocks.

Transactions can occur with a nearly continuous range of fee rates and so in order to avoid tracking every historical transaction independently, they are grouped into fee rate "buckets". A fee rate bucket represents a range of fee rates within which the algorithm treats all transactions as having approximately the same fee rate and the answer the algorithm returns is actually the lowest fee rate bucket such that transactions in that bucket and all higher buckets were confirmed within the target over the recent history.

The recent history is defined as an exponentially decaying moving average with the data point counted at the time of transaction confirmation. In 0.14 this decay is 0.998 or a half-life of 346 blocks. Only transactions which had no unconfirmed parents at the time they entered the mempool are considered.

The implementation conceptually is that for each fee rate bucket you keep a counter for each possible confirmation target (1-25 in 0.14) of how many transactions in that bucket were confirmed in the target or less. You also keep an overall counter of all transactions of that fee rate. So if a block includes a specific transaction T, when you look up its mempool entry and see it entered the mempool 12 blocks ago, you then increment the counters for targets 12-25 for its fee rate bucket and increment the total tx count for that bucket. All counters are decayed by 0.998 every block.

To calculate an estimate you look only at the counters corresponding to your desired confirmation target and you start at the highest fee rate bucket and evaluate for txs in that fee rate bucket whether:

txs-confirmed-in-target-or-less / (all-confirmed-txs + txs-still-in-mempool-for-at-least-target) > threshold (0.95 in 0.14).

If you pass the threshold you check the next highest fee rate bucket and keep going, returning the last bucket that met the threshold before the first bucket which fails it. There is a required number of data points per bucket before doing the evaluation and you will just combine successive fee rate buckets to achieve that.

# Proposed Changes for 0.15

Please see [PR 10199](https://github.com/bitcoin/bitcoin/pull/10199)

## Under the hood improvements

- The fee estimate calculation has been improved so that failing to hit the threshold returns a fee rate only if continuing to add datapoints from lower fee rate buckets does not cause the passing percentage to re-exceed the threshold. This allows for a smaller required number of data points because extraneous noise won't accidentally cause a high estimate.
- The size of fee rate buckets has been halved (more precision possible in part due to above change)
- An additional counter is introduced for each feerate,target pair that counts transactions which are evicted or otherwise removed from the mempool without being included in a block. This counter is incremented for every target <= the number of blocks the transaction had been in the mempool at the time of its eviction. And the threshold calculation is changed to add failed-txs-which-left-mempool-after-target to the denominator. These failures are recorded for all mempool transactions at shutdown.
- A concept of scale is introduced for performance reasons, where confirmation targets are only resolved to within a certain scale. (for instance at scale of 2 a transaction confirmed in 19 or 20 blocks is tracked in the same counter)

## Algorithm Changes

- The algorithm is now repeated 3 times to cover different horizons for recent history.
  - Short history: 0.962 decay (half-life 18 blocks, 3 hours) for targets up to 12 (scale 1)
  - Medium history: 0.9952 decay (half-life of 144 blocks, 1 day) for targets up to 48 (scale 2)
  - Long hisotry: 0.99931 decay (half-life of 1008 blocks, 1 week) for targets up to 1008 (scale 24)
- The logic of estimatesmartfee has changed. Instead of requiring a 95% threshold at the target, it now returns the maximum of these 3 calculations:

  - 60% threshold at target/2
  - 85% threshold at target
  - 95% threshold at 2 \* target

  Each of the above calculations is done with the fee estimate data for the shortest time horizon that contains the target. If we can not query the short time horizon for a target of say 14, but the maximum possible target of 12 in the short time horizon would produce a lower fee rate, that lower answer is used. The ability to use a much shorter time horizon when possible makes the estimates far more responsive to changes in the prevailing conditions.

- Estimates are optionally (but by default) conservative, meaning that the 95% threshold for 2 \* target must be met not only at the shortest possible horizon but also at all longer horizons. This is useful for transactions (such as those unable to use CPFP or RBF) that do not want to take the risk of an extended delay if conditions change adversely.
- A maximum target for which estimates can be returned for is determined by tracking how many blocks have been recorded for data and setting the max target to half this number. When estimates are saved, this historical max target can be used if the estimates are restarted within a reasonable time window (6 weeks). The historical max target is replaced on shutdown when the new estimates have achieved a max target at least half as long.
