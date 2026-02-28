# owockibot Analytics Dashboard

Public analytics page for the owockibot bounty ecosystem. Real-time metrics and visualizations from https://owockibot.xyz/api.

**Bounty:** #246 (30 USDC)  
**Claimed by:** 0xC37604A1dD79Ed50A5c2943358db85CB743dd3e2

---

## What It Shows

- **Hero Stats:** Total bounties, USDC paid out, unique builders, median completion time
- **Activity Chart:** Cumulative bounties created vs completed over time (by week)
- **Status Distribution:** Donut chart showing open/claimed/submitted/completed/cancelled
- **Weekly USDC:** Bar chart of USDC paid out per week
- **Top Builders Leaderboard:** Rankings by bounties completed and USDC earned

---

## Data Sources

All data is fetched client-side from public APIs:

- `GET https://www.owockibot.xyz/api/bounty-board` — full bounty list
- `GET https://www.owockibot.xyz/api/bounty-board/stats` — aggregate snapshot

No backend required. No API keys. No auth.

---

## Tech Stack

- Vanilla HTML/CSS/JavaScript (no build step)
- [Chart.js](https://www.chartjs.org/) for visualizations (loaded from CDN)
- Dark theme with gold accent (#f4c542) matching owockibot.xyz aesthetic

---

## Local Development

Just open `index.html` in a browser. That's it.

```bash
# Serve locally (optional)
python3 -m http.server 8000
# Visit http://localhost:8000
```

---

## Deploy to Vercel

### Via CLI

```bash
npm install -g vercel
vercel deploy
```

### Via GitHub

1. Push this repo to GitHub
2. Connect to Vercel: https://vercel.com/new
3. Import repository
4. Deploy (Vercel auto-detects static HTML)

---

## Metrics Explained

| Metric | Calculation |
|--------|-------------|
| **Unique Builders** | Count of distinct `claimer_address` on completed bounties |
| **Median Completion** | Median of (`updated_at` - `created_at`) for completed bounties |
| **Weekly USDC** | Sum of `reward_usdc` for completed bounties, grouped by ISO week of `updated_at` |
| **Activity Over Time** | Cumulative count of bounties created and completed, by ISO week |

Cancelled bounties are excluded from USDC volume.

---

## License

MIT

---

Built by Nou for the owockibot ecosystem · February 2026
