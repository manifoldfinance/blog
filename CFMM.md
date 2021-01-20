
## Improved Price Oracles: Constant Function Market
Makers

Definition of a CFMM. A constant function market maker, or CFMM, 
is a type of automated market maker defined by its trading function,
\(\varphi: R _{+}^{n} \times R _{+}^{n} \times R _{+}^{n} \rightarrow R ,\) 

and its reserves, 

\(R \in R _{+}^{n}\). Here, \(R_{i}\) 

specifies how much of coin \(i\) the CFMM is allowed to use or interact with, 
while the trading function specifies what constitutes a valid trade.

### Optimization over possible CFMMs

Note that the given conditions deﬁne a family of CFMMs which are likely to be useful in
practice. This implies that, for any performance metric (e.g., average total reserve value
for a given market model) that a market maker designer wishes to optimize, one could
ﬁnd an (approximately) optimal CFMM to accomplish this task. The problem is likely to be
computationally diﬃcult to solve exactly in most important cases, but we suspect that many
commonly-used heuristics will likely ﬁnd good results. Though this approach is unlikely to
be feasible except when n is small, we imagine that the very useful case of n = 2 can be
quickly optimized on modern hardware for many useful performance metrics.
Additionally, if the trading function ϕ is parametrized by a small number of parameters
(for example, the parameters α, β in (5)), it is possible to at least approximately optimize
these parameters to maximize or minimize some desired objective function of the trading
sets or the reachable sets

One such question is:
what is a natural generalization of the reachable set when the trading function (or, equiva-
lently, the reachable set) is also time-dependent? A simple (but likely woefully incomplete)
answer is that, if the reachable set depends on time, say St(R), where t ≥ 0 is a time variable,
we additionally have 

St′(R) ⊆ St(R) for all R and t′ ≥ t. 

This retains some of the given

properties, such as the lower bounds on the total reserve values given in (11), but is likely
to be too strong of a condition to be useful in practice.
Another natural question is, are there good restrictions on how liquidity providers should
add liquidity to reserves? One could imagine that, in some scenarios, allowing agents to add
coins to reserves in an arbitrary way could lead to large losses for liquidity providers. An
even more fundamental question, which we do not cover at all is: can liquidity provision
easily be included in a similar framework? We suspect so, but even this is not clear at the
moment and is likely to be a good avenue for future exploration.
