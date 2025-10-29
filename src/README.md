# Trading Bot Integration Documentation

## 📋 Project Overview

This project provides comprehensive integrations for multiple DEX platforms, focusing on Hyperliquid and Aster for futures and spot trading.

## 🏗️ Project Structure

```
src/
├── hiperliquid/           # Hyperliquid DEX Integration
│   ├── hyperliquid.js     # Core Hyperliquid API functions
│   ├── contracts.js       # Smart contract addresses and ABIs
│   ├── HYPERLIQUID_DOCUMENTATION.md  # Complete Hyperliquid docs
│   ├── AGENT_APPROVAL_GUIDE.md       # EIP712 agent approval guide
│   └── README_CONTRACTS.md           # Contract documentation
├── aster/                 # Aster DEX Integration
│   ├── aster.js          # Core Aster API functions
│   ├── ASTER_DOCUMENTATION.md        # Complete Aster docs
│   └── aster.md          # Basic Aster information
└── arbitrage_funding_rate/  # Arbitrage and funding rate tools
    ├── funding_rate_bot.js  # Funding rate monitoring
    ├── price_comparison_bot.js # Price comparison tools
    └── find_symbols.js      # Symbol discovery
```

## 🚀 Quick Start

### Hyperliquid Integration

```javascript
import {
  getExchangeWithAuth,
  getTicker,
  createFuturesOrder,
  getOpenPositions,
} from "./hiperliquid/hyperliquid.js";

// Initialize exchange
const exchange = getExchangeWithAuth();

// Get market data
const ticker = await getTicker("BTC/USDC:USDC");
console.log("BTC Price:", ticker.last);

// Create position
const order = await createFuturesOrder("BTC/USDC:USDC", "buy", 0.001, 50000);

// Check positions
const positions = await getOpenPositions();
console.log("Open positions:", positions.count);
```

### Aster Integration

```javascript
import { getTicker, createOrder, getBalance } from "./aster/aster.js";

// Get market data
const ticker = await getTicker("BTCUSDT");
console.log("BTC Price:", ticker.lastPrice);

// Create order
const order = await createOrder("BTCUSDT", "BUY", "LIMIT", 0.001, 50000, "GTC");

// Check balance
const balance = await getBalance();
console.log("USDT Balance:", balance.USDT);
```

## 🔧 Configuration

### Environment Variables

Create a `.env` file with the following variables:

```bash
# Hyperliquid Configuration
HYPERLIQUID_API_KEY=your_hyperliquid_api_key
HYPERLIQUID_API_SECRET_KEY=your_hyperliquid_secret
HYPERLIQUID_PRIVATE_KEY=your_private_key
HYPERLIQUID_USER=your_wallet_address
HYPERLIQUID_RPC_URL=https://bsc-dataseed.binance.org/
HYPERLIQUID_CHAIN_ID=0xa4b1

# Aster Configuration
ASTER_API_KEY=your_aster_api_key
ASTER_API_SECRET_KEY=your_aster_secret
ASTER_BASE_URL=https://fapi.asterdex.com
ASTER_RPC_URL=https://bsc-dataseed.binance.org/
ASTER_CHAIN_ID=your_chain_id
```

## 📚 Documentation

### Hyperliquid

- **Complete Documentation**: `src/hiperliquid/HYPERLIQUID_DOCUMENTATION.md`
- **Agent Approval Guide**: `src/hiperliquid/AGENT_APPROVAL_GUIDE.md`
- **Contract Documentation**: `src/hiperliquid/README_CONTRACTS.md`

### Aster

- **Complete Documentation**: `src/aster/ASTER_DOCUMENTATION.md`
- **Basic Information**: `src/aster/aster.md`

## 🔒 Security Features

### Hyperliquid

- EIP712 transaction signing
- Private key management
- Agent approval system
- Secure credential storage

### Aster

- HMAC-SHA256 signature authentication
- Timestamp validation
- Rate limiting
- IP whitelisting support

## ⚠️ Important Notes

