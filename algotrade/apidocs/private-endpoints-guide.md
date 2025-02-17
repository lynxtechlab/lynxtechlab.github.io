
# Stock Analysis Documentation

## Introduction

Welcome to the **Stock Analysis Documentation**. This guide provides a comprehensive overview of how stock market data is analyzed, including financial indicators, technical analysis, and predictive models. Whether you are an investor, trader, or developer, this documentation will help you understand how to interpret market data effectively.

### Key Features

- **Fundamental Analysis:** Evaluating company financials and industry trends.
- **Technical Analysis:** Identifying market trends using indicators.
- **Predictive Models:** Using algorithms for forecasting.
- **API & Automation:** Integrating stock analysis into automated workflows.

### Audience

This documentation is intended for:
- **Traders and Investors** looking to make informed decisions.
- **Developers** integrating stock data into applications.
- **Financial Analysts** conducting in-depth market research.

Proceed to the next sections to explore the components of the stock analysis system in detail.

## Average Trading Volume for Stock Over the Past Five Days

curl -H "x-api-key: YOUR_API_KEY" https://algotrade.lynxtechlab.com/api/v1/volavg

```json
[
    {
        "StockCode": "BINBN",
        "AvgVol": "113129378",
        "timefetch": "2025-02-14T18:40:01.000Z"
    },
    {
        "StockCode": "GLRMK",
        "AvgVol": "733065116",
        "timefetch": "2025-02-14T18:40:01.000Z"
    }
]
```

## Intraday Stock Statistics: 5-Minute Chart Analysis

ticks = A numeric value that represents 5-minute range of data.

curl -H "x-api-key: YOUR_API_KEY" "https://algotrade.lynxtechlab.com/api/v1/stockstats?stock=SAHOL&ticks=5"

```json
[
    {
        "symbol": "SAHOL",
        "timefetch": "2025-02-17T10:22:55.000Z",
        "rsiValue": 47.643755,
        "macdValue": -0.249024,
        "rsv": 51.724138,
        "priceValue": 100.1,
        "cci": 88.010862,
        "aroon": -33.333333,
        "kdjk": 16.748466,
        "kdjd": 13.733219,
        "kdjj": 22.778958,
        "boll": 100.507143,
        "bollub": 101.391612,
        "bolllb": 99.622674,
        "macds": -0.233261,
        "macdh": -0.015763,
        "roc": 0.350877,
        "high": 100.8,
        "low": 99.35,
        "ftr": -7.600402,
        "RN": "1"
    },
    {
        "symbol": "SAHOL",
        "timefetch": "2025-02-17T10:19:46.000Z",
        "rsiValue": 35.204517,
        "macdValue": -0.285876,
        "rsv": 16.363636,
        "priceValue": 99.8,
        "cci": 126.406157,
        "aroon": -66.666667,
        "kdjk": 11.486335,
        "kdjd": 12.225596,
        "kdjj": 10.007812,
        "boll": 100.545238,
        "bollub": 101.424957,
        "bolllb": 99.66552,
        "macds": -0.225379,
        "macdh": -0.060497,
        "roc": -0.2,
        "high": 100.8,
        "low": 99.35,
        "ftr": -7.600402,
        "RN": "2"
    },
    {
        "symbol": "SAHOL",
        "timefetch": "2025-02-17T10:14:58.000Z",
        "rsiValue": 19.208598,
        "macdValue": -0.280756,
        "rsv": 5.454545,
        "priceValue": 99.5,
        "cci": 185.644769,
        "aroon": -83.333333,
        "kdjk": 9.047684,
        "kdjd": 12.595227,
        "kdjj": 1.952599,
        "boll": 100.602381,
        "bollub": 101.43333,
        "bolllb": 99.771432,
        "macds": -0.19513,
        "macdh": -0.085625,
        "roc": -1.191658,
        "high": 100.8,
        "low": 99.35,
        "ftr": -7.600402,
        "RN": "3"
    },
    {
        "symbol": "SAHOL",
        "timefetch": "2025-02-17T10:09:45.000Z",
        "rsiValue": 23.182998,
        "macdValue": -0.215109,
        "rsv": 6,
        "priceValue": 99.75,
        "cci": 234.003831,
        "aroon": -83.333333,
        "kdjk": 10.844254,
        "kdjd": 14.368998,
        "kdjj": 3.794765,
        "boll": 100.664286,
        "bollub": 101.326964,
        "bolllb": 100.001607,
        "macds": -0.152318,
        "macdh": -0.062791,
        "roc": -0.746269,
        "high": 100.8,
        "low": 99.6,
        "ftr": -7.600402,
        "RN": "4"
    },
    {
        "symbol": "SAHOL",
        "timefetch": "2025-02-17T10:04:51.000Z",
        "rsiValue": 28.013094,
        "macdValue": -0.152651,
        "rsv": 2.325581,
        "priceValue": 100,
        "cci": 283.333333,
        "aroon": -83.333333,
        "kdjk": 13.266381,
        "kdjd": 16.131371,
        "kdjj": 7.536401,
        "boll": 100.728571,
        "bollub": 101.26947,
        "bolllb": 100.187673,
        "macds": -0.120922,
        "macdh": -0.031729,
        "roc": -0.398406,
        "high": 100.8,
        "low": 99.95,
        "ftr": -7.600402,
        "RN": "5"
    }
]
```

