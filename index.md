---
layout: article
title: "Can You Spot a Great Company Before It Takes Off — Just By Reading Its Annual Report?"
---

# Can You Spot a Great Company Before It Takes Off — Just By Reading Its Annual Report?

*I spent $4 and a few hours running an experiment to find out.*

---

Every year, every publicly listed company publishes an annual report. Most people never read these. They're dry, long, and full of corporate jargon.

But here's the thing: if you're trying to figure out whether a company is going to do well over the next five years, the annual report is one of the few documents where management has to put something real on paper. They have to show their margins, disclose their strategy, say something about where the business is heading — and the next year's report will show whether they delivered. Some companies also choose to disclose their order book (the value of work already contracted but not yet completed) — and that choice, as it turns out, tells you a lot.

I wanted to know: **can you use AI to read these reports and find signals that predict which companies will compound — before they do?** And if this works — can I make it (relatively) cheap? Anyone can dump a bunch of PDF documents into a powerful AI model and get good results. The trick is to do this with a cheaper model so you can target more companies without selling your kidneys.

---

## Before we start — a few definitions

I needed clear rules, not fuzzy ones. So I defined:

- **Winner** = a company whose market cap grew to at least 2.5 times its starting value over a five-year period. A company that went from $200M to $500M qualifies.
- **Control** (the losers) = a company whose market cap ended at 0.8 times or below its starting value. Flat or declining.

I excluded everything in between. A company that went from $200M to $350M isn't clearly a winner or clearly a loser — that middle ground would just confuse the results.

I picked 20 ASX small-caps from a 2021–2026 window: **11 winners** and **9 controls**. The winners ranged from 2.2x to 15x (one documented borderline case at 2.2x, below the 2.5x threshold, kept with a note — full details on the [data page](data/)). The controls ranged from 0.18x (down 82%) to 0.74x (down 26%).

#### Models

For those of you who are not AI nerds, the models used in this experiment:

- **Claude Opus** — powerful (and not cheap) model from Anthropic
- **Claude Sonnet** — less powerful and cheaper to use
- **DeepSeek V4 Pro** — advanced Chinese model
- **DeepSeek V4 Flash** — a cheaper Chinese model

---

## Stage 1: Extraction — Read Everything, Blind

The first problem was: *I didn't know what to look for.* Nobody really does — that's the whole point of the experiment.

So instead of guessing upfront, I designed (or was I lazy and asked Opus to do this?) a reading list. I created a structured template — roughly 40 questions — that an AI would try to answer for every annual report it read. I wasn't asking *will the company win or lose*, I was asking more grounded and specific questions in six categories (we don't know which ones will become important at this stage):

- **Management** — Is this founder-led? What percentage of shares do executives own? Is their tone optimistic and vague, or grounded and specific? Are they more or less confident than last year?
- **Ambition** — Do they describe a specific dollar opportunity, or just say "large addressable market"? Do they name actual contracts, or is everything "in the pipeline"?
- **Operations** — How fast is revenue growing? Are profit margins getting better or worse? How big is the order book (the list of work already contracted but not yet delivered)?
- **Capital discipline** — Are they raising new money constantly? Are they buying other companies, or growing the existing business? Do they pay dividends?
- **Structural tailwinds** — Is this company in an industry where spending is growing (government infrastructure, defence, resources)?
- **Consistency** — Did they promise something last year? Did they deliver?

I picked four AI models to test this activity: Claude Opus, Claude Sonnet, DeepSeek Pro, and DeepSeek Flash. Opus is the most thorough and expensive; Flash is fast and cheap.

Before running all 20 companies, I ran a calibration test. I fed all four models the same annual report — Data#3 (ticker: DTL), an IT services company — and compared their answers against each other. Opus was the reference. The question was: which cheaper model comes closest?

Here's a concrete example of where they diverged. One field in the template was **insider ownership %** — what percentage of the company's shares do its own executives own? This isn't always stated directly. Sometimes you have to find the share registry section, count the shares attributed to directors, divide by total shares on issue, and calculate the number yourself.

Opus did this without being asked. It found the relevant table, did the arithmetic, and returned: *"~3.4% based on director share holdings of 5.8M shares against 172M total shares on issue."*

Sonnet returned almost the same answer — correct number, slightly less detailed sourcing, but within rounding. DeepSeek Pro returned a reasonable estimate. DeepSeek Flash returned `null` — it couldn't find an explicit statement of the percentage and didn't attempt to calculate it from the underlying data.

That pattern repeated across several fields. Any time the answer required genuine inference — calculating a margin trend from a cost breakdown rather than a stated percentage, or detecting that management's tone had become more cautious even though the headline numbers looked fine — Flash either missed it or guessed wrong. Sonnet tracked Opus almost exactly, at roughly a quarter of the cost.

