
---

# 🔥 Prometheus – Free Multi‑Exchange Chart Analysis & Automation Bot

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Installations](https://img.shields.io/badge/installations-25.6k-brightgreen)]()
[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://python.org)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey)]()


**Prometheus** is a free, open‑source automation bot that harnesses the power of **Binance** and **OKX** APIs to analyse charts, generate trading signals, and execute strategies.  
It offers real‑time market scanning, advanced technical analysis, backtesting, and fully automated execution – all without writing a single line of code if you use the ready‑to‑run executable.

---

> ⚠️ **Risk Warning**  
> Trading cryptocurrencies carries a high level of risk and may not be suitable for all investors.  
> Prometheus is provided **for educational and research purposes only**. Never trade with money you cannot afford to lose. The developers assume no liability for any financial losses.

---

## 🚀 Quick Start for Everyone (No Technical Skills Required)

We provide a self‑contained **executable file** that bundles Python and all dependencies. You don’t need to install anything – just download, configure, and run.

### 1. Download the Executable
- **Windows:** [Prometheus‑win64.exe](#) (latest version)
- **macOS:** [Prometheus‑macOS.dmg](#) (coming soon)
- **Linux:** [Prometheus‑linux.AppImage](#) (coming soon)

> 📁 The download includes the bot and a default `config.yaml` file. Extract the `.zip` file to a folder of your choice (e.g., `C:\Prometheus`).

### 2. Edit Your Configuration
1. Open the folder and locate `config.yaml`.
2. Right‑click → **Open with** → Notepad (or any text editor).
3. Fill in your **exchange API keys** and set your strategy preferences.  
   *Detailed guidance is inside the `config.yaml` itself – just follow the comments.*
4. Save the file.

### 3. Run Prometheus
- **Windows:** Double‑click `prometheus.exe`. A terminal window will open showing real‑time logs.  
  *If SmartScreen blocks it, click **“More info”** → **“Run anyway”**.*
- **macOS/Linux:** Right‑click the app and select **Open** (macOS) or run from terminal (`./prometheus`).

### 4. Start Paper Trading
By default, Prometheus runs in **paper trading mode** (no real money at risk).  
Watch it scan the market, analyse charts, and show signals. When you’re ready for live trading, change `mode` to `live` in `config.yaml` and restart.

> 💡 **First time?** Stick with paper trading until you fully understand how your strategy behaves.

---

*If you’re a developer and prefer to run from source, see the [Developer Installation](#-developer-installation-source-code) section below.*

---

## ✨ Key Features

- **Multi‑Exchange Support** – Binance (Spot, Futures) & OKX (Spot, Perpetual Swaps)
- **Real‑Time Chart Analysis** – Live WebSocket data, 1‑minute OHLCV candles
- **25+ Built‑in Indicators** – RSI, MACD, Bollinger Bands, Ichimoku, EMA crossovers, VWAP…
- **Fully Customisable Strategies** – Choose from presets or create your own (simple Python class)
- **Automated Execution** – Configurable position sizing, stop‑loss, take‑profit, trailing stops
- **Backtesting Engine** – Test any strategy on historical data, view full performance reports
- **Multi‑Symbol Scanner** – Monitor dozens of pairs at once
- **Smart Risk Management** – Daily loss limit, max open trades, per‑trade risk %
- **Notifications** – Telegram, Discord, Email alerts
- **Paper Trading** – Risk‑free simulation with real exchange data
- **Portfolio Dashboard** – Live balances, P&L, open positions across exchanges

---

## ⚙️ Configuration Explained (Quick Overview)

The `config.yaml` file controls everything. Here’s a minimal example:

```yaml
exchanges:
  binance:
    enabled: true
    api_key: "YOUR_API_KEY"
    api_secret: "YOUR_API_SECRET"
    testnet: false        # Use true for Binance testnet
    futures: true
  okx:
    enabled: false

global_settings:
  mode: "paper"           # "paper" or "live"
  base_quote: "USDT"
  max_daily_loss: 200
  risk_per_trade: 1.0     # % of balance per trade

strategies:
  active: ["ema_crossover"]
  ema_crossover:
    symbols: ["BTC/USDT", "ETH/USDT"]
    timeframe: "15m"
    fast_ema: 9
    slow_ema: 21

notifications:
  telegram:
    enabled: false
    bot_token: "YOUR_BOT_TOKEN"
    chat_id: "YOUR_CHAT_ID"
```

Full documentation is in the `docs/` folder (or visit our wiki).

---

## 📦 Developer Installation (Source Code)

If you prefer to work directly with the Python source code:

```bash
git clone https://github.com/yourusername/prometheus.git
cd prometheus
python -m venv venv
source venv/bin/activate     # Linux/macOS
venv\Scripts\activate        # Windows
pip install -r requirements.txt
cp config.example.yaml config.yaml
# Edit config.yaml with your keys
python prometheus.py --mode paper
```

---

## 🧪 Writing a Custom Strategy

1. Create a file in `strategies/` (e.g., `my_strategy.py`).
2. Inherit from `BaseStrategy` and implement `on_candle(self, df)`.
3. Return `"BUY"`, `"SELL"`, or `None`.

```python
class MyStrategy(BaseStrategy):
    def on_candle(self, df):
        last = df.iloc[-1]
        if last['rsi'] < 30 and last['close'] > last['ema_50']:
            return "BUY"
        if last['rsi'] > 70:
            return "SELL"
        return None
```

Add your strategy name to `config.yaml` under `strategies.active`.

---

## 🐳 Docker (Alternative)

```bash
docker build -t prometheus .
docker run -d --name prometheus -v $(pwd)/config.yaml:/app/config.yaml prometheus
```

---

## 🤝 Contributing

Prometheus is community‑driven. Issues, pull requests, and feature suggestions are highly welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📜 License

MIT License – see [LICENSE](LICENSE).

---

## ⭐ Support

- 💬 [Discord Community](https://discord.gg/example) (coming soon)
- 🐛 [Report a Bug](https://github.com/yourusername/prometheus/issues)
- 📧 prometheus@gmail.com (no financial advice)

**Prometheus** – *Bring fire to your trading.*
