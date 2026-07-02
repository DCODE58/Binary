# 🔥 Prometheus – Free Multi‑Exchange Chart Analysis & Automation Bot

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://python.org)
[![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey)]()

**Prometheus** is a free, open‑source automation bot that harnesses the power of **Binance** and **OKX** APIs to analyse charts, generate trading signals, and execute strategies – all from a single, unified interface.  
Built with Python, it offers real‑time market scanning, advanced technical analysis, backtesting, and fully automated execution for spot and futures markets.

---

> ⚠️ **Risk Warning**  
> Trading cryptocurrencies carries a high level of risk and may not be suitable for all investors.  
> Prometheus is provided for free testing to see your yield in investment. Never trade with money you cannot afford to lose. The developers assume no liability for any financial losses.

---

## ✨ Key Features

- **Multi‑Exchange Support** – Works seamlessly with **Binance** (Spot, Futures) and **OKX** (Spot, Perpetual Swaps). Add more exchanges via a modular adapter.
- **Real‑Time Chart Analysis** – WebSocket streams for tick‑by‑tick data and 1‑minute OHLCV candles.
- **25+ Built‑in Indicators** – RSI, MACD, Bollinger Bands, Ichimoku, EMA crossovers, VWAP, Stochastic, ATR and more.
- **Fully Customisable Strategies** – Write your own Python logic using a simple `BaseStrategy` class. Combine multiple indicators, timeframes, and conditions.
- **Automated Execution** – Convert signals into live orders with configurable position sizing, stop‑loss, take‑profit, and trailing stops.
- **Backtesting Engine** – Test any strategy against historical data from both exchanges. Generates detailed performance reports (Sharpe ratio, drawdown, win rate).
- **Multi‑Symbol Scanner** – Monitor dozens of pairs simultaneously and only fire when conditions are met.
- **Smart Risk Management** – Global daily loss limit, max concurrent trades, maximum drawdown protection, and per‑trade risk percentage.
- **Notifications** – Instant alerts via Telegram, Discord, or Email on trade entry, exit, and errors.
- **Paper Trading** – Simulate strategies in a risk‑free environment using real exchange data.
- **Portfolio Dashboard** – Live overview of balances, open positions, and P&L across all connected exchanges.
- **Lightweight & Docker‑Ready** – Single command setup; runs 24/7 on a VPS, Raspberry Pi, or local machine.

---

## 🧰 Tech Stack

- **Language:** Python 3.9+
- **Exchange APIs:** `python-binance`, `python-okx` (official / community libraries)
- **Data Analysis:** `pandas`, `numpy`, `ta` (Technical Analysis Library)
- **Real‑Time Data:** `websocket-client`, `asyncio`
- **Configuration:** YAML‑based, easily editable
- **Deployment:** Docker support included

---

## 📦 Installation

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/prometheus.git
cd prometheus
```

### 2. Set up a virtual environment (recommended)
```bash
python -m venv venv
source venv/bin/activate    # Linux/macOS
venv\Scripts\activate       # Windows
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure your exchanges and strategies
```bash
cp config.example.yaml config.yaml
```
Open `config.yaml` and fill in your API credentials (Binance and/or OKX), along with your preferred trading parameters.

---

## ⚙️ Configuration

All bot behaviour is controlled by a single `config.yaml` file. Below is a minimal example.

```yaml
exchanges:
  binance:
    enabled: true
    api_key: "YOUR_BINANCE_API_KEY"
    api_secret: "YOUR_BINANCE_API_SECRET"
    testnet: false            # Use testnet for paper trading
    futures: true             # Enable Futures; set false for Spot
  okx:
    enabled: false
    api_key: "YOUR_OKX_API_KEY"
    api_secret: "YOUR_OKX_API_SECRET"
    passphrase: "YOUR_OKX_PASSPHRASE"
    demo: true

global_settings:
  mode: "paper"               # "paper", "live"
  base_quote: "USDT"          # Quote currency for all pairs
  max_open_trades: 5
  max_daily_loss: 200         # In quote currency
  risk_per_trade: 1.0         # % of balance per trade

strategies:
  active: ["ema_crossover", "rsi_scalp"]
  # Each strategy can override its own symbol list and timeframe
  ema_crossover:
    symbols: ["BTC/USDT", "ETH/USDT"]
    timeframe: "15m"
    fast_ema: 9
    slow_ema: 21
    trade_direction: "long"   # "long", "short", "both"
  rsi_scalp:
    symbols: ["SOL/USDT", "DOGE/USDT"]
    timeframe: "5m"
    rsi_period: 7
    oversold: 30
    overbought: 70

notifications:
  telegram:
    enabled: false
    bot_token: "YOUR_TELEGRAM_BOT_TOKEN"
    chat_id: "YOUR_CHAT_ID"
  discord:
    enabled: false
    webhook_url: "YOUR_DISCORD_WEBHOOK"
  email:
    enabled: false
    smtp_server: "smtp.gmail.com"
    smtp_port: 587
    username: "you@example.com"
    password: "your_app_password"
    recipient: "alerts@example.com"

logging:
  level: "INFO"
  log_file: "prometheus.log"
  trade_journal: "journal.csv"
```

