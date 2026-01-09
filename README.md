# Hyperliquid Dashboard

A visual dashboard for viewing Hyperliquid trading data in a calendar format.

## Features

- **Funding Calendar** - Daily funding payments displayed in a calendar view
- **Realized P&L Calendar** - Daily realized profits/losses from closed trades
- **Combined View** - Funding + P&L combined in one calendar
- **Drill-down** - Click any day to see detailed trade breakdown

## Usage

Just open `hyperliquid-dashboard.html` in your browser. No server required - it fetches data directly from Hyperliquid's public API.

## Configuration

The wallet address is hardcoded in the HTML file. To use with a different wallet, edit this line:

```javascript
const WALLET = '0x3093189bd4d429CB2F47D03C14b9e9200B6b9425';
```

## Data Source

- Hyperliquid API: `https://api.hyperliquid.xyz/info`
- Endpoints used:
  - `userFunding` - Funding payment history
  - `userFills` - Trade history (for realized P&L)

## Screenshots

The dashboard shows 90 days of data with:
- Monthly totals
- Daily amounts color-coded (green = profit, red = loss)
- Trade counts per day
- Modal popup with trade details when clicking on a day