**Sonnet was chosen for the full extraction run.**

I then fed Sonnet 70 annual reports — roughly four years of reports for each of the 20 companies. Critically, **Sonnet had no idea which companies were winners and which were losers.** It was just reading and answering the 40 questions, blind. No outcome knowledge. Its sole task was to create a company profile based on its annual reports.

The result: 70 structured data files, one for each company-year.

*(The full prompts used in both stages — the 40-field extraction template and the targeted five-signal version — are on the [prompts page](prompts/).)*

---

## Stage 2: Which Questions Actually Mattered?

Now I had 70 files, each with ~40 answers. The question was: which of those 40 things actually differed between the winners and the losers? In Stage 1 we didn't know what we were looking for — but if we knew, then maybe we could use a cheaper model to look for it.

To find out, I brought in Opus again — but this time I told it which companies were in which group. I showed Opus the aggregated multi-year profiles of all 20 companies and asked it: *what consistently appears in the winners that doesn't appear in the losers?*

This is the key methodological point. **The reading was blind. The comparison was informed.** Sonnet read the reports without knowing outcomes. Opus compared the groups with full knowledge of who won and who lost — specifically to find what retrospectively separated them.

What Opus found confirmed some things and reversed others.

The clear differentiators: order book trajectory, margin trend, management confidence, and how specifically management described what was coming next. The non-differentiators were just as interesting — more on those later.

But here's the problem. Knowing *what* separates winners from losers is only useful if you can check for it cheaply enough to screen hundreds of companies. Some of the signals Opus found required the kind of reading only an expensive model can do — detecting a subtle shift in tone, inferring a margin trend from buried cost ratios rather than a stated number. Great for analysis. Useless for a scalable screening tool.

So I ran a second pass with one question: which of these signals can a cheap model find too? I took each differentiating field and tested it against DeepSeek Flash — roughly 150 times cheaper than Opus. If Flash got it right consistently, the signal stayed. If it kept missing or guessing, the signal was dropped. Only what's cheap to extract at scale made the cut.

**Five signals survived this filter:**