---

## 🚀 Usage

### Paper Trading (Safe Simulation)
```bash
python prometheus.py --mode paper
```
All signals are executed virtually. Perfect for testing strategies without any risk.

### Live Trading (Real Money – Use with Caution!)
```bash
python prometheus.py --mode live
```

### Run a Single Strategy on a Specific Pair
```bash
python prometheus.py --strategy ema_crossover --symbol BTC/USDT
```

### Backtesting
Evaluate a strategy on historical data from Binance or OKX:
```bash
python backtest.py --strategy ema_crossover --symbol ETH/USDT --timeframe 1h --from 2024-06-01 --to 2025-01-01
```
Results are saved to `backtest_results/` and include a chart (if matplotlib is installed).

### Multi‑Symbol Scanner
Monitor the entire market for sudden volume spikes or breakout patterns:
```bash
python scanner.py --exchange binance --quote USDT --min_volume 500000
```

---

## 📁 Project Structure

```
prometheus/
├── prometheus.py             # Main entry point
├── backtest.py               # Historical backtesting tool
├── scanner.py                # Market scanner
├── config.example.yaml       # Configuration template
├── requirements.txt
├── Dockerfile
├── strategies/               # User‑defined strategy files
│   ├── base_strategy.py
│   ├── ema_crossover.py
│   ├── rsi_scalp.py
│   └── ...
├── core/                     # Core engine
│   ├── exchange_handler.py   # Unified interface for Binance & OKX
│   ├── risk_manager.py
│   ├── data_feed.py          # Real‑time and historical data
│   ├── portfolio.py
│   └── notifier.py           # Telegram, Discord, Email
├── indicators/               # Custom indicator calculations
├── utils/                    # Helpers, logging, etc.
└── tests/                    # Unit and integration tests
```

---

## 🧪 Writing Your Own Strategy

1. Create a new file in `strategies/` (e.g., `my_strategy.py`).
2. Subclass `BaseStrategy` and implement the `on_candle(self, df)` method.
3. The method receives a pandas DataFrame with OHLCV and your chosen indicators.
4. Return `"BUY"`, `"SELL"`, or `None` to trigger an order (or no action).

Example:
```python
from strategies.base_strategy import BaseStrategy

class MyStrategy(BaseStrategy):
    def on_candle(self, df):
        # Buy when RSI < 30 and price above 50-EMA
        last = df.iloc[-1]
        if last['rsi'] < 30 and last['close'] > last['ema_50']:
            return "BUY"
        # Sell when RSI > 70
        if last['rsi'] > 70:
            return "SELL"
        return None
```

Add your strategy to `config.yaml` under `strategies.active`.

---

## 🐳 Running with Docker

```bash
docker build -t prometheus .
docker run -d --name prometheus -v $(pwd)/config.yaml:/app/config.yaml prometheus
```

---

## 🤝 Contributing

Prometheus is a community‑driven project. Contributions, bug reports, and feature requests are highly welcome!  
Please read our [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

---

## 📜 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

---

## ⭐ Support & Community

- 💬 Join our [Discord server](https://discord.gg/example) (coming soon)
- 🐛 Report issues: [GitHub Issues](https://github.com/yourusername/prometheus/issues)
- 📧 Direct contact: prometheus@gmail.com (no financial advice)

---

**Prometheus** – *Bring fire to your trading.*
