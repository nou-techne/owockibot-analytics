# Bounty #246 — owockibot Analytics Page

**Bounty:** 30 USDC  
**Status:** CLAIMED (0xC376...d3e2)  
**Deliverable:** Public analytics page at owockibot.xyz/analytics  
**Deploy target:** Vercel (standalone HTML/JS, no framework required)  
**Submit:** Source code + screenshot

---

## What We're Building

A single-page analytics dashboard that makes the owockibot bounty ecosystem legible — how much work is happening, who's doing it, and whether the velocity is increasing. Same philosophy as the watershed dashboard: fetch public data, visualize it clearly, deploy statically.

---

## Data Sources

Both APIs are public, no auth required.

### GET /api/bounty-board
Returns full list of all bounties (75 as of Feb 28). Each record includes:
- `id`, `title`, `description`, `reward_usdc`
- `status`: open | claimed | submitted | completed | cancelled
- `creator_address`, `claimer_address`, `submission_url`
- `created_at`, `updated_at`
- `feedback`, `comments[]`

### GET /api/bounty-board/stats
Returns aggregate snapshot:
```json
{
  "total": 75,
  "open": 1,
  "claimed": 20,
  "submitted": 5,
  "completed": 34,
  "cancelled": 15,
  "total_volume_usdc": 775,
  "total_posted_usdc": 1500
}
```

---

## Deliverable Requirements (from bounty)

1. Total bounties created / completed / pending over time
2. Total USDC paid out
3. Unique builders count
4. Average completion time
5. Charts / graphs for trends

---

## Implementation Plan

### Stack
- Vanilla HTML + JS (no build step, Vercel static deploy)
- Chart.js for visualizations (CDN)
- Style: dark theme, gold accent (#f4c542) matching owockibot.xyz aesthetic

### Page Sections

**Hero Stats Row** (top-level numbers, pulls from /stats)
- Total bounties posted
- Total USDC paid out
- Open bounties
- Unique builders (derived: count distinct claimer_addresses in completed bounties)

**Chart 1 — Bounty Activity Over Time** (line chart)
- X axis: date (by week)
- Y axis: cumulative bounties created vs completed
- Derived from `created_at` and `updated_at` on completed records
- Shows growth curve and completion velocity

**Chart 2 — Status Distribution** (donut chart)
- Segments: completed / claimed / submitted / open / cancelled
- Real-time from /stats endpoint

**Chart 3 — USDC Paid Out Over Time** (bar chart)
- Weekly USDC earned (sum of reward_usdc for completed bounties by week)
- Derived from completed records + their updated_at timestamps

**Leaderboard — Top Builders**
- Rank by: bounties completed + USDC earned
- Derived from claimer_address on completed records
- Truncated wallet display (0x1234...5678)

**Average Completion Time**
- For completed bounties: diff between created_at and updated_at
- Display as median (not mean — outliers skew heavily)
- Exclude cancelled records

### Derived Metrics (computed client-side from /api/bounty-board)

| Metric | Derivation |
|--------|------------|
| Unique builders | distinct claimer_address on status=completed |
| Avg completion time | median(updated_at - created_at) for completed |
| Weekly USDC | sum(reward_usdc) grouped by ISO week of updated_at |
| Weekly bounties created | count grouped by ISO week of created_at |
| Weekly completed | count grouped by ISO week of updated_at where status=completed |

---

## File Structure

```
owockibot-246-analytics/
  index.html        # single file, self-contained
  README.md         # setup + deployment notes
  vercel.json       # static config (optional, Vercel auto-detects HTML)
```

---

## Visual Design

Match owockibot.xyz:
- Background: #0a0a0a (near black)
- Text: #e0e0e0
- Accent: #f4c542 (gold)
- Font: Berkeley Mono (local) or monospace fallback
- Card style: subtle border, slight glow on hover

---

## Deployment

- Vercel project: `owockibot-analytics` (new, under our account or nou-techne org)
- Target URL: submit as live Vercel URL (owockibot.xyz/analytics is their domain — we submit standalone, they can proxy if they want)
- Submit: GitHub repo link + Vercel live URL + screenshot

---

## Acceptance Criteria Checklist

- [ ] Bounty activity over time chart (created vs completed, by week)
- [ ] Total USDC paid out visible
- [ ] Unique builders count visible
- [ ] Average (median) completion time visible
- [ ] At least one trend chart with historical data
- [ ] Live Vercel deployment accessible
- [ ] Source code in public GitHub repo
- [ ] Screenshot included in submission

---

## Notes

- The /api/bounty-board endpoint returns all 75 bounties in one call — no pagination needed yet
- Completion time outliers: some bounties sat claimed for days without completing — use median, not mean
- Cancelled bounties: exclude from USDC volume (reward was never paid)
- The watershed dashboard pattern applies directly here: fetch on load, compute client-side, render with Chart.js

---

*Claimed: 2026-02-28 · Wallet: 0xC37604A1dD79Ed50A5c2943358db85CB743dd3e2*
