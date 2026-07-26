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

| Ticker | Company | Sector | 5-Year Multiple |
|--------|---------|--------|----------------|
| DRO | DroneShield | Industrials / Aerospace & Defence | **15.0x** |
| CMM | Capricorn Metals | Basic Materials / Gold | **6.5x** |
| NEU | Neuren Pharmaceuticals | Healthcare / Biotech | **8.7x** |
| NWH | NRW Holdings | Industrials / Engineering & Construction | **5.6x** |
| MAH | Macmahon Holdings | Industrials / Engineering & Construction | **5.5x** |
| GNG | GR Engineering Services | Industrials / Engineering & Construction | **5.3x** |
| MND | Monadelphous Group | Industrials / Engineering & Construction | **3.4x** |
| PRN | Perenti | Industrials / Engineering & Construction | **3.2x** |
| ACF | Acrow Limited | Industrials / Engineering & Construction | **2.9x** |
| TLX | Telix Pharmaceuticals | Healthcare / Biotech | **2.67x** |
| DTL | Data#3 | Technology / IT Services | **2.2x** |

DTL at 2.2x is technically below the 2.5x winner threshold and was kept as a documented borderline case.

### Controls

| Ticker | Company | Sector | 5-Year Multiple |
|--------|---------|--------|----------------|
| PPE | People Infrastructure | Industrials / Staffing | **0.18x** |
| ALC | Alcidion Group | Healthcare / Health IT | **0.26x** |
| OFX | OFX Group | Financial Services / Payments | **0.38x** |
| CVN | Carnarvon Energy | Energy / Oil & Gas Exploration | **0.42x** |
| CLV | Clover Corporation | Consumer Defensive / Foods | **0.54x** |
| PLT | Plenti Group | Financial Services / Credit | **0.54x** |
| RDY | ReadyTech Holdings | Technology / SaaS | **0.66x** |
| ACL | Australian Clinical Labs | Healthcare / Diagnostics | **0.74x** |
| PPS | Praemium | Financial Services / Asset Management | **0.74x** |

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

## How Each Company Scored

All 20 companies from the FY2019–FY2022 training set. Winners in green, controls in red. Hover a bar for the signal breakdown. The dashed line marks the winner average (52.6).

<div class="chart-wrap">
  <canvas id="scoreChart"></canvas>
</div>
<p class="chart-caption">Winners averaged 52.6; controls averaged 38.7 — a 14-point gap. The key finding isn't the averages: no control ever had all three core signals (order book + margins + execution tone) firing in the same year. Four winners did.</p>

