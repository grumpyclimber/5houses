---
layout: subpage
title: "Data — Training Set, Scores, and Rubric"
---

# Data — Training Set, Scores, and Rubric

*Supplementary page for: [Can You Spot a Great Company Before It Takes Off?](/)*

---

## The Training Set

20 ASX small-caps. 11 winners, 9 controls. All measured over a five-year window (2021–2026). Market cap between $100M and $1B at the start of the window.

Winner threshold: market cap grew to ≥ 2.5x. Control threshold: market cap ended at ≤ 0.8x. Everything between 0.8x and 2.5x was excluded.

### Winners

| Ticker | Company | Sector | 5-Year Multiple | Track |
|--------|---------|--------|----------------|-------|
| DRO | DroneShield | Industrials / Aerospace & Defence | **15.0x** | Main |
| CMM | Capricorn Metals | Basic Materials / Gold | **6.5x** | Main |
| NEU | Neuren Pharmaceuticals | Healthcare / Biotech | **8.7x** | Main |
| NWH | NRW Holdings | Industrials / Engineering & Construction | **5.6x** | Main |
| MAH | Macmahon Holdings | Industrials / Engineering & Construction | **5.5x** | Main |
| GNG | GR Engineering Services | Industrials / Engineering & Construction | **5.3x** | Main |
| MND | Monadelphous Group | Industrials / Engineering & Construction | **3.4x** | Main |
| PRN | Perenti | Industrials / Engineering & Construction | **3.2x** | Main |
| ACF | Acrow Limited | Industrials / Engineering & Construction | **2.9x** | Main |
| TLX | Telix Pharmaceuticals | Healthcare / Biotech | **2.67x** | Track 2 |
| DTL | Data#3 | Technology / IT Services | **2.2x** | Main |

DTL at 2.2x is technically below the 2.5x winner threshold and was kept as a documented borderline case.

### Controls

| Ticker | Company | Sector | 5-Year Multiple | Track |
|--------|---------|--------|----------------|-------|
| PPE | People Infrastructure | Industrials / Staffing | **0.18x** | Main |
| ALC | Alcidion Group | Healthcare / Health IT | **0.26x** | Main |
| OFX | OFX Group | Financial Services / Payments | **0.38x** | Main |
| CVN | Carnarvon Energy | Energy / Oil & Gas Exploration | **0.42x** | Main |
| CLV | Clover Corporation | Consumer Defensive / Foods | **0.54x** | Main |
| PLT | Plenti Group | Financial Services / Credit | **0.54x** | Main |
| RDY | ReadyTech Holdings | Technology / SaaS | **0.66x** | Main |
| ACL | Australian Clinical Labs | Healthcare / Diagnostics | **0.74x** | Main |
| PPS | Praemium | Financial Services / Asset Management | **0.74x** | Main |

BOT (Botto) was removed from the original control set — no annual reports were available.

---

## How the Score Is Calculated

Every company is scored year by year across five signals. Each signal is present (yes) or absent (no) for that year. Points are awarded per signal across all available years, then totalled to 100.

### Rubric weights

| Signal | Weight | What it measures |
|--------|--------|-----------------|
| Order book / contracted backlog | 30% | Is there a specific dollar figure disclosed, growing year-on-year? |
| Organic revenue growth >10% | 20% | Is revenue growing from the core business, not acquisitions? |
| Margin expansion | 20% | Is the operating margin improving? |
| Execution-focused management tone | 15% | Is language grounded in delivery, not aspiration? |
| Specific forward guidance | 15% | Are there real numbers attached to next year's targets? |

Plus two combination bonuses:
- **Compounding operator** (+15 pts): growing order book + expanding margins + execution tone all present in the same year
- **High-visibility compounder** (+10 pts): compounding operator + specific quantified guidance in the same year

The combination bonuses are what separate the top tier from the middle — a company can score in the 50s by consistently showing individual signals, but scoring in the 80s and 90s requires all signals firing simultaneously.

