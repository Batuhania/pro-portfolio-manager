<div align="center">

# Pro Portfolio Manager

**A real-time, multi-asset portfolio tracking system with quantitative risk analysis, AI-driven insights, and institutional-grade visualization — built as a zero-dependency Progressive Web App.**

[![Version](https://img.shields.io/badge/v3.0-stable-0A84FF?style=flat-square)](https://github.com/batuhania/pro-portfolio-manager)
[![PWA](https://img.shields.io/badge/PWA-installable-00E676?style=flat-square&logo=pwa&logoColor=white)](https://github.com/batuhania/pro-portfolio-manager)
[![JS](https://img.shields.io/badge/vanilla_JS-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](https://github.com/batuhania/pro-portfolio-manager)
[![Supabase](https://img.shields.io/badge/Supabase-realtime-3ECF8E?style=flat-square&logo=supabase&logoColor=white)](https://supabase.com)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--3.5-412991?style=flat-square&logo=openai&logoColor=white)](https://openai.com)

</div>

---

## Overview

Pro Portfolio Manager is a self-hosted wealth tracking application purpose-built for the Turkish financial ecosystem. It consolidates BIST equities, cryptocurrencies, precious metals, fiat currencies, real estate, vehicles, and liabilities into a single, real-time dashboard — enabling individual investors to monitor net worth, assess portfolio risk, and generate AI-powered investment commentary.

The application runs entirely in the browser with no build step, no framework overhead, and no server-side rendering. All sensitive credentials (API keys) are stored exclusively on the user's device via `localStorage`, and the codebase contains zero hardcoded secrets.

### Why This Exists

Retail investors in Turkey face a fragmented landscape: BIST positions live in one brokerage app, crypto in another, gold prices on a third — with no unified view of total exposure or risk-adjusted performance. This project bridges that gap by providing a consolidated, real-time net worth dashboard with analytical depth typically reserved for institutional platforms.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         CLIENT (Browser)                           │
│                                                                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────────────┐  │
│  │ Portfolio │  │ Analysis │  │ Settings │  │ Service Worker (SW)│  │
│  │   View   │  │Dashboard │  │  Panel   │  │   PWA + Caching    │  │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────────────────────┘  │
│       │              │              │                                │
│  ┌────┴──────────────┴──────────────┴──────┐                        │
│  │           Application Layer             │                        │
│  │  App.init() → Data.fetchAll() → Render  │                        │
│  └────────────────┬────────────────────────┘                        │
│                   │                                                  │
│  ┌────────────────┴────────────────────────┐                        │
│  │            Analytics Engine             │                        │
│  │                                         │                        │
│  │  • Risk Scoring (weighted by asset)     │                        │
│  │  • Real Return (inflation-adjusted)     │                        │
│  │  • Sector Heatmap Distribution          │                        │
│  │  • Period Stats (1D / 1W / 1M)          │                        │
│  │  • Benchmark Comparison                 │                        │
│  │  • Dividend Calendar                    │                        │
│  └─────────────────────────────────────────┘                        │
│                   │                                                  │
│  ┌────────────────┴───────────────┐                                 │
│  │       localStorage             │                                 │
│  │  Assets · History · API Keys   │                                 │
│  └────────────────────────────────┘                                 │
└──────────┬──────────────────────────────────┬───────────────────────┘
           │                                  │
    ┌──────┴──────┐                   ┌───────┴──────────┐
    │  Supabase   │                   │   External APIs  │
    │  REST API   │                   │                  │
    │             │                   │  • ExchangeRate  │
    │  • Assets   │                   │    (USD/TRY,     │
    │  • Prices   │                   │     EUR/TRY)     │
    │  • Dividends│                   │                  │
    └─────────────┘                   │  • OpenAI GPT    │
                                      │    (AI Analysis) │
                                      └──────────────────┘
```

### Data Pipeline

1. **Primary Source** — Asset prices are fetched from a user-configured Supabase instance via REST API (`/rest/v1/assets?select=*`). This single query returns all tickers, current prices, and dividend metadata.

2. **FX Fallback** — If the primary source lacks USD/TRY or EUR/TRY rates, the system falls back to `api.exchangerate-api.com` and derives cross-rates (EUR/TRY = 1/EUR×USD×TRY).

3. **Derived Pricing** — Gram gold (GLDGR) is calculated from ounce gold: `(XAU × USD/TRY) ÷ 31.1035`, following the troy ounce standard.

4. **Auto-Refresh** — The price engine runs on a 10-second polling interval, re-rendering the UI and checking price alerts on each cycle.

---

## Features

### Portfolio Management
- **Multi-asset support** — BIST equities (30+ tickers), cryptocurrencies (20+), precious metals, fiat currencies, investment funds, real estate, vehicles, and debt instruments
- **Smart search** — Fuzzy-matched autocomplete across 60+ pre-defined instruments, with support for custom/manual entries
- **Per-asset price history** — Track historical prices over time with inline charts
- **Price alerts** — Set target prices with browser notification support
- **Scenario calculator** — "What if this asset reaches X?" profit simulation
- **Privacy mode** — One-tap balance masking across the entire UI

### Quantitative Analysis
- **Risk Score (0–10)** — Weighted composite score based on asset allocation:
  - Crypto: 10 | Equities: 7 | Precious Metals: 2 | FX: 2 | Stablecoins: 1
- **Real Return** — Inflation-adjusted weekly performance using the Fisher equation:
  ```
  Real = ((1 + Nominal%) / (1 + Inflation%)) - 1
  ```
  Baseline: 60% annual → ~0.9% weekly
- **Sector Heatmap** — Visual representation of portfolio concentration by industry sector (BANKA, ENERJI, METAL, SAVUNMA, etc.)
- **Benchmark Comparison** — Weekly performance ranked against USD, gold, and inflation
- **Period Statistics** — P&L tracking across 1-day, 7-day, and 30-day windows with 90-day historical depth

### AI-Powered Insights
- **GPT-3.5 Integration** — Generates concise, token-efficient portfolio commentary based on:
  - Risk score, weekly delta, top 3 holdings, sector breakdown
- **Manual trigger** — AI analysis runs on-demand to minimize API costs
- **Turkish-language output** — Responses are contextual and locale-aware

### Visualization
- **Doughnut charts** — Asset class distribution (equities, crypto, gold, cash, funds)
- **Growth line chart** — 90-day historical portfolio value with filled area
- **Sector heatmap** — Color-coded blocks sized by allocation weight
- **Live ticker** — Scrolling marquee with BIST-100, USD, EUR, gold, and BTC prices

### Data Management
- **JSON backup** — Full asset data export
- **CSV/Excel export** — Semi-colon delimited with BOM header for Turkish locale compatibility
- **URL-based configuration** — Auto-configure API keys via query parameters: `?sburl=...&sbkey=...&aikey=...`
- **Factory reset** — Full `localStorage` wipe with confirmation

---

## Supported Instruments

| Category | Examples | Pricing |
|----------|----------|---------|
| **BIST Equities** | THYAO, EREGL, ASELS, GARAN, KCHOL, SAHOL, BIMAS, FROTO, TOASO, SASA + 20 more | Supabase |
| **Crypto** | BTC, ETH, SOL, XRP, BNB, AVAX, ADA, DOGE, SHIB, DOT, MATIC, LTC, LINK, UNI, PEPE, FET | Supabase + USD/TRY conversion |
| **Precious Metals** | Gram Gold (GLDGR), Gram Silver (SLVGR), Gram Platinum (PLTGR), Ounce Gold (XAU) | Derived from XAU × FX |
| **Currencies** | USD/TRY, EUR/TRY, GBP/TRY, CHF/TRY | Supabase + FX fallback |
| **Cash & Funds** | TRY, USD, EUR cash positions, investment funds | Manual / API |
| **Fixed Assets** | Real estate, vehicles, land, collectibles, watches | Manual entry |
| **Liabilities** | Loans, credit card debt | Manual (negative net worth) |

---

## Getting Started

### Prerequisites

| Requirement | Purpose |
|-------------|---------|
| Modern browser | Chrome, Safari, Firefox, Edge |
| [Supabase](https://supabase.com) account (free tier) | Real-time price data backend |
| OpenAI API key *(optional)* | AI portfolio commentary |

### 1. Clone & Open

```bash
git clone https://github.com/batuhania/pro-portfolio-manager.git
cd pro-portfolio-manager
```

Open `index.html` directly in a browser — no build step, no dev server, no `npm install`.

### 2. Configure Supabase

Create an `assets` table in your Supabase project:

```sql
CREATE TABLE assets (
    id          BIGSERIAL PRIMARY KEY,
    ticker      TEXT NOT NULL,
    current_price NUMERIC DEFAULT 0,
    asset_type  TEXT DEFAULT 'STOCK',
    name        TEXT
);

-- Dividend entries use asset_type = 'DIVIDEND_INFO'
-- The 'name' field stores the dividend date (YYYY-MM-DD)
```

Then populate it with a scheduled job or external price feed.

### 3. Connect the App

Navigate to **Settings** in the bottom navigation and enter:

| Field | Value |
|-------|-------|
| Supabase URL | `https://your-project.supabase.co` |
| Supabase Key | Your `anon` / `public` key |
| OpenAI Key | *(optional)* `sk-...` |

Click **Save & Reload**. All credentials are stored in `localStorage` — nothing is transmitted beyond the configured API endpoints.

**Quick config via URL:**
```
index.html?sburl=https://xxx.supabase.co&sbkey=eyJ...&aikey=sk-...
```
Parameters are saved and then stripped from the URL for security.

### 4. Install as PWA

| Platform | Steps |
|----------|-------|
| **iOS** | Safari → Share (↑) → "Add to Home Screen" |
| **Android** | Chrome → Menu (⋮) → "Install app" or "Add to home screen" |
| **Desktop** | Chrome → Address bar → Install icon |

---

## Tech Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| **Runtime** | Vanilla ES6+ | Zero build overhead, no framework lock-in, instant load |
| **Data Layer** | Supabase (PostgreSQL + REST) | Free tier, real-time capable, row-level security |
| **Charting** | Chart.js | Lightweight, responsive, touch-friendly |
| **Icons** | Font Awesome 6 | Comprehensive financial icon set |
| **Typography** | Inter + JetBrains Mono | Optimized for data-dense interfaces |
| **PWA** | Service Worker + Manifest | Offline-capable, installable, native-like experience |
| **AI** | OpenAI GPT-3.5 Turbo | Cost-effective, fast inference for portfolio commentary |

---

## Project Structure

```
pro-portfolio-manager/
├── index.html          # Single-file application (HTML + CSS + JS)
├── manifest.json       # PWA manifest (app name, icons, orientation)
├── sw.js              # Service worker for offline caching
├── .gitignore
└── README.md
```

> The entire application ships as a single HTML file — no bundler, no transpiler, no node_modules. This is a deliberate architectural choice prioritizing deployment simplicity and long-term maintainability over framework convenience.

---

## License

All rights reserved. © 2026 Batuhan.

<div align="center">

![Footer](https://capsule-render.vercel.app/api?type=waving&color=gradient&height=60&section=footer)

</div>