<script>
(function() {
  var data = [
    {t:'ACF', g:'W', s:100, ob:'3/3', mg:'3/3', et:'3/3', cb:'2/3', fg:'3/3', rg:'1/3'},
    {t:'MAH', g:'W', s:70,  ob:'4/4', mg:'2/4', et:'4/4', cb:'1/4', fg:'3/4', rg:'2/4'},
    {t:'ALC', g:'C', s:70,  ob:'4/4', mg:'3/4', et:'4/4', cb:'0/4', fg:'4/4', rg:'0/4'},
    {t:'GNG', g:'W', s:66,  ob:'4/4', mg:'2/4', et:'4/4', cb:'1/4', fg:'2/4', rg:'1/4'},
    {t:'DRO', g:'W', s:65,  ob:'4/4', mg:'1/4', et:'4/4', cb:'0/4', fg:'4/4', rg:'4/4'},
    {t:'NWH', g:'W', s:63,  ob:'4/4', mg:'1/4', et:'4/4', cb:'1/4', fg:'3/4', rg:'0/4'},
    {t:'PRN', g:'W', s:56,  ob:'4/4', mg:'1/4', et:'4/4', cb:'0/4', fg:'2/4', rg:'1/4'},
    {t:'MND', g:'W', s:51,  ob:'4/4', mg:'1/4', et:'4/4', cb:'0/4', fg:'0/4', rg:'1/4'},
    {t:'RDY', g:'C', s:45,  ob:'2/4', mg:'1/4', et:'4/4', cb:'0/4', fg:'4/4', rg:'0/4'},
    {t:'ACL', g:'C', s:40,  ob:'0/2', mg:'2/2', et:'2/2', cb:'0/2', fg:'1/2', rg:'0/2'},
    {t:'PPS', g:'C', s:40,  ob:'1/4', mg:'2/4', et:'3/4', cb:'0/4', fg:'4/4', rg:'1/4'},
    {t:'OFX', g:'C', s:39,  ob:'1/4', mg:'2/4', et:'4/4', cb:'0/4', fg:'2/4', rg:'1/4'},
    {t:'TLX', g:'W', s:35,  ob:'0/3', mg:'1/3', et:'3/3', cb:'0/3', fg:'3/3', rg:'2/3'},
    {t:'PPE', g:'C', s:31,  ob:'1/4', mg:'1/4', et:'3/4', cb:'0/4', fg:'3/4', rg:'0/4'},
    {t:'DTL', g:'W', s:30,  ob:'1/4', mg:'1/4', et:'3/4', cb:'0/4', fg:'1/4', rg:'3/4'},
    {t:'CMM', g:'W', s:30,  ob:'0/4', mg:'1/4', et:'4/4', cb:'0/4', fg:'4/4', rg:'0/4'},
    {t:'CVN', g:'C', s:30,  ob:'2/4', mg:'0/4', et:'2/4', cb:'0/4', fg:'3/4', rg:'0/4'},
    {t:'CLV', g:'C', s:28,  ob:'0/4', mg:'2/4', et:'3/4', cb:'0/4', fg:'1/4', rg:'3/4'},
    {t:'PLT', g:'C', s:25,  ob:'0/2', mg:'1/2', et:'1/2', cb:'0/2', fg:'1/2', rg:'1/2'},
    {t:'NEU', g:'W', s:23,  ob:'0/4', mg:'0/4', et:'4/4', cb:'0/4', fg:'3/4', rg:'0/4'}
  ];

  var labels = data.map(function(d) { return d.t; });
  var scores = data.map(function(d) { return d.s; });
  var colors = data.map(function(d) {
    return d.g === 'W' ? 'rgba(34,197,94,0.78)' : 'rgba(239,68,68,0.75)';
  });
  var borders = data.map(function(d) {
    return d.g === 'W' ? 'rgba(22,163,74,1)' : 'rgba(220,38,38,1)';
  });

  var ctx = document.getElementById('scoreChart');
  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: labels,
      datasets: [{
        label: 'Rubric Score',
        data: scores,
        backgroundColor: colors,
        borderColor: borders,
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
            title: function(items) {
              var d = data[items[0].dataIndex];
              return d.t + ' (' + (d.g === 'W' ? 'Winner' : 'Control') + ')  —  Score: ' + d.s;
            },
            label: function(item) { return ''; },
            afterLabel: function(item) {
              var d = data[item.dataIndex];
              return [
                'Order book:        ' + d.ob,
                'Margin expansion:  ' + d.mg,
                'Execution tone:    ' + d.et,
                'Combo signal:      ' + d.cb,
                'Forward guidance:  ' + d.fg,
                'Revenue growth:    ' + d.rg
              ];
            }
          },
          backgroundColor: '#fff',
          titleColor: '#1c1c1e',
          bodyColor: '#6b7280',
          borderColor: '#e5e7eb',
          borderWidth: 1,
          padding: 12,
          bodyFont: { family: "'SF Mono', 'Fira Code', Consolas, monospace", size: 11 },
          titleFont: { family: "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif", size: 12, weight: 'bold' }
        }
      },
      scales: {
        x: {
          min: 0, max: 110,
          grid: { color: 'rgba(0,0,0,0.05)' },
          ticks: { stepSize: 20 }
        },
        y: {
          grid: { display: false },
          ticks: {
            font: { family: "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif", size: 12 }
          }
        }
      }
    },
    plugins: [{
      id: 'avg-lines',
      afterDraw: function(chart) {
        var c2 = chart.ctx;
        var xAxis = chart.scales.x;
        var yAxis = chart.scales.y;
        [[52.6, 'rgba(22,163,74,0.5)', 'Winners avg 52.6'],
         [38.7, 'rgba(220,38,38,0.5)', 'Controls avg 38.7']].forEach(function(item) {
          var x = xAxis.getPixelForValue(item[0]);
          c2.save();
          c2.beginPath();
          c2.moveTo(x, yAxis.top);
          c2.lineTo(x, yAxis.bottom);
          c2.lineWidth = 1.5;
          c2.strokeStyle = item[1];
          c2.setLineDash([4, 3]);
          c2.stroke();
          c2.fillStyle = item[1].replace('0.5', '0.9');
          c2.font = "10.5px -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif";
          c2.fillText(item[2], x + 4, yAxis.top + 12);
          c2.restore();
        });
      }
    }]
  });
})();
</script>

<div class="chart-wrap">
  <canvas id="scatterChart"></canvas>
</div>
<p class="chart-caption">Each dot is one company. Hover for ticker and score. Winners (green) averaged 52.6; controls (red) averaged 38.7. ALC scores 70 but returned only 0.26× — the main anomaly. Purple triangles are the out-of-sample test (PME, HUB), scored on earlier data not used to build the rubric.</p>

