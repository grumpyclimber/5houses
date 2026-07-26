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
| 1 | QOR | Qoria *(delisted July 2026)* | **100** | Technology / SaaS | All five signals present. Growing contracted ARR (order book equivalent), expanding margins, execution-focused tone, quantified revenue guidance, strong organic growth. The only current company to hit the same profile as PME FY2016. |
| 2 | NXL | Nuix | **96** | Technology / Analytics | ACV of $234M (order book equivalent), EBITDA margin expanding from 14.6% to 21.8%, 96% subscription revenue. Revenue up 15.2% organically. |
| 3 | FWD | Fleetwood | **88** | Industrials / Modular Buildings | Order book present ($115M) but declining from $178M. Margin expansion strong (EBIT 2% → 7.5%), execution tone, repeatable revenue 83%. Matches structural profile of training set engineering winners. |
| 4 | SFR | Sandfire Resources | **70** | Materials / Copper | Perfect margins, tone, guidance, and revenue signals. Order book limited by spot-price exposure. Copper project pipeline partially substitutes. |
| 5 | CIP | Centuria Industrial REIT | **68** | Real Estate | Long-dated leases (WALE 7.1yr, 95.7% occupancy) provide revenue visibility. Quantified FFO guidance (18.2–18.5 cpu). Like-for-like NOI growth 5.1%. |
| 6 | MSV | Mitchell Services | **55** | Materials / Drilling | Margins, tone, guidance, and revenue all present. Order book present. Matches the mining services profile of several training winners. |
| 7 | PLT | Plenti Group | **55** | Financial Services / Credit | Note: PLT was a control in the training set (0.54x). Current score has improved — margin expansion and loan book growth now present. Re-rating possible if execution continues. |
| 8 | DUG | DUG Technology | **53** | Technology / HPC | Record order book (US$52M backlog), founder-operator tone. EBITDA margin stable at 25%. Revenue declined 4.5% full-year but H2 was stronger (30% margin). |
| 9 | VGL | Vista Group | **52** | Software / Cinema | SaaS metrics (ARR $163M, 90% recurring revenue) substitute for traditional order book. EBITDA margin expanding (14.4% → 17.2%). Quantified FY26 guidance provided. |
| 10 | MLG | MLG Oz | **51** | Materials / Mining Services | Margins expanding (11.8% → 12.2%), revenue up 15.5%. Order book described as "robust" but not quantified. |
| 11 | PEN | Peninsula Energy | **51** | Energy / Uranium | Quantified production guidance (0.4–0.5 Mlbs CY2026) and contracted offtake through 2034. Pre-revenue — $0 sales in FY2025, losses continuing. Score driven by forward visibility, not current operations. |
| 12 | REP | RAM Essential Services | **50** | Real Estate / Infrastructure | Contracted pipeline + quantified distribution guidance. |

<div class="chart-wrap">
  <canvas id="forwardChart"></canvas>
</div>
<p class="chart-caption">57 ASX small-caps scored against the same rubric used to build the training set. The dashed line marks 50 — in the training set, every company above 50 was a winner. Hover for score. QOR is marked as delisted following its July 2026 acquisition.</p>

<script>
(function() {
  var ctx = document.getElementById('forwardChart');
  var labels = ['QOR — Qoria*','NXL — Nuix','FWD — Fleetwood','SFR — Sandfire','CIP — Centuria Ind.','MSV — Mitchell Svcs','PLT — Plenti','DUG — DUG Tech','VGL — Vista Group','MLG — MLG Oz','PEN — Peninsula Energy','REP — RAM Essential'];
  var scores = [100, 96, 88, 70, 68, 55, 55, 53, 52, 51, 51, 50];
  var colors = scores.map(function(s) {
    return s >= 80 ? 'rgba(34,197,94,0.82)'
         : s >= 60 ? 'rgba(34,197,94,0.65)'
         : 'rgba(34,197,94,0.48)';
  });

  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: labels,
      datasets: [{
        label: 'Rubric Score',
        data: scores,
        backgroundColor: colors,
        borderColor: 'rgba(22,163,74,0.9)',
        borderWidth: 1.5,
        borderRadius: 3
      }]
    },
    options: {
      indexAxis: 'y',
      responsive: true,
      plugins: {
        legend: { display: false },
        tooltip: {
          callbacks: {
            label: function(c) { return 'Score: ' + c.raw + ' / 100'; }
          },
          backgroundColor: '#fff',
          titleColor: '#1c1c1e',
          bodyColor: '#1c1c1e',
          borderColor: '#e5e7eb',
          borderWidth: 1,
          padding: 10,
          bodyFont: { family: "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" }
        }
      },
      scales: {
        x: {
          min: 0, max: 115,
          grid: { color: 'rgba(0,0,0,0.05)' },
          ticks: { stepSize: 25 }
        },
        y: {
          grid: { display: false },
          ticks: {
            font: {
              family: "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif",
              size: 11.5
            }
          }
        }
      }
    },
    plugins: [{
      id: 'threshold-line',
      afterDraw: function(chart) {
        var c2 = chart.ctx;
        var xAxis = chart.scales.x;
        var yAxis = chart.scales.y;
        var x = xAxis.getPixelForValue(50);
        c2.save();
        c2.beginPath();
        c2.moveTo(x, yAxis.top);
        c2.lineTo(x, yAxis.bottom);
        c2.lineWidth = 1.5;
        c2.strokeStyle = 'rgba(107,114,128,0.55)';
        c2.setLineDash([5, 4]);
        c2.stroke();
        c2.fillStyle = '#6b7280';
        c2.font = "11px -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif";
        c2.fillText('50 — winner zone', x + 6, yAxis.top + 14);
        c2.restore();
      }
    }]
  });
})();
</script>

---

## What Stands Out

**QOR (Qoria) is the highest-scoring company in the entire dataset** — including the training set. 100/100 on the current rubric, matching PME FY2016. Qoria makes parental controls software (Circle, Bark) sold to schools and families. It had contracted ARR growing at ~40%, expanding margins, and specific forward guidance on ARR targets.

**Update (July 2026): Qoria was acquired by US digital safety group Aura Consolidated Group and delisted from the ASX on 20 July 2026.** Shareholders approved the scheme 91.49% in favour; the implied acquisition price of ~A$0.40 per share represented a 32.4% premium to the 30-day VWAP. The combined entity now trades on the ASX as AXQ. The rubric scored it 100 before any of this was public — it was showing every signal the training winners showed, and it was acquired at a premium within months of the scan.

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
