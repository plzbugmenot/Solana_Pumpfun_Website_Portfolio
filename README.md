# Solana Pumpfun Sniper Bot

## Main Features
- <b>Real-Time Monitoring</b>: Tracks newly launched tokens on Pumpfun.
- <b>Automated Trading</b>: Executes buy and sell orders based on user-defined settings.
- <b>Blockchain Interaction</b>: Utilizes the Solana RPC API for seamless blockchain transactions.
- <b>User Alerts</b>: Sends notifications for all transactions and important events.
- <b>Comprehensive Logs</b>: Maintains detailed records of all trading activity for user review.


## Tech stack
- Typescript
- Node.js
- Mongodb
- Email Verification
- Solana/web3
- Pumpfun API
- DexScreener API
- Jupiter API

# API Endpoints

API prefix: `/api/{apiversion}`

## Auth

- GET `/auth/sendcode`

```js
request:
{
  email: "email@example.com",
}
response: 200
{
  message: "Code sent successfully",
  timestamp: number,
}
response: 500
{
  message: "Login failed",
  error: "This is error message",
}
```

- POST `/auth/login`

```js
request:
{
  email: "email@example.com",
  otp: string,
}
response: 200
{
  message: "Login successful",
  token: string,
  walletAddress: string,
  email: "email@example.com",
}
response: 400
{
  message: "OTP has expired" | "Invalid OTP" | "No OTP found for this email"
},
response: 500
{
  message: "Login failed",
  error: "This is error message",
}
```

- POST `/auth/logout`

```js
response: 200
{
  message: "Logged out successfully",
  timestamp: number,
}
response: 500
{
  message: "Logout failed",
  error: "This is error message",
}
```

## Dashboard

- POST `/token/mark-read`

```js
request: {
  id: id;
}
```

- POST `/token/mark-all-read`
- GET `/token/:time/`

```js
response: 200
{
  data: pumpfunTokenData[],
  total: 100,
  alerts: alerts[],
  offset: 0,
  limit: 10,
}
```

## Setting

- GET `/setting/main`
- POST `/setting/main`

```js
request: {
  isRunning: boolean;
  workingHours: {
    start: string;
    end: string;
    enabled: boolean;
  }
  isSniping: boolean;
}
```

- GET `/setting/buy`
- POST `/setting/buy`

```js
request: {
  age: {
    value: number;
    enabled: boolean;
  }
  marketCap: {
    min: number;
    max: number;
    enabled: boolean;
  }
  maxDevBuyAmount: {
    value: number;
    enabled: boolean;
  }
  maxDevHoldingAmount: {
    value: number;
    enabled: boolean;
  }
  holders: {
    value: number;
    enabled: boolean;
  }
  lastMinuteTxns: {
    value: number;
    enabled: boolean;
  }
  lastHourVolume: {
    value: number;
    enabled: boolean;
  }
  maxGasPrice: number;
  slippage: number;
  jitoTipAmount: number;
  investmentPerToken: number;
}
```

- GET `/setting/sell`
- POST `/setting/sell`

```js
request: {
  saleRules: [
    {
      percent: number;
      revenue: number;
    }
  ];
  lossExitPercent: number;
}
```

## Assets

- GET `/assets/`
```js
{
  calcTable: {
    currentBalance: number,
    totalProfit: number,
    realProfit: number,
    unRealProfit: number,
    totalInvested: number,
    totalTickers: number,
    successTickers: number,
    totalFeePaid: number,
    currentPercent: number,
  },
  data: [
    {
      mint: string;
      tokenName: string;
      tokenSymbol: string;
      tokenImage: string;
      tokenCreateTime: number;
      investedAmount: number;
      investedPrice_usd: number;
      investedMC_usd: number;
      investedAmount_usd: number;
      currentAmount: number;
      currentPrice_usd: number;
      currentMC_usd: number;
      fdv: number;
      pnl: {
        profit_usd: number;
        percent: number;
      };
      holding: {
        value_usd: number;
      };
      sellingStep: number;
      realisedProfit: number;
      unRealizedProfit: number;
      totalFee: number;
    }
  ],
  total: number,
  offset: number,
  limit: number,
}
```
- GET `/assets/wallet`

```js
{
  walletBal: number,
  solPrice: number,
}
```

- POST `/assets/selltoken`

```js
request:
{
  mint: string,
}
```

- GET `/assets/:mint`

```js
{
  success: boolean,
  data: {
    tokenData: {
      mint: string,
      tokenName: string,
      tokenSymbol: string,
      tokenImage: string,
      tokenCreateTime: number,
      investedAmount: number,
      investedPrice_usd: number,
      investedMC_usd: number,
      investedAmount_usd: number,
      currentAmount: number,
      currentPrice_usd: number,
      currentMC_usd: number,
      fdv: number,
      pnl: {
        profit_usd: number,
        percent: number,
      },
      holding: {
        value_usd: number,
      },
      sellingStep: number,
      realisedProfit: number,
      unRealizedProfit: number,
      totalFee: number,
    },
    txData: [
      {
        txHash: string;
        mint: string;
        txTime: number;
        tokenName: string;
        tokenSymbol: string;
        tokenImage: string;
        swap: "BUY" | "SELL";
        swapPrice_usd: number;
        swapAmount: number;
        swapSolAmount: number;
        swapFee_usd: number;
        swapMC_usd: number;
        swapProfit_usd: number;
        swapProfitPercent_usd: number;
        date: number;
      }
    ]
  },
}
```

## Transactions

- GET `/transactions/`

```js
{
  total: number,
  offset: number,
  limit: number,
  sortField: string,
  sortOrder: "asc" | "desc",
  data: [
    {
      txHash: string,
      mint: string,
      txTime: number,
      tokenName: number,
      tokenSymbol: number,
      tokenImage: number,
      swap: "BUY" | "SELL",
      swapPrice_usd: number,
      swapAmount: number,
      swapFee_usd: number,
      swapMC_usd: number,
      currentMC_usd: number,
      swapProfit_usd: number,
      swapProfitPercent_usd: number,
      date: number,
    }
  ],
}
```

- GET `/transactions/getall`

```js
response: (200)[
  {
    txHash: string,
    mint: string,
    txTime: number,
    tokenName: number,
    tokenSymbol: number,
    tokenImage: number,
    swap: "BUY" | "SELL",
    swapPrice_usd: number,
    swapAmount: number,
    swapFee_usd: number,
    swapMC_usd: number,
    currentMC_usd: number,
    swapProfit_usd: number,
    swapProfitPercent_usd: number,
    date: number,
  }
];
```

## Logs

- GET `/logs/all`
  {
  logs: string[],
  }
- GET `/logs/info`

```js
response: 200
{
  msg: "Current Bot Status",
  isSniping: boolean,
  isRunning: boolean,
  isWorkingTime: boolean,
  workingHours: string,
}
```

- GET `/logs/remove`

```js
response: 200;
{
  message: "Logs removed successfully";
}
```



## Version 1.0, 29/1/2025

## Contact Info

- [Telegram](https://t.me/plzbugmenot)
- [github](https://github.com/plzbugmenot)