---

## Full Scoring Table

All 20 companies across FY2019–FY2022 (main set). Signals shown as years-present / years-available.

| Ticker | Group | Yrs | OrdBk | Mrgn↑ | ExecT | Combo | FwdGuid | OrgGr | Score |
|--------|-------|:---:|:-----:|:-----:|:-----:|:-----:|:-------:|:-----:|:-----:|
| ACF | WIN | 3 | 3/3 | 3/3 | 3/3 | 2/3 | 3/3 | 1/3 | **90** |
| MAH | WIN | 4 | 4/4 | 2/4 | 4/4 | 1/4 | 3/4 | 2/4 | **70** |
| GNG | WIN | 4 | 4/4 | 2/4 | 4/4 | 1/4 | 2/4 | 1/4 | **66** |
| DRO | WIN | 4 | 4/4 | 1/4 | 4/4 | 0/4 | 4/4 | 4/4 | **65** |
| NWH | WIN | 4 | 4/4 | 1/4 | 4/4 | 1/4 | 3/4 | 0/4 | **63** |
| PRN | WIN | 4 | 4/4 | 1/4 | 4/4 | 0/4 | 2/4 | 1/4 | **56** |
| MND | WIN | 4 | 4/4 | 1/4 | 4/4 | 0/4 | 0/4 | 1/4 | **51** |
| TLX | WIN | 3 | 0/3 | 1/3 | 3/3 | 0/3 | 3/3 | 2/3 | **35** |
| DTL | WIN | 4 | 1/4 | 1/4 | 3/4 | 0/4 | 1/4 | 3/4 | **30** |
| CMM | WIN | 4 | 0/4 | 1/4 | 4/4 | 0/4 | 4/4 | 0/4 | **30** |
| NEU | WIN | 4 | 0/4 | 0/4 | 4/4 | 0/4 | 3/4 | 0/4 | **23** |
| **Winner avg** | | | | | | | | | **52.6** |
| ALC | CTRL | 4 | 4/4 | 3/4 | 4/4 | 0/4 | 4/4 | 0/4 | **70** |
| RDY | CTRL | 4 | 2/4 | 1/4 | 4/4 | 0/4 | 4/4 | 0/4 | **45** |
| ACL | CTRL | 2 | 0/2 | 2/2 | 2/2 | 0/2 | 1/2 | 0/2 | **40** |
| PPS | CTRL | 4 | 1/4 | 2/4 | 3/4 | 0/4 | 4/4 | 1/4 | **40** |
| OFX | CTRL | 4 | 1/4 | 2/4 | 4/4 | 0/4 | 2/4 | 1/4 | **39** |
| PPE | CTRL | 4 | 1/4 | 1/4 | 3/4 | 0/4 | 3/4 | 0/4 | **31** |
| CVN | CTRL | 4 | 2/4 | 0/4 | 2/4 | 0/4 | 3/4 | 0/4 | **30** |
| CLV | CTRL | 4 | 0/4 | 2/4 | 3/4 | 0/4 | 1/4 | 3/4 | **28** |
| PLT | CTRL | 2 | 0/2 | 1/2 | 1/2 | 0/2 | 1/2 | 1/2 | **25** |
| **Control avg** | | | | | | | | | **38.7** |

**Separation: 14 points.** Overlap: ALC (70) scores above TLX, DTL, CMM, and NEU.

The combo column is the decisive one — no control ever triggered the compounding operator combination in any year. Four winners did.

---

## Cross-Cycle Validation (Track 3 — FY2016/2017)

Two companies scored on reports from a different time window (2016–2021), using the same frozen rubric. This was the out-of-sample test.

| Ticker | Company | 5-Year Multiple (2016–2021) | Score |
|--------|---------|---------------------------|-------|
| PME | Pro Medicus | **10.96x** | **100** |
| HUB | Hub24 | **6.65x** | **48** |
