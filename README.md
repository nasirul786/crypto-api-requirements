# Api Requirements

### 1. Get supported networks, tokens, addresses, network fee and network fee in each currency

**Example Request**

```GET https://example.com/api/getme```

**Example Response**
```json
{
  "ok": true, // true if request successful, false if failed
  "message": "Wallet balances fetched successfully", // response message
  "data": [
    {
      "network": "BEP20", // blockchain network
      "address": "0x0000000000000000000", // wallet address
      "tokens": [
        {
          "token": "USDT", // token symbol
          "balance": 100.00000 // available balance
        },
        {
          "token": "BNB",
          "balance": 0.50000
        },
        {
          "token": "USDC",
          "balance": 0.00000
        },
        {
          "token": "BUSD",
          "balance": 25.75000
        }
      ],
      "fee": {
        "fee": 0.000063, // estimated network fee
        "fee_token": "BNB", // token used for paying fee
        "fee_in_usdt": 0.041, // fee converted into USDT value
        "available": 0.50000 // available balance of fee token
      }
    },
    {
      "network": "TRC20",
      "address": "TRX9aBcD1234567890",
      "tokens": [
        {
          "token": "USDT",
          "balance": 250.00000
        },
        {
          "token": "TRX",
          "balance": 120.00000
        },
        {
          "token": "JST",
          "balance": 40.50000
        },
        {
          "token": "SUN",
          "balance": 0.00000
        }
      ],
      "fee": {
        "fee": 1.200000,
        "fee_token": "TRX",
        "fee_in_usdt": 0.324,
        "available": 120.00000
      }
    },
    {
      "network": "ERC20",
      "address": "0xABCDEF1234567890ABCDEF",
      "tokens": [
        {
          "token": "USDT",
          "balance": 75.00000
        },
        {
          "token": "ETH",
          "balance": 0.03500
        },
        {
          "token": "USDC",
          "balance": 18.25000
        },
        {
          "token": "DAI",
          "balance": 50.00000
        }
      ],
      "fee": {
        "fee": 0.000420,
        "fee_token": "ETH",
        "fee_in_usdt": 1.120,
        "available": 0.03500
      }
    }
  ]
}
```

### 2. Get deposits history of a network:


**Example Request**

```GET https://example.com/api/get-deposits?network=BEP20&token=USDT&from_address=0x000000```

| Parameter      | Description                                          | Required |
| -------------- | ---------------------------------------------------- | -------- |
| `network`      | Blockchain network name (e.g. BEP20, TRC20, ERC20)   | `true`   |
| `token`        | Token symbol to filter history data (e.g. USDT, BNB) | `false`  |
| `from_address` | Wallet address to fetch deposits from                | `false`  |


**Example Response**
```json
{
  "ok": true, // true if request successful
  "message": "Deposit history fetched successfully",
  "data": [
    {
      "network": "BEP20", // blockchain network
      "token": "USDT", // deposited token
      "amount": 125.50000, // deposited amount
      "timestamp": 1779452100, // unix timestamp
      "from_address": "0xA1B2C3D4E5F678901234567890ABCDEF12345678", // sender wallet
      "to_address": "0x99887766554433221100AABBCCDDEEFF11223344", // receiver wallet
      "transaction_hash": "0x8f2f6b52bcb1f8a4b2f9981c4aefab1234567890abcdef1234567890abcdef",
      "block": 43891221, // blockchain block number
      "confirmations": 18, // total confirmations
      "status": "completed", // completed, pending, failed
      "fee": 0.000052, // network fee
      "fee_token": "BNB"
    },
    {
      "network": "TRC20",
      "token": "USDT",
      "amount": 50.00000,
      "timestamp": 1779448700,
      "from_address": "TRx9ABCD1234567890XYZ",
      "to_address": "TRx7QWER9876543210LMN",
      "transaction_hash": "4b8dbe7c3f8f1234567890abcdef1234567890abcdef1234567890abcdef12",
      "block": 68452111,
      "confirmations": 32,
      "status": "completed",
      "fee": 1.000000,
      "fee_token": "TRX"
    },
    {
      "network": "ERC20",
      "token": "ETH",
      "amount": 0.25000,
      "timestamp": 1779430000,
      "from_address": "0x1234567890ABCDEF1234567890ABCDEF12345678",
      "to_address": "0xABCDEF1234567890ABCDEF1234567890ABCDEF12",
      "transaction_hash": "0x9bcdef1234567890abcdef1234567890abcdef1234567890abcdef12345678",
      "block": 22451098,
      "confirmations": 7,
      "status": "pending",
      "fee": 0.000410,
      "fee_token": "ETH"
    }
  ]
}
```

### 3. Withdraw Token
Will be used to withdraw a token to another wallet

**Example Request**

```POST https://example.com/api/withdraw```

**Request Body**
```json
{
    "address": "0x0000000", // withdraw to adress,
    "amount": 100.0000,
    "network": "BEP20"
}
```

**Example Response**
```json
{
  "ok": true, // true if withdraw request accepted
  "message": "Withdraw successful, will be credited after blockchain processing",
  "data": {
    "withdraw_id": "WD78219451", // unique withdraw request id
    "network": "BEP20", // selected blockchain network
    "token": "USDT", // withdrawn token
    "address": "0x0000000", // destination wallet address
    "amount": 100.0000, // requested withdraw amount
    "fee": 0.8000, // withdraw fee
    "fee_token": "BNB", // token used for fee
    "status": "pending", // pending, completed, failed
    "timestamp": 1779454500, // unix timestamp
    "transaction_hash": "0x9bcdef1234567890abcdef1234567890abcdef1234567890abcdef12345678" // transaction hash
  }
}
```

### 4. Get withdrawal info:
**Example Request**
``` GET https://example.com/api/withdrawal/withdraw_id```

**Example Response**
```json
{
    "ok": true, //or false,
    "message": "Fetched",
    "data": {
        "withdraw_id": "WD78219451", // unique withdraw request id
        "network": "BEP20", // selected blockchain network
        "token": "USDT", // withdrawn token
        "address": "0x0000000", // destination wallet address
        "amount": 100.0000, // requested withdraw amount
        "fee": 0.8000, // withdraw fee
        "fee_token": "BNB", // token used for fee
        "status": "completed", // pending, completed, failed
        "timestamp": 1779454500, // unix timestamp
        "transaction_hash": "0x9bcdef1234567890abcdef1234567890abcdef1234567890abcdef12345678" // transaction hash
   }
}
```