## Historical Stock Statistics: Day Chart Analysis

curl -H "x-api-key: YOUR_API_KEY" "https://algotrade.lynxtechlab.com/api/v1/stockstatshistorical?stock=YKBNK&ticks=5"

ticks = A numeric value that represents day range of data.

```json
[
    {
        "symbol": "YKBNK",
        "timefetch": "2025-02-17T11:36:06.000Z",
        "rsiValue": 40.085575,
        "macdValue": -0.666598,
        "macds": -0.424696,
        "macdh": -0.241901,
        "supertrend": 29.481159,
        "supertrendlb": 27.811772,
        "supertrendub": 29.481159,
        "rsv": 13.360324,
        "priceValue": 28.72,
        "cci": -77.875439,
        "aroon": -73.333333,
        "kdjk": 10.144451,
        "kdjd": 10.797184,
        "kdjj": 8.838985,
        "boll": 30.572381,
        "bollub": 34.255195,
        "bolllb": 26.889567,
        "roc": -1.373626,
        "high": 28.86,
        "low": 28.38,
        "datefetch": "2025-02-17T00:00:00.000Z",
        "ema5": 28.741936,
        "ema20": 29.817814,
        "ema50": 30.00789,
        "ema100": 29.725196,
        "ema200": 29.605682,
        "ftr": -3.592264,
        "vwma": 29.273291,
        "RN": "1"
    },
    {
        "symbol": "YKBNK",
        "timefetch": "2025-02-14T19:00:00.000Z",
        "rsiValue": 39.913507,
        "macdValue": -0.661368,
        "macds": -0.364221,
        "macdh": -0.297147,
        "supertrend": 29.48614,
        "supertrendlb": 27.811772,
        "supertrendub": 29.48614,
        "rsv": 12.955466,
        "priceValue": 28.7,
        "cci": -84.884414,
        "aroon": -73.333333,
        "kdjk": 9.238415,
        "kdjd": 11.12355,
        "kdjj": 5.468146,
        "boll": 30.714286,
        "bollub": 34.326291,
        "bolllb": 27.10228,
        "roc": -6.575521,
        "high": 28.94,
        "low": 28.42,
        "datefetch": "2025-02-14T00:00:00.000Z",
        "ema5": 28.752904,
        "ema20": 29.927595,
        "ema50": 30.06046,
        "ema100": 29.745638,
        "ema200": 29.61537,
        "ftr": -3.401671,
        "vwma": 29.86648,
        "RN": "2"
    },
    {
        "symbol": "YKBNK",
        "timefetch": "2025-02-13T19:00:00.000Z",
        "rsiValue": 39.589656,
        "macdValue": -0.643036,
        "macds": -0.289934,
        "macdh": -0.353102,
        "supertrend": 29.48614,
        "supertrendlb": 27.811772,
        "supertrendub": 29.48614,
        "rsv": 11.627907,
        "priceValue": 28.66,
        "cci": -93.69469,
        "aroon": -86.666667,
        "kdjk": 8.060522,
        "kdjd": 12.066118,
        "kdjj": 0.04933,
        "boll": 30.819048,
        "bollub": 34.311313,
        "bolllb": 27.326782,
        "roc": -10.381488,
        "high": 29.02,
        "low": 28.4,
        "datefetch": "2025-02-13T00:00:00.000Z",
        "ema5": 28.779356,
        "ema20": 30.050354,
        "ema50": 30.115991,
        "ema100": 29.766905,
        "ema200": 29.625393,
        "ftr": -3.267959,
        "vwma": 30.104265,
        "RN": "3"
    },
    {
        "symbol": "YKBNK",
        "timefetch": "2025-02-12T19:00:00.000Z",
        "rsiValue": 36.557804,
        "macdValue": -0.606213,
        "macds": -0.201659,
        "macdh": -0.404554,
        "supertrend": 29.48614,
        "supertrendlb": 28.679378,
        "supertrendub": 29.48614,
        "rsv": 3.985507,
        "priceValue": 28.28,
        "cci": -109.416737,
        "aroon": -93.333333,
        "kdjk": 6.656,
        "kdjd": 14.068915,
        "kdjj": -8.169831,
        "boll": 30.872381,
        "bollub": 34.258765,
        "bolllb": 27.485997,
        "roc": -12.716049,
        "high": 29.1,
        "low": 28.06,
        "datefetch": "2025-02-12T00:00:00.000Z",
        "ema5": 28.839034,
        "ema20": 30.18939,
        "ema50": 30.175422,
        "ema100": 29.789422,
        "ema200": 29.635972,
        "ftr": -3.066661,
        "vwma": 30.527875,
        "RN": "4"
    },
    {
        "symbol": "YKBNK",
        "timefetch": "2025-02-11T19:00:00.000Z",
        "rsiValue": 38.553832,
        "macdValue": -0.511704,
        "macds": -0.10052,
        "macdh": -0.411183,
        "supertrend": 29.739447,
        "supertrendlb": 28.679378,
        "supertrendub": 29.739447,
        "rsv": 1.612903,
        "priceValue": 28.7,
        "cci": -104.140127,
        "aroon": -86.666667,
        "kdjk": 7.991246,
        "kdjd": 17.775373,
        "kdjj": -11.577008,
        "boll": 30.958095,
        "bollub": 34.154691,
        "bolllb": 27.761499,
        "roc": -12.606577,
        "high": 29.06,
        "low": 28.62,
        "datefetch": "2025-02-11T00:00:00.000Z",
        "ema5": 29.11855,
        "ema20": 30.380329,
        "ema50": 30.25279,
        "ema100": 29.82013,
        "ema200": 29.650846,
        "ftr": -2.635723,
        "vwma": 30.919434,
        "RN": "5"
    }
]
```



