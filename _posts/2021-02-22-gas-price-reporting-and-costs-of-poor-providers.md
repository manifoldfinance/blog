# Gas Price Reporting Index

> Collection of All Gas Price Prediction and Reporting Services and their various formats

Whereever you may be getting your Ethereum transaction pricing information (A.K.A Gas or Gwei), we guarantee its almost entirely useless.

We would like to introduce a new service endpoint, [ERCGas.org](http://ercgas.org) - a statistically accurate and useful real time pricing engine for
ethereum transactions with a reference transaction utilized as a typical Uniswap trade. 

Not only do we provide percentile rankings over the distribution of pricing, we also plan on offering EIP1559 support within the next two weeks so that other developers 
and users can start testing it out.

Below you will find a small example and a recent result served from the API. After looking at our endpoint continue reading below to see what normally passes for transaction
cost estimates and reporting. *Hint* It's not much. 

### Fee Speed Definitions

Fastest: next block (i.e. <30 seconds) <br>
Fast: below 2 minutes (<10 blocks)  <br>
Medium: around 5 minutes (<20 blocks)  <br>
Slow: below 30 minutes (a.k.a safe-low, <120 blocks)  <br>

### New API Available: ERCGas.org

Visting [ercgas.org](ercgas.org) directly outputs the raw json response. Below is a way to use `jq` and get information you would want in one line

An example: 

```bash
curl -s ercgas.org | jq .percentile_75
```
resposne: 

```bash
"325 gwei"
```

### Data format of response 


> nanoeth is the **SI** nomenclature for `gwei`, we utilize `gwei` as it is what is used in normal conversations.

```json
{
    "number_of_blocks": 200,
    "latest_block_number": 11911047,
    "percentile_1": "0.000011001 gwei",
    "percentile_2": "0.000021445 gwei",
    "percentile_3": "10 gwei",
    "percentile_4": "10 gwei",
    "percentile_5": "10 gwei",
    "percentile_10": "135 gwei",
    "percentile_15": "152.000001459 gwei",
    "percentile_20": "181.000001459 gwei",
    "percentile_25": "200 gwei",
    "percentile_30": "239 gwei",
    "percentile_35": "261 gwei",
    "percentile_40": "264.000000233 gwei",
    "percentile_45": "270.6 gwei",
    "percentile_50": "283 gwei",
    "percentile_55": "300 gwei",
    "percentile_60": "301 gwei",
    "percentile_65": "305 gwei",
    "percentile_70": "306 gwei",
    "percentile_75": "309 gwei",
    "percentile_80": "330 gwei",
    "percentile_85": "350 gwei",
    "percentile_90": "366 gwei",
    "percentile_95": "367.2 gwei",
    "percentile_96": "368 gwei",
    "percentile_97": "368.000001459 gwei",
    "percentile_98": "370 gwei",
    "percentile_99": "376 gwei",
    "percentile_100": "403 gwei"
}
```


## Catalog

- [Gas Price Reporting Index](#gas-price-reporting-index)
    + [Fee Speed Definitions](#fee-speed-definitions)
    + [gaswatch](#gaswatch)
    + [gnosis](#gnosis)
    + [MetaMask  / Consensys CoDeFi](#metamask----consensys-codefi)
    + [ethGasStation](#ethgasstation)
    + [etherchain.org](#etherchainorg)
    + [poanetwork](#poanetwork)
    + [Zoltu](#zoltu)
    + [MyCrypto](#mycrypto)
    + [EtherScan](#etherscan)
  * [URL Index](#url-index)

### gaswatch
```json
{"code":200,"data":{"rapid":131000000000,"fast":116000000000,"standard":100000000000,"slow":91600000000,"timestamp":1613914581546}}
```

### gnosis

[endpoint url](https://safe-relay.gnosis.io/api/v1/gas-station/)


```json
{
  "lastUpdate": "2021-02-21T13:38:38.945308Z",
  "lowest": "2",
  "safeLow": "109000000001",
  "standard": "119000000001",
  "fast": "131000000001",
  "fastest": "10680081443136"
}
```

### MetaMask  / Consensys CoDeFi

[endpoint url](https://api.metaswap.codefi.network/gasPrices)

```json 
{"SafeGasPrice":"100","ProposeGasPrice":"108","FastGasPrice":"119"}
```

```json
{
    "SafeGasPrice": "100",
    "ProposeGasPrice": "108",
    "FastGasPrice": "119"
}
```

### ethGasStation 

[endpoint url](https://ethgasstation.info/json/ethgasAPI.json)

```json
{"fast": 1200.0, "fastest": 1200.0, "safeLow": 1020.0, "average": 1050.0, "block_time": 13.327868852459016, "blockNum": 11900622, "speed": 0.997822721438169, "safeLowWait": 12.9, "avgWait": 1.5, "fastWait": 0.5, "fastestWait": 0.5, "gasPriceRange": {"1200": 0.5, "1180": 0.5, "1160": 0.6, "1140": 0.6, "1120": 0.7, "1100": 0.7, "1080": 1.2, "1060": 1.5, "1040": 11.5, "1020": 12.9, "1000": 14.3, "980": 17.4, "960": 18.9, "940": 21.1, "920": 222.1, "900": 222.1, "880": 222.1, "860": 222.1, "840": 222.1, "820": 222.1, "800": 222.1, "780": 222.1, "760": 222.1, "740": 222.1, "720": 222.1, "700": 222.1, "680": 222.1, "660": 222.1, "640": 222.1, "620": 222.1, "600": 222.1, "580": 222.1, "560": 222.1, "540": 222.1, "520": 222.1, "500": 222.1, "480": 222.1, "460": 222.1, "440": 222.1, "420": 222.1, "400": 222.1, "380": 222.1, "360": 222.1, "340": 222.1, "320": 222.1, "300": 222.1, "280": 222.1, "260": 222.1, "240": 222.1, "220": 222.1, "200": 222.1, "190": 222.1, "180": 222.1, "170": 222.1, "160": 222.1, "150": 222.1, "140": 222.1, "130": 222.1, "120": 222.1, "110": 222.1, "100": 222.1, "90": 222.1, "80": 222.1, "70": 222.1, "60": 222.1, "50": 222.1, "40": 222.1, "30": 222.1, "20": 222.1, "10": 222.1, "8": 222.1, "6": 222.1, "4": 222.1, "1050": 1.5}}
```

```json
{
    "fast": 1200.0,
    "fastest": 1200.0,
    "safeLow": 1020.0,
    "average": 1050.0,
    "block_time": 13.327868852459016,
    "blockNum": 11900622,
    "speed": 0.997822721438169,
    "safeLowWait": 12.9,
    "avgWait": 1.5,
    "fastWait": 0.5,
    "fastestWait": 0.5,
    "gasPriceRange": {
        "1200": 0.5,
        "1180": 0.5,
        "1160": 0.6,
        "1140": 0.6,
        "1120": 0.7,
        "1100": 0.7,
        "1080": 1.2,
        "1060": 1.5,
        "1040": 11.5,
        "1020": 12.9,
        "1000": 14.3,
        "980": 17.4,
        "960": 18.9,
        "940": 21.1,
        "920": 222.1,
        "900": 222.1,
        "880": 222.1,
        "860": 222.1,
        "840": 222.1,
        "820": 222.1,
        "800": 222.1,
        "780": 222.1,
        "760": 222.1,
        "740": 222.1,
        "720": 222.1,
        "700": 222.1,
        "680": 222.1,
        "660": 222.1,
        "640": 222.1,
        "620": 222.1,
        "600": 222.1,
        "580": 222.1,
        "560": 222.1,
        "540": 222.1,
        "520": 222.1,
        "500": 222.1,
        "480": 222.1,
        "460": 222.1,
        "440": 222.1,
        "420": 222.1,
        "400": 222.1,
        "380": 222.1,
        "360": 222.1,
        "340": 222.1,
        "320": 222.1,
        "300": 222.1,
        "280": 222.1,
        "260": 222.1,
        "240": 222.1,
        "220": 222.1,
        "200": 222.1,
        "190": 222.1,
        "180": 222.1,
        "170": 222.1,
        "160": 222.1,
        "150": 222.1,
        "140": 222.1,
        "130": 222.1,
        "120": 222.1,
        "110": 222.1,
        "100": 222.1,
        "90": 222.1,
        "80": 222.1,
        "70": 222.1,
        "60": 222.1,
        "50": 222.1,
        "40": 222.1,
        "30": 222.1,
        "20": 222.1,
        "10": 222.1,
        "8": 222.1,
        "6": 222.1,
        "4": 222.1,
        "1050": 1.5
    }
}
```

### etherchain.org 

[endpoint url](https://www.etherchain.org/api/gasPriceOracle)

```json
{"safeLow":102,"standard":105,"fast":114.6,"fastest":120}
```


### poanetwork

[endpoint url](https://gasprice.poa.network/)

```json
{"health":true,"block_number":11900628,"slow":101.0,"standard":107.0,"fast":115.0,"instant":130.8,"block_time":13.191}
```

```json
{
    "health": true,
    "block_number": 11900628,
    "slow": 101.0,
    "standard": 107.0,
    "fast": 115.0,
    "instant": 130.8,
    "block_time": 13.191
}
```

### MyCrypto

```json
{"safeLow":159,"standard":184,"fast":262,"fastest":289,"blockNum":11907235}
```

```json
{
    "safeLow": 159,
    "standard": 184,
    "fast": 262,
    "fastest": 289,
    "blockNum": 11907235
}
```

### EtherScan

[api endpoint *key required*](https://api.etherscan.io/api?module=gastracker&action=gasoracle&apikey=${YOUR_API_KEY})

```json
{"status":"1","message":"OK","result":{"LastBlock":"11907242","SafeGasPrice":"248","ProposeGasPrice":"269","FastGasPrice":"294"}}
```

```json
{
    "status": "1",
    "message": "OK",
    "result": {
        "LastBlock": "11907242",
        "SafeGasPrice": "248",
        "ProposeGasPrice": "269",
        "FastGasPrice": "294"
    }
}
```


## URL Index

Current offchain list:

- https://ethgasstation.info/json/ethgasAPI.json
- https://www.etherchain.org/api/gasPriceOracle
- https://gasprice.poa.network/
- https://www.gasnow.org/api/v3/gas/price