### Hyperliquid Requirements

- **Minimum Order Value**: $10
- **Account Balance**: Sufficient for margin requirements
- **Maintenance Margin**: Required for position holding
- **Agent Approval**: Required for trading

### Aster Requirements

- **API Credentials**: Valid API key and secret
- **Rate Limits**: 1200 requests per minute
- **Balance**: Sufficient for trading
- **Timestamp Sync**: Critical for authentication

## 🛠️ Common Issues & Solutions

### Hyperliquid Issues

1. **"Order has zero size"** → Increase order size
2. **"Order must have minimum value of $10"** → Calculate proper size
3. **"Cannot convert mark to a BigInt"** → Check price format
4. **Positions closing immediately** → Check margin requirements

### Aster Issues

1. **"Invalid signature"** → Check API credentials
2. **"Timestamp out of range"** → Sync system clock
3. **"Insufficient balance"** → Check account balance
4. **"Order not found"** → Verify order ID

## 📊 Available Functions

### Hyperliquid Functions

- `getExchange()` / `getExchangeWithAuth()` - Exchange instances
- `checkUser()` - User authentication
- `checkBalance()` - Account balance
- `getTicker(symbol)` - Market data
- `createFuturesOrder()` - Create positions
- `getOpenPositions()` - Position management
- `getPortfolioValue()` - Portfolio valuation

### Aster Functions

- `getTicker(symbol)` - Market data
- `createOrder()` - Create orders
- `getBalance()` - Account balance
- `getOpenOrders()` - Order management
- `getOrderHistory()` - Trade history
- `getAccountInfo()` - Account information

## 🚀 Example Usage

### Complete Trading Bot

```javascript
import {
  getExchangeWithAuth,
  getTicker,
  createFuturesOrder,
} from "./hiperliquid/hyperliquid.js";
import { getTicker as getAsterTicker, createOrder } from "./aster/aster.js";

async function arbitrageBot() {
  try {
    // Get prices from both exchanges
    const hyperliquidPrice = await getTicker("BTC/USDC:USDC");
    const asterPrice = await getAsterTicker("BTCUSDT");

    const priceDiff = Math.abs(hyperliquidPrice.last - asterPrice.lastPrice);
    const threshold = 100; // $100 difference

    if (priceDiff > threshold) {
      console.log(`Arbitrage opportunity: $${priceDiff} difference`);

      // Implement arbitrage logic here
      if (hyperliquidPrice.last < asterPrice.lastPrice) {
        // Buy on Hyperliquid, sell on Aster
        console.log("Buy Hyperliquid, sell Aster");
      } else {
        // Buy on Aster, sell on Hyperliquid
        console.log("Buy Aster, sell Hyperliquid");
      }
    }
  } catch (error) {
    console.error("Arbitrage error:", error.message);
  }
}
```

## 📈 Monitoring & Analytics

### Key Metrics

- **Portfolio Value**: Total account value
- **Open Positions**: Current positions
- **PnL**: Profit and loss
- **Margin Usage**: Margin utilization
- **Order Status**: Order execution status

### Logging

- Comprehensive error logging
- Trade execution logs
- Performance metrics
- Debug information

## 🔧 Development

### Prerequisites

- Node.js 16+
- npm or yarn
- Valid API credentials
- Sufficient account balance

### Installation

```bash
npm install
# or
yarn install
```

### Testing

```bash
# Test Hyperliquid connection
node src/hiperliquid/test_connection.js

# Test Aster connection
node src/aster/test_connection.js
```

## 📞 Support

### Hyperliquid

- **Documentation**: https://hyperliquid.gitbook.io/
- **API Reference**: https://hyperliquid.gitbook.io/hyperliquid/
- **Community**: https://discord.gg/hyperliquid

### Aster

- **Documentation**: https://asterdex.com/docs
- **API Reference**: https://asterdex.com/api
- **Support**: https://asterdex.com/support

---

**Last Updated**: October 2024
**Version**: 1.0.0
**Maintainer**: Trading Bot Team
