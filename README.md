# Bellabeat Wellness Study — Data Analyst Portfolio

A single-page portfolio site built to present findings from a Bellabeat-style
fitness tracker dataset: 35 members with combined activity and sleep logs,
joined on member ID.

**Live file:** `index.html` — a single, self-contained page. All CSS and
JavaScript are embedded inline; there are no external stylesheets, scripts,
or font requests. It can be opened directly in a browser or served from
GitHub Pages with no build step.

## Design

Soft, art-deco visual language: ivory and blush tones, deep aubergine
plum, rose gold and champagne gold linework, radiating sunburst motifs,
and geometric progress rings — built to feel warm and womanly rather than
strictly corporate, while still reading as a data-first portfolio.

## Data & methodology

Source data: `combine_member_data.sql` and `combined_member_data.xlsx`,
included at the repo root, were used to derive the figures below.

- 35 members, activity table left-joined to sleep logs (24 of 35 members
  have sleep data).
- Days tracked per member ranged from 8 to 32 (average 13).
- All percentages and averages quoted on the page are computed directly
  from the `Combined_By_Member` sheet.

## Three key insights

1. **Movement is the exception, not the norm.** Sedentary minutes average
   82.8% of tracked time; average daily steps are 6,538, and only ~17% of
   members average 10,000+ steps/day.
2. **Sleep is good when captured — the gap is capture itself.** Sleep
   efficiency averages 91.2% among the 24 members who logged it, but 31%
   of members (11 of 35) never logged a night at all.
3. **Engagement dips are common, and activity still pays off.** Steps and
   calories burned correlate moderately (r ≈ 0.48), but 14 of 35 members
   (40%) had at least one zero-step day (61 such days total).

## Where the data falls short

- Incomplete sleep logging (31% of members have none)
- No demographic or menstrual-cycle context (age, weight, height, cycle
  phase are absent)
- Short observation window (8–32 days, average 13)
- No heart rate, HRV, or stress markers

## Predictions

- Nudging average steps from ~6,500 to ~8,000/day should produce a
  measurable rise in calories burned, given the existing correlation.
- Sleep-logging rate is likely to keep functioning as an early churn
  signal, independent of actual sleep quality.
- Collecting demographic and cycle-phase data would likely reveal
  distinct member segments currently hidden inside a single blended
  average.

## Project structure

```
.
├── index.html                   # self-contained site (HTML + CSS + JS)
├── images/                       # reserved for future image assets
│   └── .gitkeep
├── combine_member_data.sql       # SQL used to join activity + sleep tables
├── combined_member_data.xlsx     # combined dataset the page's figures come from
└── README.md
```

## Running locally

No build step required — just open `index.html` in a browser, or serve
the folder with any static file server:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.
