# ProPortfoy (v5) 🚀

A robust, privacy-focused Portfolio Tracking Dashboard powered by Python automation and Modern Web Technologies.

## Features

- **Hybrid Asset Tracking:** Stocks (BIST), Crypto, Gold, Forex, Funds, and Fixed Assets (Car, Real Estate).
- **Automated Data:** Python script (`main.py`) fetches real-time data from financial sources.
- **Privacy First:** Data is stored locally in your browser (`localStorage`). No external database required for portfolio data.
- **Analysis Tools:**
    - **Interactive Charts:** Asset distribution (Pie) and History (Line).
    - **Profit/Loss:** Real-time P/L calculation + Unrealized Gains.
    - **Dividend Calendar:** Tracking upcoming dividends.
- **Mobile Ready:** Progressive Web App (PWA) support for iOS & Android.

## Installation

### 1. Web App (Frontend)
Simply open `index.html` (formerly `v5_robotlu.html`) in any modern browser. 
*(No server required for basic usage)*

### 2. Python Robot (Backend Data)
 To enable automatic price updates:

```bash
# Install dependencies
pip install -r requirements.txt

# Run the robot
python main.py
```

*Note: Create a `.env` file with your `SUPABASE_URL` and `SUPABASE_KEY` if using cloud sync features.*

## Tech Stack

- **Frontend:** Vanilla JS, Chart.js, PWA (Manifest/SW).
- **Backend:** Python (yfinance, supabase-py).
- **Storage:** LocalStorage + Supabase (Optional).

## License

MIT License. Free to use and modify.
