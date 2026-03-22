# Stock Market Monitor — Gemini AI + Gmail

An n8n automation workflow that monitors 200+ stocks across US, European, and UK markets daily, performs technical analysis, and sends an AI-powered investment report to your inbox.

## What It Does

1. **Triggers** at 8:00 AM on weekdays (Mon–Fri)
2. **Fetches** 1-year daily price data from Yahoo Finance for 200+ tickers across:
   - United States (S&P 500 components)
   - Europe (Euro Stoxx / Major EU markets)
   - United Kingdom (FTSE 100)
3. **Calculates** for each stock:
   - 180-day Simple Moving Average (SMA)
   - RSI-14 (Relative Strength Index)
   - % Deviation from SMA (classifies as OVERSOLD / OVERBOUGHT / NEUTRAL)
   - Dividend yield and ex-dividend dates
4. **Generates** a formatted HTML report with the top 20 oversold and overbought stocks per market
5. **Analyzes** the data using Google Gemini Flash AI (investment summary, sector themes, risk assessment)
6. **Sends** the combined report via Gmail

## Workflow Nodes

```
Daily Schedule (8AM Weekdays)
        │
        ▼
Fetch All Stocks & Calculate     ← Yahoo Finance API (200+ tickers)
        │
        ▼
Format HTML Email & AI Input
        │
        ▼
AI Investment Analysis  ←──── Google Gemini Flash (models/gemini-flash-latest)
        │
        ▼
Combine AI Summary + Report
        │
        ▼
Send Gmail Report
```

## Prerequisites

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- **Google Gemini API** credential (Google PaLM API type in n8n)
- **Gmail OAuth2** credential configured in n8n

## Setup

1. Import the workflow JSON into your n8n instance:
   - Go to **Workflows → Import from File**
   - Select `workflows/Stock Market Monitor - Gemini AI + Gmail.json`

2. Configure credentials:
   - Replace `YOUR_GEMINI_CREDENTIAL_ID` with your Google Gemini API credential
   - Replace `YOUR_GMAIL_CREDENTIAL_ID` with your Gmail OAuth2 credential

3. Update the recipient email address in the **Send Gmail Report** node:
   - Replace `YOUR_EMAIL@example.com` with your actual email address(es)

4. Activate the workflow

## Schedule

Runs automatically at **08:00 AM (server time), Monday–Friday**.

Cron expression: `0 8 * * 1-5`

## Technical Indicators Used

| Indicator | Description | Threshold |
|-----------|-------------|-----------|
| 180-Day SMA | Simple Moving Average over 180 trading days | — |
| Deviation % | `(price - SMA) / SMA * 100` | < -3% = Oversold, > +3% = Overbought |
| RSI-14 | Relative Strength Index over 14 periods | < 30 = Oversold, > 70 = Overbought |
| Dividend Yield | Annualized yield based on last 12 months of dividends | — |

## Disclaimer

This workflow is for **informational purposes only** and does not constitute financial advice. Always conduct your own research and consult a qualified financial advisor before making investment decisions.

## Data Source

Stock data is fetched from the [Yahoo Finance Chart API](https://finance.yahoo.com/). This is a public API used for personal, non-commercial use.
