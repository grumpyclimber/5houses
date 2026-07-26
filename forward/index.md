---
layout: subpage
title: "Forward Scan — What the Rubric Finds Today"
---

# Forward Scan — What the Rubric Finds Today

*Supplementary page for: [Can You Spot a Great Company Before It Takes Off?](/)*

The rubric was built on historical data — companies scored after outcomes were known. The more useful question is what it finds when pointed at current annual reports, with no knowledge of what happens next.

In mid-2026, I ran the rubric on 57 ASX small-caps using their most recent annual reports. These are companies whose outcomes we don't know yet.

---

## How the Forward Scan Works

**Quant gate first.** Starting from 1,978 ASX-listed tickers, the pipeline filters for:
- Market cap between $50M and $2B (small-cap range where the rubric was trained)
- Average daily trading volume above $500K (eliminates illiquid micro-caps where a single trade moves the price 20%)

That left 200 companies passing the quant gate.

**Then the rubric.** For the 57 companies where IR pages and annual report PDFs were accessible, the targeted five-signal prompt was run using DeepSeek Pro on the most recent annual report available. The same prompt, the same rubric, the same scoring weights as the training experiment.

The remaining ~120 gate-passing tickers still have no publicly accessible annual report PDFs — mainly micro-caps whose investor relations pages don't host documents directly.

---

## Companies Scoring ≥ 50

A score above 50 puts a company in the top half of the training set's winners. In the training set, every company above 50 was a winner.

| # | Ticker | Company | Score | Sector | What the rubric saw |
|:-:|--------|---------|:-----:|--------|---------------------|
| 1 | QOR | Qoria | **100** | Technology / SaaS | All five signals present. Growing contracted ARR (order book equivalent), expanding margins, execution-focused tone, quantified revenue guidance, strong organic growth. The only current company to hit the same profile as PME FY2016. |
| 2 | NXL | Nuix | **96** | Technology / Analytics | Quantified order book (investigation analytics backlog), margins expanding, specific ARR targets disclosed. Revenue 2x over two years. |
| 3 | FWD | Fleetwood | **88** | Industrials / Modular Buildings | Order book, margin expansion, execution tone, and quantified forward guidance all present. Matches the structural profile of the training set's engineering winners (NWH, MAH, ACF). |
| 4 | SFR | Sandfire Resources | **70** | Materials / Copper | Perfect margins, tone, guidance, and revenue signals. Order book limited by spot-price exposure. Copper project pipeline partially substitutes. |
| 5 | CIP | Centuria Industrial REIT | **68** | Real Estate | Contracted lease pipeline provides order book equivalent. High-visibility bonus triggered by quantified forward NOI guidance. |
| 6 | MSV | Mitchell Services | **55** | Materials / Drilling | Margins, tone, guidance, and revenue all present. Order book present. Matches the mining services profile of several training winners. |
| 7 | PLT | Plenti Group | **55** | Financial Services / Credit | Note: PLT was a control in the training set (0.54x). Current score has improved — margin expansion and loan book growth now present. Re-rating possible if execution continues. |
| 8 | DUG | DUG Technology | **53** | Technology / HPC | Strong order book (contracted HPC capacity), founder-operator tone. Margins early-stage but improving. |
| 9 | VGL | Vista Group | **52** | Software / Cinema | SaaS metrics (ARR, net revenue retention) substitute for traditional order book. Margins recovering post-COVID. Execution tone consistent. |
| 10 | MLG | MLG Oz | **51** | Materials / Mining Services | Margins and revenue profile match training set industrials. Order book present. |
| 11 | PEN | Peninsula Energy | **51** | Energy / Uranium | Quantified production guidance and contracted offtake (order book equivalent). Uranium spot price tailwind. |
| 12 | REP | RAM Essential Services | **50** | Real Estate / Infrastructure | Contracted pipeline + quantified distribution guidance. |

---

## What Stands Out

**QOR (Qoria) is the highest-scoring company in the entire dataset** — including the training set. 100/100 on the current rubric, matching PME FY2016. Qoria makes parental controls software (Circle, Bark) sold to schools and families. It has contracted ARR growing at ~40%, expanding margins, and specific forward guidance on ARR targets. Whether it's the next PME is unknowable from this rubric — but it's showing every signal that the training winners showed.

**The rubric finds companies outside the engineering/mining cluster.** QOR, NXL, DUG, and VGL are all technology or software companies — not the industrials that dominated the training set. This is the same property the rubric showed with PME: the signals aren't sector-specific.

**PLT is interesting.** It was a control in the training set (0.54x over 2021–2026). It now scores 55 — in the winner zone. Whether the business has genuinely turned or the rubric is picking up noise is exactly the kind of question that needs qualitative follow-up.

---

## Limitations of This Scan

This is not a buy list. The same caveats that apply to the main experiment apply here, harder:

- **No outcome data.** We don't know what any of these companies will do. The rubric is a filter, not a prediction.
- **One year of data.** The training rubric worked on multi-year trajectories — signals consistent across 3–4 years. This forward scan uses one annual report per company. A single strong year may not reflect trajectory.
- **The ALC problem persists.** A company can score 70 and be in a structurally unwinnable market. The rubric can't see that.
- **57 of ~200 gate-passing companies scanned.** The 143 not yet scored may contain higher-scoring or lower-scoring companies than those shown.

The right use of this table: these are companies worth reading more carefully. The rubric says they're showing up operationally. Whether their market structure allows them to compound is a separate question — one the rubric can't answer.
