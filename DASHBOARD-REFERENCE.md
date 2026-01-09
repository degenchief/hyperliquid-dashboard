# Hyperliquid Dashboard - Complete Reference

> **For Claude:** This document explains EVERYTHING about this project. Read it at the start of any session.

**Created:** January 2026
**Owner:** Clement (degenchief)
**Repository:** https://github.com/degenchief/hyperliquid-dashboard

---

## What This Dashboard Does

A **single-file HTML dashboard** that visualizes Hyperliquid trading data in calendar format:

1. **Funding Calendar** - Shows daily funding payments received from perpetual positions
2. **Realized P&L Calendar** - Shows profits/losses from closed trades
3. **Combined View** - Funding + P&L together
4. **Trading Journal** - THE MOST IMPORTANT FEATURE - personal notes for each trading day

---

## Key Features

### Calendar Views
- Year tabs: 2024, 2025, 2026 (user doesn't need 2023)
- Monthly totals displayed
- Color-coded: Green = profit, Red = loss
- Click any day to see detailed breakdown

### Trading Journal (Critical Feature)
- User writes daily trading thoughts/reflections
- Days with notes show 📝 indicator and orange border
- **ALL days are clickable** for journaling (not just days with trades)
- Stored in browser's localStorage (key: `hl-journal`)

### Journal Backup System
- **Export Journal** button - Downloads JSON backup file
- **Import Journal** button - Restores from backup (merges with existing)
- User explicitly asked to REMOVE the "Clear All" button for safety

---

## Technical Details

### Data Source
- **API:** `https://api.hyperliquid.xyz/info`
- **Wallet:** `0x3093189bd4d429CB2F47D03C14b9e9200B6b9425`
- **Endpoints:**
  - `userFunding` - Funding payment history
  - `userFills` - Trade history for P&L

### API Pagination (Important!)
The Hyperliquid API returns max 500 records per request. The dashboard uses pagination:

```javascript
// Fetch ALL data with pagination
async function fetchFunding() {
  let allData = [];
  let startTime = new Date('2024-01-01').getTime();
  while (true) {
    const res = await fetch(API_URL, {
      method: 'POST',
      body: JSON.stringify({ type: 'userFunding', user: WALLET, startTime: startTime })
    });
    const data = await res.json();
    if (!data || data.length === 0) break;
    allData = allData.concat(data);
    if (data.length < 500) break;
    const maxTime = Math.max(...data.map(f => f.time));
    startTime = maxTime + 1;  // Continue from next millisecond
  }
  return allData;
}
```

### Journal Storage
- **localStorage key:** `hl-journal`
- **Format:** `{ "2026-01-09": "My trading thoughts...", ... }`
- **Export format:** JSON with metadata (exportDate, noteCount, entries)

---

## Files

| File | Purpose |
|------|---------|
| `hyperliquid-dashboard.html` | The entire dashboard (single file) |
| `README.md` | Basic usage docs |
| `DASHBOARD-REFERENCE.md` | This file - complete technical reference |

### Desktop Shortcut
- Location: `/Users/degenchief/Desktop/Hyperliquid Dashboard.html`
- Type: Symlink pointing to the HTML file

---

## Common Issues & Fixes

### 1. Missing Recent Funding Data
**Symptom:** Recent days (e.g., Jan 6-9) show no funding amounts
**Cause:** API returns 500 records; if using `startTime: 0`, you get oldest data, not recent
**Fix:** Use pagination starting from a reasonable date (2024-01-01)

### 2. Can't Click Days Without Trades
**Symptom:** Empty days aren't clickable for journaling
**Cause:** Original code only made days with data clickable
**Fix:** All days should be clickable (already implemented)

### 3. Journal Notes Lost
**Symptom:** Notes disappear
**Cause:** localStorage cleared, or different browser used
**Fix:** Use Export/Import feature for backup. Export regularly!

---

## What NOT to Do

1. **NEVER** hardcode personal journal entries or trading thoughts into the code
2. **NEVER** commit the localStorage data to git
3. **NEVER** add back a "Clear All Journal" button (user explicitly removed it)
4. **NEVER** use `startTime: 0` for API calls (causes missing recent data)

---

## User Preferences

- Clement trades crypto on Hyperliquid
- He writes daily reflections on his trades
- Journal entries are THE MOST IMPORTANT - must be preserved
- He doesn't need 2023 data (only 2024, 2025, 2026)
- He wanted the "Clear All" button removed for safety
- Desktop shortcut should always work

---

## Future Ideas (KIV - Keep In View)

- AI analysis of trading journal to find patterns
- Cloud sync for journal backup
- Mobile-friendly version

---

## Quick Commands

```bash
# Open dashboard
open /Users/degenchief/Projects/hyperliquid-dashboard/hyperliquid-dashboard.html

# Or use desktop shortcut
open "/Users/degenchief/Desktop/Hyperliquid Dashboard.html"

# Push changes
cd /Users/degenchief/Projects/hyperliquid-dashboard
git add . && git commit -m "description" && git push

# View git history
git log --oneline -10
```

---

*Last updated: 2026-01-09*
