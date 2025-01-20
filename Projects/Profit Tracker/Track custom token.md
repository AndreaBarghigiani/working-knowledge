If I want to track the prices of a custom token I could use the DexScreener API that will answer me with a JSON with the following structure:
```json
{
  "schemaVersion": "1.0.0",
  "pairs": [
    {
      "chainId": "bsc",
      "dexId": "pancakeswap",
      "url": "https://dexscreener.com/bsc/0x50a7bad86bd493c128863c4df8a830417763f2d6",
      "pairAddress": "0x50A7BAd86bD493c128863c4df8a830417763F2d6",
      "labels": ["v2"],
      "baseToken": {
        "address": "0x29C55f1b02A95F0B30e61976835a3eee2359ad92",
        "name": "ESHARE V2",
        "symbol": "ESHARE"
      },
      "quoteToken": {
        "address": "0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c",
        "name": "Wrapped BNB",
        "symbol": "WBNB"
      },
      "priceNative": "0.1454",
      "priceUsd": "34.55",
      "txns": {
        "h1": { "buys": 0, "sells": 0 },
        "h24": { "buys": 9, "sells": 31 },
        "h6": { "buys": 1, "sells": 2 },
        "m5": { "buys": 0, "sells": 0 }
      },
      "volume": { "h24": 20462.13, "h6": 4883.77, "h1": 0, "m5": 0 },
      "priceChange": { "h1": 0, "h24": -6.77, "h6": -6.21, "m5": 0 },
      "liquidity": { "usd": 136452.39, "base": 1974.3231, "quote": 287.1203 },
      "fdv": 1745243,
      "pairCreatedAt": 1675115168000
    }
  ]
}
```

Keep in mind that checking these API **will not give you CoinGecko ID**, this could be good to identify if we're talking about a custom token.

Once I got these information I should massage the data as follow to respect the `Token` db structure:
```js
{
	symbol: pairs[0].baseToken.symbol,
	name: pairs[0].baseToken.name,
	iconUrl: null,
	latestPrice: pairs[0].priceUsd, (if we want to get specific we could calculate based on multiple pairs)
	coingecko_id: null,
	platforms: {[pairs[0].chainId]: pairs[0].baseToken.address}
	tracked: true,
}
```