## Statistics on Money Flow by Stock: 10-Minute Chart Analysis

curl -H "x-api-key: YOUR_API_KEY" "https://algotrade.lynxtechlab.com/api/v1/paragirislivetoday?stock=YKBNK"

```json
[

    {
        "symbol": "YKBNK",
        "timeupdated": "2025-02-17T10:59:00.000Z",
        "Date": "2025-02-17T00:00:00.000Z",
        "BuyVol": 718512478,
        "SellVol": 591792610,
        "ParaGiris": 45.16,
        "ParaGirisNet": -126719.868,
        "ParaGirisNetPre": -128362.088,
        "ParaGirisPre": 44.79,
        "ParaGirisDiff": 1642.22,
        "ParaGirisRatioDiff": 0.37
    },
    {
        "symbol": "YKBNK",
        "timeupdated": "2025-02-17T10:48:59.000Z",
        "Date": "2025-02-17T00:00:00.000Z",
        "BuyVol": 680633201,
        "SellVol": 552271113,
        "ParaGiris": 44.79,
        "ParaGirisNet": -128362.088,
        "ParaGirisNetPre": -150364.452,
        "ParaGirisPre": 42.91,
        "ParaGirisDiff": 22002.364,
        "ParaGirisRatioDiff": 1.88
    },
    {
        "symbol": "YKBNK",
        "timeupdated": "2025-02-17T10:39:02.000Z",
        "Date": "2025-02-17T00:00:00.000Z",
        "BuyVol": 605750322,
        "SellVol": 455385870,
        "ParaGiris": 42.91,
        "ParaGirisNet": -150364.452,
        "ParaGirisNetPre": -130634.999,
        "ParaGirisPre": 40.47,
        "ParaGirisDiff": -19729.453,
        "ParaGirisRatioDiff": 2.44
    },
    {
        "symbol": "YKBNK",
        "timeupdated": "2025-02-17T10:29:24.000Z",
        "Date": "2025-02-17T00:00:00.000Z",
        "BuyVol": 408053043,
        "SellVol": 277418044,
        "ParaGiris": 40.47,
        "ParaGirisNet": -130634.999,
        "ParaGirisNetPre": -47854.544,
        "ParaGirisPre": 43,
        "ParaGirisDiff": -82780.455,
        "ParaGirisRatioDiff": -2.53
    },
    {
        "symbol": "YKBNK",
        "timeupdated": "2025-02-17T10:19:16.000Z",
        "Date": "2025-02-17T00:00:00.000Z",
        "BuyVol": 194857810,
        "SellVol": 147003266,
        "ParaGiris": 43,
        "ParaGirisNet": -47854.544,
        "ParaGirisNetPre": 0,
        "ParaGirisPre": null,
        "ParaGirisDiff": -47854.544,
        "ParaGirisRatioDiff": null
    }
]

```



