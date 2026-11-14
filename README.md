# MarketMate AI v3 — fundamental stock research

## Features
- Goal calculator: investment, risk, horizon, desired profit.
- Fundamental screen: P/E, industry P/E, P/B, debt/equity, ROE, book value, face value.
- Quote and news integrations; AI ranking/explanations; risk notes and scenario arithmetic.
- Import an NSE equity universe CSV.

## Run
Requires Node.js 20+:
```bash
npm install
cp .env.example .env
npm start
```
Open http://localhost:3000.

## API setup
Keep keys in `.env` on the server:
- `OPENAI_API_KEY` and optional `OPENAI_MODEL`
- `TWELVE_DATA_API_KEY` for quote data. The adapter requests `SYMBOL.NSE`; confirm your plan has NSE coverage and real-time entitlements. If your subscription provides delayed data, the website must be labelled delayed.
- `FUNDAMENTALS_API_URL` and `FUNDAMENTALS_API_KEY` for your licensed fundamentals provider. The adapter expects JSON keys `pe`, `industryPE`, `pb`, `debtToEquity`, `roe`, `bookValue`, `faceValue`, `asOf`. Map these to your vendor's actual response schema.
- `NEWS_API_KEY` for headlines.

## All-NSE universe
Import a current NSE or licensed-provider CSV through `POST /api/universe/import` with `Content-Type: text/csv`. It must contain a `symbol` header; optional `name`, `sector`, `isin`. The included sample universe is not complete. `SCAN_LIMIT` defaults to 60 to control provider usage; raise it only within your plan's quota.

## Important
A provider API key does not itself guarantee real-time exchange access. Check market-data timestamps, NSE exchange entitlement, rate limits and redistribution rights. Twelve Data documents quote/price endpoints, fundamentals endpoints and NSE exchange coverage; see its documentation and NSE real-time data subscription requirements.

P/E and P/B need sector comparisons. Debt/equity is not directly comparable for banks and NBFCs. Book value and face value are different measures; face value does not tell you whether a stock is cheap. Fundamentals are not enough to forecast short-term price changes.

The app calculates the return required for a user goal; it does not predict expected returns. Scenario rows are hypothetical fixed moves, not stock-specific forecasts. Build validated backtests with point-in-time data, survivorship-bias controls, corporate actions, transaction costs and out-of-sample testing before making return estimates.

Before public release add authentication, rate limiting, caching, logs, robust provider error handling, timestamps per metric, source links, data licensing review and legal/compliance review. This is an educational research tool, not SEBI-registered advice; it does not place trades.