<script>
(function() {
  var ctx = document.getElementById('scatterChart');
  new Chart(ctx, {
    type: 'scatter',
    data: {
      datasets: [
        {
          label: 'Winners',
          data: [
            {x:90, y:2.9,  t:'ACF'},
            {x:70, y:5.5,  t:'MAH'},
            {x:66, y:5.3,  t:'GNG'},
            {x:65, y:15.0, t:'DRO'},
            {x:63, y:5.6,  t:'NWH'},
            {x:56, y:3.2,  t:'PRN'},
            {x:51, y:3.4,  t:'MND'},
            {x:35, y:2.67, t:'TLX'},
            {x:30, y:2.2,  t:'DTL'},
            {x:30, y:6.5,  t:'CMM'},
            {x:23, y:8.7,  t:'NEU'}
          ],
          backgroundColor: 'rgba(34,197,94,0.75)',
          borderColor: 'rgba(22,163,74,1)',
          borderWidth: 1.5,
          pointRadius: 7,
          pointHoverRadius: 9
        },
        {
          label: 'Controls',
          data: [
            {x:70, y:0.26, t:'ALC'},
            {x:45, y:0.66, t:'RDY'},
            {x:40, y:0.74, t:'ACL'},
            {x:40, y:0.74, t:'PPS'},
            {x:39, y:0.38, t:'OFX'},
            {x:31, y:0.18, t:'PPE'},
            {x:30, y:0.42, t:'CVN'},
            {x:28, y:0.54, t:'CLV'},
            {x:25, y:0.54, t:'PLT'}
          ],
          backgroundColor: 'rgba(239,68,68,0.75)',
          borderColor: 'rgba(220,38,38,1)',
          borderWidth: 1.5,
          pointRadius: 7,
          pointHoverRadius: 9
        },
        {
          label: 'Out-of-sample',
          data: [
            {x:100, y:10.96, t:'PME'},
            {x:48,  y:6.65,  t:'HUB'}
          ],
          backgroundColor: 'rgba(99,102,241,0.85)',
          borderColor: 'rgba(79,70,229,1)',
          borderWidth: 1.5,
          pointRadius: 8,
          pointStyle: 'triangle',
          pointHoverRadius: 10
        }
      ]
    },
    options: {
      responsive: true,
      plugins: {
        legend: {
          position: 'bottom',
          labels: {
            font: { family: "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif", size: 12 },
            padding: 16,
            usePointStyle: true
          }
        },
        tooltip: {
          callbacks: {
            label: function(c) {
              var d = c.raw;
              return d.t + '  |  score ' + d.x + '  |  ' + d.y + '×';
            }
          },
          titleColor: '#1c1c1e',
          bodyColor: '#1c1c1e',
          backgroundColor: '#fff',
          borderColor: '#e5e7eb',
          borderWidth: 1,
          padding: 10,
          bodyFont: { family: "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" }
        }
      },
      scales: {
        x: {
          title: { display: true, text: 'Rubric Score', font: { size: 12 } },
          min: 0, max: 115,
          grid: { color: 'rgba(0,0,0,0.05)' },
          ticks: { stepSize: 20 }
        },
        y: {
          title: { display: true, text: '5-Year Multiple (×)', font: { size: 12 } },
          min: 0,
          grid: { color: 'rgba(0,0,0,0.05)' }
        }
      }
    }
  });
})();
</script>

<div class="chart-wrap">
  <canvas id="signalChart"></canvas>
</div>
<p class="chart-caption">% of companies in each group where the signal appeared in at least one scored year. Individual signals barely separate the groups — execution tone and forward guidance are near-universal. The combination signal (order book + margins + execution tone firing in the same year) is the clean separator: 4 of 11 winners triggered it; zero controls ever did.</p>

<script>
(function() {
  var ctx2 = document.getElementById('signalChart');
  new Chart(ctx2, {
    type: 'bar',
    data: {
      labels: ['Order Book', 'Margin\nExpansion', 'Exec Tone', 'Fwd Guidance', 'Org. Growth', 'Combo\n(all three)'],
      datasets: [
        {
          label: 'Winners (11)',
          data: [73, 91, 100, 91, 73, 36],
          backgroundColor: 'rgba(34,197,94,0.7)',
          borderColor: 'rgba(22,163,74,1)',
          borderWidth: 1.5,
          borderRadius: 3
        },
        {
          label: 'Controls (9)',
          data: [67, 89, 100, 100, 44, 0],
          backgroundColor: 'rgba(239,68,68,0.7)',
          borderColor: 'rgba(220,38,38,1)',
          borderWidth: 1.5,
          borderRadius: 3
        }
      ]
    },
    options: {
      responsive: true,
      plugins: {
        legend: {
          position: 'bottom',
          labels: {
            font: { family: "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif", size: 12 },
            padding: 16,
            usePointStyle: true
          }
        },
        tooltip: {
          callbacks: {
            label: function(c) { return c.dataset.label + ': ' + c.raw + '% of companies'; }
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
        y: {
          min: 0, max: 115,
          ticks: {
            callback: function(v) { return v + '%'; },
            stepSize: 20
          },
          grid: { color: 'rgba(0,0,0,0.05)' }
        },
        x: {
          grid: { display: false }
        }
      }
    }
  });
})();
</script>

---

## Cross-Cycle Validation (Track 3 — FY2016/2017)

Two companies scored on reports from a different time window (2016–2021), using the same frozen rubric. This was the out-of-sample test.

| Ticker | Company | 5-Year Multiple (2016–2021) | Score |
|--------|---------|---------------------------|-------|
| PME | Pro Medicus | **10.96x** | **100** |
| HUB | Hub24 | **6.65x** | **48** |