## Matriks: 15 Seconds Delayed Live Data

curl -H "x-api-key: YOUR_API_KEY" "https://algotrade.lynxtechlab.com/api/v1/matrikslivestocks"

```json
[
    {
        "Symbol": "A1CAP",
        "timefetch": "2025-02-17T12:12:51.000Z",
        "Last": 23.88,
        "PreviousDay": 23.82,
        "WeightedAvg": 24.02,
        "Open": 23.84,
        "High": 24.48,
        "Low": 23.78
    },
    {
        "Symbol": "MOPAS",
        "timefetch": "2025-02-17T12:12:52.000Z",
        "Last": 32.38,
        "PreviousDay": 32.46,
        "WeightedAvg": 32.7,
        "Open": 33.92,
        "High": 34.2,
        "Low": 31.58
    },
    {
        "Symbol": "AKFIS",
        "timefetch": "2025-02-17T12:12:52.000Z",
        "Last": 24.64,
        "PreviousDay": 24.26,
        "WeightedAvg": 24.61,
        "Open": 24.4,
        "High": 24.9,
        "Low": 24.32
    },
    {
        "Symbol": "ALTINS1",
        "timefetch": "2025-02-17T12:12:52.000Z",
        "Last": 34.81,
        "PreviousDay": 34.77,
        "WeightedAvg": 34.56,
        "Open": 34.66,
        "High": 34.82,
        "Low": 34.55
    },
    {
        "Symbol": "CGCAM",
        "timefetch": "2025-02-17T12:12:53.000Z",
        "Last": 28,
        "PreviousDay": 27.48,
        "WeightedAvg": 27.83,
        "Open": 27.66,
        "High": 28.16,
        "Low": 27.5
    }
]
```