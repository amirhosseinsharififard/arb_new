# Aster DEX API Integration Documentation

## 📋 Overview
a
Complete integration with Aster DEX for spot trading, balance management, and market data retrieval.

## 🔧 Configuration

### Environment Variables

```bash
ASTER_API_KEY=your_api_key
ASTER_API_SECRET_KEY=your_secret_key
ASTER_BASE_URL=https://fapi.asterdex.com
ASTER_RPC_URL=https://bsc-dataseed.binance.org/
ASTER_CHAIN_ID=your_chain_id
```

### Default Configuration

```javascript
const config = {
  apiKey: "your_api_key_here",
  secretKey: "your_secret_key_here",
  baseUrl: "https://fapi.asterdex.com",
  rpcUrl: "https://bsc-dataseed.binance.org/",
  chainId: process.env.ASTER_CHAIN_ID,
  sandbox: true,
  testnet: true,
};
```

## 🚀 Core Functions

### API Authentication

```javascript
import { makeRequest } from "./aster.js";

// Create signature for authenticated requests
function createSignature(queryString, secretKey) {
  return crypto
    .createHmac("sha256", secretKey)
    .update(queryString)
    .digest("hex");
}
```

### Account Information

```javascript
import { getAccountInfo, getBalance } from "./aster.js";

// Get account information
const accountInfo = await getAccountInfo();
console.log("Account:", accountInfo);

// Get balance
const balance = await getBalance();
console.log("USDT Balance:", balance.USDT);
```

### Market Data

```javascript
import { getTicker, getOrderBook, getKlines } from "./aster.js";

// Get current price
const ticker = await getTicker("BTCUSDT");
console.log("BTC Price:", ticker.lastPrice);

// Get order book
const orderBook = await getOrderBook("BTCUSDT");
console.log("Bids:", orderBook.bids);
console.log("Asks:", orderBook.asks);

// Get klines/candles
const klines = await getKlines("BTCUSDT", "1h", 100);
console.log("Hourly candles:", klines);
```

### Trading Operations

```javascript
import { createOrder, cancelOrder, getOpenOrders } from "./aster.js";

// Create buy order
const buyOrder = await createOrder(
  "BTCUSDT",
  "BUY",
  "LIMIT",
  0.001, // quantity
  50000, // price
  "GTC" // timeInForce
);

// Create sell order
const sellOrder = await createOrder(
  "BTCUSDT",
  "SELL",
  "MARKET",
  0.001, // quantity
  null, // price (null for market)
  "IOC" // timeInForce
);

// Cancel order
const cancelResult = await cancelOrder(buyOrder.orderId);

// Get open orders
const openOrders = await getOpenOrders("BTCUSDT");
console.log("Open Orders:", openOrders);
```

### Order Management

```javascript
import { getOrderHistory, getTradeHistory } from "./aster.js";

// Get order history
const orderHistory = await getOrderHistory("BTCUSDT", 100);
console.log("Order History:", orderHistory);

// Get trade history
const tradeHistory = await getTradeHistory("BTCUSDT", 100);
console.log("Trade History:", tradeHistory);
```

## 📊 Available Functions

### Account Management

- `getAccountInfo()` - Account information
- `getBalance()` - Account balance
- `getAccountTrades()` - Account trade history

### Market Data

- `getTicker(symbol)` - Current price data
- `getOrderBook(symbol)` - Order book data
- `getKlines(symbol, interval, limit)` - Candlestick data
- `getExchangeInfo()` - Exchange information
- `getServerTime()` - Server timestamp

### Trading

- `createOrder(symbol, side, type, quantity, price, timeInForce)` - Create order
- `cancelOrder(orderId)` - Cancel order
- `cancelAllOrders(symbol)` - Cancel all orders
- `getOpenOrders(symbol)` - Get open orders
- `getOrderHistory(symbol, limit)` - Order history

### Advanced Features