1. **Order book / contracted backlog** — Is there a specific dollar figure disclosed, and is it growing year-on-year? (Flash can find a number in a table. It can compare it to last year's number.)
2. **Margin trend** — Is the operating margin expanding? (Flash can calculate this from stated revenue and profit figures.)
3. **Execution tone** — Is management language grounded in what they've actually delivered, not just where they want to go? (A simpler version of the broader "confidence trajectory" — Flash can classify language as delivery-focused vs aspirational.)
4. **Specificity of forward guidance** — Are there real numbers attached to next year's targets, or just "we are well-positioned for growth"? (Flash can spot a dollar figure vs a vague statement.)
5. **Organic revenue growth** — Is revenue growing above 10%, from the core business and not from buying other companies? (Direct arithmetic from the financial statements.)

These five became the frozen rubric — **rubric-v1**. Every company was then scored purely on these five signals, year by year. No AI judgment at scoring time. Pure arithmetic: signal present or absent, tallied up.

---

## Stage 3: What the Scores Showed

Winners averaged **52.6 out of 100**. Controls averaged **38.7**. A gap of 14 points.

The scores aren't a black box — each of the five signals is worth a fixed share of 100 points (order book 30%, revenue growth 20%, margin expansion 20%, execution tone 15%, forward guidance 15%), tallied per year. A company that shows all five signals in all four years scores near 100; one that shows none scores near 0. ACF scores 90 because it hit the combination in every year available. NWH scores 63 because it had the order book and tone consistently but margins only expanded in three of four years.

The gap sounds modest. But the interesting finding isn't the average — it's the combination.

When I looked for companies that simultaneously had a growing order book, an execution-focused management tone, and no large acquisitions muddying the organic growth story, the result was stark:

**6 of 11 winners hit this combination at some point. Zero of 9 controls ever did.**

This combination was identified retrospectively by searching for what separated the two groups — which is why the out-of-sample tests later in this piece matter. But even within the training set, the clean zero on the control side is striking.

Not 6 versus 1. Not 6 versus 2. Six versus zero.

Opus — with sector information redacted so it couldn't pattern-match on industry — confirmed the finding when asked to summarise what it saw:

> "Growing order book + tone consistent + execution-focused tone + no capital raise or bolt-on only acquisitions — the 'disciplined operator' combination indicating companies with genuine organic momentum backed by management credibility (A: 6/11, B: 0/9)."

A few real examples of what this looked like on paper:

**NRW Holdings (NWH) — 5.6x over five years.** This is an Australian engineering and construction company. Its order book (the dollar value of contracts already signed but not yet completed) grew from $2.2 billion to $5.2 billion between FY2019 and FY2022. Margins expanded in three of four years. Every year, the CEO's letter contained specific project names, specific workforce numbers, specific schedule milestones. Rubric score: 63.

**DroneShield (DRO) — 15x over five years.** DroneShield makes counter-drone systems — technology to detect and disable drones in military and critical infrastructure settings. In FY2019, the company had just $3.6 million in revenue. But it disclosed an $80 million sales pipeline — 22 times its actual revenue. Every year, the order book grew: from under $1 million to $24 million by FY2022. Management tone was grounded and specific throughout. Rubric score: 65.

**Acrow Limited (ACF) — 2.9x.** The smallest company in the set — $107 million market cap in 2021, which is tiny. All three signals in the disciplined-operator combination — growing order book, execution tone, no large acquisitions — present in every year available. Score: 90. The rubric doesn't care about size.

*(Full scoring table for all 20 companies, plus rubric weights and cross-cycle validation: [data page](data/).)*

---

## What Didn't Separate Them

Some of the reversals here are more useful than the findings that confirmed expectations.

**International expansion, entering new markets — completely useless.** Every single company, every single year, mentioned one or both. When a signal appears in both groups universally, it tells you nothing. It's just baseline corporate language.

**Founder-led companies underperformed (in this dataset).** The conventional wisdom is that founders outperform — skin in the game, long-term thinking. Here it went the other way. Most winners had professional management with an execution-focused tone. Most controls were founder-led. The tone mattered more than who held the title.

**High insider ownership was more common among the losers.** The intuition is reasonable — if the CEO owns a big chunk of the company, they're incentivised to make good decisions. But the companies with the highest insider ownership in this dataset were all controls. Very high ownership in a small company may actually signal that professional investors haven't validated the business yet, which is a yellow flag rather than a green one.

**Aggressive growth language barely discriminated — but the direction surprised me.** Both groups used it, but it was slightly more prevalent among the losers: 7 of 9 controls used aggressive language in most years, versus 6 of 11 winners. The real separation wasn't the presence of ambition — it was what sat alongside it. Winners paired measured language with actual delivery. The losing companies that talked the biggest game rarely backed it up in the next year's report.

**Risk disclosure counts meant nothing.** Both groups disclosed roughly the same volume of risks per year. The number is irrelevant. A company that discloses significant risks while management tone stays consistently execution-focused is probably different from one where the risks and the tone are both deteriorating — but this experiment tested the counts, not that interaction. That's a follow-up question.

---

## Does It Work on Companies I Didn't Train It On?

The rubric was built on FY2019–2022 data — reports from the years leading into and overlapping the measurement window. A company flagged by the rubric in 2019 should, if the signals are predictive, have already been on the radar before the five-year clock started ticking. But the rubric was designed by looking at outcomes. The honest test is whether it works on data it never saw.

I ran two tests, both using the same frozen rubric, both on a different time window (2016–2021) that predates the training data entirely.

**Pro Medicus (PME)** is a medical imaging software company. Score on its FY2016 annual report: **100 out of 100.**

Every signal was present. The company disclosed $100 million in forward contracted revenue — 3.6 times its annual revenue at the time, described as having "doubled over the past 12 months." Operating margins were expanding from 28.6% to 34.2%. Revenue was growing at 57% organically. Management tone was execution-focused, increasingly confident.

The actual quote from the 2016 report:
> "Forward contracted revenue doubled over the past 12 months and now exceeds $100M AUD over the next five years."

PME went from $5.08 per share in 2016 to $55.63 in 2021 — a 10.96x return.

*(What the model actually saw in the 2016 annual report, signal by signal: [PME page](pme/).)*

**Hub24 (HUB)** is an investment platform — technology, not engineering. Score on its FY2016 annual report: **48 out of 100.** No traditional order book (platform businesses don't have contract backlogs in the same way), but expanding margins, execution-focused tone, and specific near-term guidance including a "profit before tax" target. HUB went 6.65x over the same window. A score of 48 puts it above the control average (38.7) but below the winner average (52.6) — appropriately ambiguous for a company that was genuinely compounding but not yet showing all five signals cleanly.

Both tested companies exceeded the winner threshold. Neither was a false positive. The rubric didn't have outcome data for either — it just saw the same annual report any investor could have read in 2016.

---

## What Does It Find When Pointed at Current Companies?

The real test isn't whether the rubric can explain the past. It's whether it finds anything useful when pointed at companies whose outcomes aren't known yet.

I ran the same pipeline on 57 current ASX small-caps — most recent annual report available, same five-signal prompt, same scoring weights. No outcome knowledge. Just: what are these companies showing right now?

Starting from ~2,000 ASX-listed tickers, a quant filter cut the list to companies in the right size range with enough daily trading volume to be investable. Of the ~200 that passed, I was able to get annual report PDFs for 57. The remaining ~140 don't have accessible IR pages.

The top scorer was **QOR (Qoria)** at **100 out of 100** — the only company in the entire dataset, training set included, to match PME's FY2016 score. Qoria makes parental controls and digital safety software for schools. Contracted recurring revenue growing at ~40%, margins expanding, specific annual recurring revenue targets disclosed, entirely organic growth. Every signal present.

Behind it: **NXL (Nuix)**, an investigative analytics company, at 96. **FWD (Fleetwood)**, a modular buildings company with an order book and margin profile almost identical to the training set's engineering winners, at 88.

Three things stood out from the scan:

**The rubric finds companies outside the engineering cluster.** QOR, NXL, and several others are software or SaaS (software-as-a-service) businesses — not the mining services companies that dominated the training set. The signals are showing up in different sectors, which is a good sign the rubric isn't just a proxy for "is this an engineering company in a spending boom." This mirrors what the training set showed: Telix Pharmaceuticals (TLX), a biotech that was one of the 11 training winners, showed the same execution tone and increasing confidence signals as the industrials, even though its "order book" was clinical trial milestones rather than construction contracts.

**PLT (Plenti Group) reappears — but on the other side.** Plenti was a control in the training set, scoring 25 and declining 46%. Its current annual report scores 55. Margin expansion and loan book growth are now present. Whether the business has genuinely turned or the rubric is picking up noise is exactly the kind of question that requires qualitative follow-up.

**57 of ~200 scanned.** The full picture is incomplete. The ~140 companies not yet scored could move the averages in either direction.

The forward results in full — all 12 companies scoring ≥ 50, with what triggered each score — are on the [forward scan page](forward/).

The envelope is sealed. Check back in 2030.

---

## The Hard Case: ALC

Alcidion Group scored **70 out of 100** — highest among the controls, ahead of four actual winners. Quantified pipeline every year, expanding margins, execution-focused tone throughout. Market cap went from $501M to $132M. Down 74%.

The rubric believed the annual reports. The stock market eventually didn't.

Alcidion was genuinely doing what it said it was doing. The problem was its customer base — NHS and Australian public hospital procurement is slow, political, and margin-compressing by design. The business model didn't scale, and no annual report will tell you that directly.

A high score means the company shows up operationally. It doesn't mean the stock market will reward it. The rubric raises the odds in one direction — ALC shows a high score can still lose, and NEU (8.7x on a score of 23) shows a low score can still win. Think of it as a filter that eliminates the weakest candidates, not a guarantee in either direction.

---

## Limitations (the honest version)

**The sector problem.** Six of eleven winners were in Australian engineering and construction, with a seventh (CMM) in gold mining — sectors that all benefited from a massive infrastructure and resources spending surge in Australia over 2021–2026 (a "capex supercycle" — years when government and private spending on infrastructure, mining, and defence all ran high simultaneously). The signals may partly be picking up "was this company in the right industry at the right time" rather than pure company quality. The four non-resources winners in the training set — TLX, NEU, DTL, and DRO (defence) — showed the same signal patterns, which is encouraging. But with 7 of 11 concentrated in one macro tailwind, this limitation is real.

**Twenty companies.** This is an experiment, not a study. The right follow-up is scoring the next 100 small-caps today and tracking what happens in five years, with no knowledge of outcomes at scoring time.

**Annual reports are written by companies.** Management has an incentive to present their pipeline and margins in the best light. The rubric has no way to check whether an order book is real, whether margins are being calculated fairly, or whether revenue quality is what it appears. It takes the document at face value.

**Market cap is a noisy success measure.** I defined winners and losers by what the market did to the share price over five years — not by what happened to actual revenue or earnings. A company can compound on sentiment alone, or get re-rated by the sector even if its own fundamentals are average. A cleaner version of this experiment would measure real business outcomes — revenue growth, return on capital — rather than what the market decided to pay for the company at two points in time.

---

## What I'd Do Next

The rubric is a filter, not a verdict. In practice I'd use it as a two-stage screen: anything below 40 gets deprioritised; anything above 40 gets qualitative work on whether the business model actually scales.

The most important missing piece is a biotech version. NEU (Neuren Pharmaceuticals) scored 23 on this rubric despite being an 8.7x winner. The signals that predicted its success — an FDA approval pathway, a strong patent estate, a licensing deal structure — don't appear in annual reports in a form this rubric can read. A biotech-specific module would need to treat clinical milestones as the equivalent of an order book, and regulatory pathway progress as the equivalent of forward guidance.

The other next step is the only one that would really validate this: score 50 companies now, open the envelope in 2030, report back.

---

*Total cost: ~$4 for the core methodology test (model comparison, rubric validation, Opus comparison); ~$22 for the full pipeline including all ~80 annual report readings, forward scoring, and infrastructure build.*

*All scripts, training sets, extraction outputs, and scoring results are in the [5houses pipeline repository](https://github.com/grumpyclimber/5houses).*
