# ⚡ Flap Futures

<p align="center">
  <img src="dist/public/assets/flapfutureslogo_nobg-BQ0fQ1JI.png" alt="Flap Futures Logo" width="180"/>
</p>

<p align="center">
  <strong>Decentralized perpetual futures trading platform on Binance Smart Chain</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-20+-339933?style=flat-square&logo=node.js&logoColor=white"/>
  <img src="https://img.shields.io/badge/TypeScript-5.6-3178C6?style=flat-square&logo=typescript&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostgreSQL-14+-4169E1?style=flat-square&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/BSC-BNB_Chain-F0B90B?style=flat-square&logo=binance&logoColor=white"/>
  <img src="https://img.shields.io/badge/Gate.io-Perps-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
</p>

---

## 🌐 Live Demo

**[https://flapfutures.com](https://flapfutures.com)**

---

## 📖 Overview

Flap Futures is a full-featured DeFi trading platform offering three distinct trading modes on BNB Smart Chain:

- **Spot Trading** — Direct token swaps routed through PancakeSwap V2 with a custom-built interface, real-time charting, and auto-slippage control
- **Exchange Perps** — Perpetual futures powered by Gate.io API with live order books and position management
- **FFX Futures** — Fully on-chain isolated perpetual markets, deployed on-demand via a clone factory architecture

Fully bilingual (English / 中文) with MetaMask, Trust Wallet, TokenPocket, Coinbase Wallet, Binance Wallet, and WalletConnect support.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔄 **Spot Swap** | Real-time swaps via PancakeSwap V2 with approval flow, slippage settings, live price quotes |
| 📈 **Exchange Perps** | Long/short perpetuals via Gate.io with live funding rates and order management |
| 🏗️ **FFX Futures** | On-chain perpetual markets — anyone can launch a new market with custom collateral and oracle |
| 📊 **Real-time Charts** | Candlestick charts powered by lightweight-charts with TradingView-style UX |
| 👛 **Multi-wallet** | MetaMask, Trust, TokenPocket, Coinbase, Binance, WalletConnect, OKX, SafePal |
| 🌍 **Bilingual** | Full EN / ZH translation across all pages |
| 🏆 **Leaderboard** | On-chain trader rankings by PnL |
| 📋 **Admin Panel** | Password-protected dashboard for platform management |

---

## 🏗️ Smart Contracts (FFX Futures)

All on-chain perpetual markets use an EIP-1167 clone factory architecture. Each deployed market gets four fully isolated contracts:

| Contract | Role |
|---|---|
| **FFXFactory** | Deploys markets via `CREATE2` clone, routes all creator operations |
| **FFXVault** | Per-market USDT vault — holds creator deposit and trader collateral |
| **FFXInsurance** | Per-market insurance fund — covers bad debt from liquidations |
| **FFXOracle** | Per-market price oracle — updated by an automated price bot |
| **FFXPerps** | Perpetuals engine — open, close, and liquidate positions |

Each market is fully isolated — no shared state, no cross-market risk.

---

## 🛣️ Pages

| Route | Description |
|---|---|
| `/` | Landing page |
| `/dashboard/spot` | Spot trading (PancakeSwap) |
| `/dashboard/perps` | FFX on-chain perpetuals |
| `/dashboard/exchange-perps` | Gate.io exchange perpetuals |
| `/dashboard/balance` | Wallet balance overview |
| `/dashboard/gainers` | Top gaining markets |
| `/dashboard/leaderboard` | Trader leaderboard |
| `/dashboard/create-market` | Launch a new perpetual market |
| `/whitepaper` | Platform documentation |

---

## ⚙️ Environment Variables

Create a `.env` file with the following keys:

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `SESSION_SECRET` | Express session secret |
| `DEV88_PASSWORD` | Admin panel password |
| `BOT_PRIVATE_KEY` | Wallet key for oracle price updates |
| `GATEIO_API_KEY` | Gate.io API key |
| `GATEIO_API_SECRET` | Gate.io API secret |
| `MORALIS_API_KEY` | Moralis API key for token data |
| `BSCSCAN_API_KEY` | BSCScan API key |
| `DEXSCREENER_API_KEY` | DexScreener API key |
| `PORT` | Server port (default: 5000) |

> ⚠️ Never commit `.env` to version control.

---

## 🚀 Getting Started

### Prerequisites

- Node.js 20+
- PostgreSQL 14+

### Install & Run

```bash
# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your values

# Run database migrations
npx drizzle-kit push

# Start production server
node dist/index.cjs
```

The server starts on `http://localhost:5000` by default.

---

## 🖥️ Deployment

### PM2 (Recommended)

```bash
# Create startup script
cat > start.sh << 'EOF'
#!/bin/bash
set -a
source /path/to/.env
set +a
exec node /path/to/dist/index.cjs
EOF
chmod +x start.sh

pm2 start start.sh --name flapfutures
pm2 save
pm2 startup
```

### Nginx Reverse Proxy

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_read_timeout 120s;
    }
}
```

Add SSL:

```bash
certbot --nginx -d yourdomain.com
```

---

## 🤖 Price Bot

An automated price bot runs on a configurable tick interval. It:

1. Fetches live market prices from DexScreener and Moralis
2. Pushes updated prices on-chain to each FFXOracle contract
3. Triggers automated liquidations for underwater positions

Configure refresh intervals and monitor status from the admin panel.

---

## 📄 License

MIT © [Flap Futures](https://flapfutures.com)