- `getLeverageBracket(symbol)` - Leverage information
- `changeLeverage(symbol, leverage)` - Change leverage
- `getPositionRisk(symbol)` - Position risk
- `getFundingRate(symbol)` - Funding rate

## 🔒 Security Features

### HMAC-SHA256 Signature

```javascript
// Create signature for authenticated requests
function createSignature(queryString, secretKey) {
  return crypto
    .createHmac("sha256", secretKey)
    .update(queryString)
    .digest("hex");
}
```

### Request Authentication

- Timestamp validation
- Signature verification
- Rate limiting
- IP whitelisting support

## ⚠️ Important Notes

### API Limits

- **Rate Limit**: 1200 requests per minute
- **Weight Limits**: Varies by endpoint
- **IP Restrictions**: Configurable

### Order Types

- **LIMIT**: Limit orders
- **MARKET**: Market orders
- **STOP**: Stop orders
- **STOP_MARKET**: Stop market orders

### Time in Force

- **GTC**: Good Till Canceled
- **IOC**: Immediate or Cancel
- **FOK**: Fill or Kill

## 📝 Example Usage

### Complete Trading Flow

```javascript
import { getTicker, createOrder, getOpenOrders, cancelOrder } from "./aster.js";

async function tradeExample() {
  try {
    const symbol = "BTCUSDT";

    // Get current price
    const ticker = await getTicker(symbol);
    const currentPrice = parseFloat(ticker.lastPrice);

    // Create buy order
    const buyOrder = await createOrder(
      symbol,
      "BUY",
      "LIMIT",
      0.001,
      currentPrice * 0.99, // 1% below market
      "GTC"
    );

    console.log("Buy order created:", buyOrder.orderId);

    // Wait for execution
    await new Promise((resolve) => setTimeout(resolve, 5000));

    // Check if filled
    const openOrders = await getOpenOrders(symbol);
    if (openOrders.length === 0) {
      console.log("Order filled successfully");
    } else {
      // Cancel if not filled
      await cancelOrder(buyOrder.orderId);
      console.log("Order canceled");
    }
  } catch (error) {
    console.error("Trading error:", error.message);
  }
}
```

### Portfolio Management

```javascript
import { getBalance, getAccountTrades } from "./aster.js";

async function portfolioExample() {
  try {
    // Get current balance
    const balance = await getBalance();
    console.log("Portfolio Balance:", balance);

    // Get recent trades
    const trades = await getAccountTrades("BTCUSDT", 50);
    console.log("Recent Trades:", trades);

    // Calculate PnL
    let totalPnL = 0;
    trades.forEach((trade) => {
      totalPnL += parseFloat(trade.realizedPnl || 0);
    });

    console.log("Total PnL:", totalPnL);
  } catch (error) {
    console.error("Portfolio error:", error.message);
  }
}
```

## 🔧 Troubleshooting

### Common Issues

1. **"Invalid signature"** - Check API credentials
2. **"Timestamp out of range"** - Sync system clock
3. **"Insufficient balance"** - Check account balance
4. **"Order not found"** - Verify order ID

### Debug Steps

1. Check API credentials
2. Verify timestamp synchronization
3. Test with small amounts
4. Review error messages
5. Check rate limits

### Error Handling

```javascript
try {
  const result = await createOrder(symbol, side, type, quantity, price);
  console.log("Order created:", result);
} catch (error) {
  if (error.code === -1021) {
    console.log("Timestamp error - sync your clock");
  } else if (error.code === -2010) {
    console.log("Insufficient balance");
  } else {
    console.log("Error:", error.message);
  }
}
```

## 📚 API Reference

### Endpoints

- **Base URL**: `https://fapi.asterdex.com`
- **WebSocket**: `wss://fapi.asterdex.com/ws`
- **REST API**: `/fapi/v1/`

### Response Format

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    // Response data
  }
}
```

### Error Codes

- `-1021`: Timestamp out of range
- `-2010`: Insufficient balance
- `-2011`: Order not found
- `-2013`: Invalid symbol

---

**Last Updated**: October 2024
**Version**: 1.0.0
