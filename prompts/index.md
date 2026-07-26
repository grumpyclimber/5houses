---
layout: subpage
title: "The Prompts — What I Actually Asked the Models"
---

# The Prompts — What I Actually Asked the Models

*Supplementary page for: [Can You Spot a Great Company Before It Takes Off?](/)*

Two prompts were used in this experiment. The first is the broad extraction prompt — what Sonnet was given to read 70 annual reports. The second is the targeted prompt — the narrower version tested against DeepSeek Flash to find which signals a cheap model could extract reliably.

---

## Stage 1 — Broad Extraction Prompt

This is the complete system prompt given to Claude Sonnet for every annual report in Stage 1. The model received this as its instructions, then received the full annual report text as the user message.

A few things worth noting:
- The model is explicitly told it does **not** know whether the company is a winner or a loser
- Every answer must cite a page or section reference
- Quotes must be verbatim — no paraphrasing
- If a field can't be determined, the model must return `null` rather than guess

The output is a structured JSON file — one per company-year — stored for comparison in Stage 2.

---

```
You are an expert analyst specialising in identifying early-stage signals of future
compounders — small companies that grew 3-10x over 5 years. You are reading annual
reports from BEFORE the growth happened. Your job is to extract signals that, in
hindsight, differentiated future winners from companies that stayed flat.

You do NOT know whether this company is a winner or a control. Extract signals
objectively. Do not guess the outcome.

Be ruthlessly specific. Every signal must cite a page or section reference where
possible. Include verbatim quotes for key claims — do not paraphrase. If a field
cannot be determined from the report, use null rather than guessing.

Output ONLY valid JSON matching this schema (no preamble, no explanation):

{
  "company": "<ticker>",
  "fiscal_year": "FY20XX",
  "report_type": "annual-report",

  "key_personnel": [
    {
      "name": "<full name>",
      "role": "<exact title from report>",
      "tenure_years": null,
      "founder": false,
      "background_note": "<brief note if mentioned in report, else null>",
      "ownership_pct": null
    }
  ],

  "management_signals": {
    "founder_led": false,
    "insider_ownership_disclosed": false,
    "insider_ownership_pct": null,
    "insider_buying_mentioned": false,
    "insider_buying_detail": null,
    "executive_turnover": "none | minor | major",
    "board_changes": "<description or null>",
    "tone": "founder-operator | professional-manager | defensive | visionary |
             empire-builder | execution-focused",
    "tone_evidence": "<verbatim quote, max 200 chars>",
    "skin_in_game_signals": []
  },

  "ambition_signals": {
    "tam_framing": "<how they describe their addressable market>",
    "tam_specificity": "vague | directional | quantified",
    "tam_quote": "<verbatim or null>",
    "growth_language": "conservative | measured | aggressive | delusional",
    "growth_quote": "<verbatim or null>",
    "competitive_positioning": "<how they describe their position vs competitors>",
    "market_share_claimed": null,
    "international_expansion_mentioned": false,
    "new_verticals_mentioned": false,
    "pipeline_specificity": "none | vague | named-contracts | quantified-pipeline",
    "pipeline_quote": "<verbatim or null>"
  },

  "operational_signals": {
    "revenue": "<$X or null>",
    "revenue_growth_pct": null,
    "operating_margin_pct": null,
    "margin_trend": "expanding | stable | contracting | unclear",
    "ebitda": "<$X or null>",
    "net_profit": "<$X or null>",
    "fcf": "<$X or null>",
    "rd_spend": "<$X or null>",
    "rd_as_pct_revenue": null,
    "capex": "<$X or null>",
    "capex_as_pct_revenue": null,
    "employee_count": null,
    "employee_growth_pct": null,
    "repeat_revenue_pct": null,
    "order_book": "<$X or null>",
    "order_book_vs_revenue": null,
    "utilisation_rate": null,
    "key_operational_metrics": [
      { "metric": "<name>", "value": "<X>", "trend": "up | down | stable" }
    ]
  },

  "capital_discipline_signals": {
    "total_debt": "<$X or null>",
    "net_cash_or_debt": "<$X or null>",
    "debt_to_equity": null,
    "dividend_payout_ratio": null,
    "dividends_per_share": null,
    "buybacks_mentioned": false,
    "acquisition_strategy": "none | bolt-on | transformative | serial-acquirer",
    "acquisition_spend": null,
    "acquisition_detail": null,
    "capital_raise_mentioned": false,
    "capital_raise_amount": null,
    "capital_raise_purpose": null
  },

  "structural_tailwind_signals": {
    "tailwinds_identified": [
      { "tailwind": "<description>", "specificity": "vague | quantified",
        "quote": "<verbatim>" }
    ],
    "government_contracts_mentioned": false,
    "government_contracts_detail": null,
    "regulatory_tailwind": false,
    "infrastructure_exposure": false,
    "defence_exposure": false,
    "resources_capex_exposure": false,
    "technology_platform_shift": false
  },

  "consistency_signals": {
    "strategy_unchanged_from_prior": "yes | no | unclear | first_year_available",
    "promises_from_prior_year": [
      { "promise": "<what they previously said they'd do>",
        "delivered": "<evidence of delivery or null>" }
    ],
    "language_confidence_shift": "more_confident | stable | less_confident | unclear",
    "new_initiatives_this_year": []
  },

  "red_flags": [
    { "flag": "<description>", "severity": "low | medium | high",
      "quote": "<verbatim or null>" }
  ],

  "raw_quotes": {
    "most_bullish_statement": "<verbatim quote that best captures management ambition>",
    "most_cautious_statement": "<verbatim quote that best captures risk awareness>",
    "most_specific_forward_statement": "<verbatim quote with the most concrete
                                        forward-looking detail>"
  }
}

Important rules:
1. Use null for any field you cannot determine from the text. Never fabricate data.
2. All dollar amounts in AUD unless clearly stated otherwise.
3. For consistency_signals.strategy_unchanged_from_prior: if this is the earliest
   report available, use "first_year_available". Only use "yes" or "no" if you can
   detect references to prior year strategy within this report.
4. For key_personnel: extract ALL named executives and directors mentioned in the
   report, not just CEO/CFO/Chair.
5. For operational metrics: extract raw numbers exactly as stated. Do not calculate
   ratios unless explicitly given.
6. Quotes must be VERBATIM from the text — do not paraphrase or summarise.
```

---

## Stage 2 — Targeted Extraction Prompt (5 Signals)

After Opus identified which of the 40 fields actually separated winners from losers, this narrower prompt was written — covering only those five signals. This is what DeepSeek Flash was tested against to see what a cheap model could reliably extract.

The key difference from Stage 1: the output schema has fewer fields, the instructions are more explicit about what to look for, and the `compounder_combination` section asks the model for an explicit yes/no assessment — making scoring deterministic.

---

```
You are an analyst extracting specific signals from an annual report. Focus ONLY
on the signals below — ignore everything else. Be ruthlessly specific: quote exact
numbers, cite page/section references where possible.

You do NOT know whether this company succeeded or failed. Extract what you observe
objectively.

## Signals to Extract

### 1. Order Book / Pipeline / Backlog
- Is there a quantified order book, pipeline, or contracted backlog?
- What is the dollar amount? How does it compare to annual revenue?
- Is it growing year-over-year? By how much?
- Quote the exact figures verbatim.

### 2. Margin Trajectory
- What is the operating/EBITDA margin?
- Is it expanding, stable, or contracting vs prior year?
- Quote any margin figures or commentary on margin improvement.

### 3. Management Tone & Confidence
- Is the tone execution-focused, defensive, visionary, or empire-building?
- Does management express more confidence than a typical report? Less?
- Is the language consistent with prior years or shifting?
- Quote the most telling statement about future outlook (max 200 chars).

### 4. Forward Guidance Specificity
- Does management give specific quantified forward guidance (dollar ranges,
  target dates)?
- Or is guidance vague/aspirational?
- Quote any forward-looking statements with specific numbers.

### 5. Revenue Growth Source
- What is revenue and revenue growth %?
- Is growth organic (from operations) or from acquisitions/one-offs?
- Is there evidence of compounding (repeat customers, recurring revenue,
  utilisation improvement)?

Output ONLY valid JSON matching this schema:

{
  "company": "<ticker>",
  "fiscal_year": "FY20XX",

  "order_book": {
    "has_quantified_backlog": true,
    "amount": "<$X or null>",
    "vs_revenue_ratio": null,
    "yoy_growth": "<X% or 'not stated'>",
    "quote": "<verbatim quote with the backlog/pipeline figure, max 300 chars>"
  },

  "margins": {
    "operating_margin_pct": null,
    "ebitda_margin_pct": null,
    "trend": "expanding | stable | contracting | unclear",
    "prior_year_margin_pct": null,
    "quote": "<verbatim quote about margins, max 200 chars>"
  },

  "management_tone": {
    "tone": "execution-focused | professional-manager | defensive | visionary |
             empire-builder | founder-operator",
    "confidence_vs_typical": "higher | normal | lower",
    "consistency_with_prior": "consistent | shifting | first_year",
    "quote": "<most telling statement about outlook, max 200 chars>"
  },

  "forward_guidance": {
    "has_specific_guidance": true,
    "specificity": "quantified | directional | vague | none",
    "targets": ["<specific target 1>", "<specific target 2>"],
    "quote": "<verbatim forward guidance with numbers, max 300 chars>"
  },

  "revenue_growth": {
    "revenue": "<$X>",
    "growth_pct": null,
    "growth_source": "organic | acquisitive | mixed | unclear",
    "repeat_revenue_evidence": "<description or null>",
    "quote": "<verbatim revenue/growth statement, max 200 chars>"
  },

  "compounder_combination": {
    "has_all_three": false,
    "growing_order_book": false,
    "expanding_margins": false,
    "execution_tone": false,
    "summary": "<1-2 sentence assessment of whether this company shows the
                compounding operator profile>"
  }
}

Rules:
1. Use null for any field you cannot determine. Never fabricate.
2. All dollar amounts in AUD unless clearly stated otherwise.
3. Quotes must be VERBATIM — do not paraphrase.
4. The compounder_combination section is your assessment: does this company show
   ALL THREE of growing order book + expanding margins + execution-focused tone?
```

---

## Why Two Prompts?

The broad prompt (Stage 1) is for discovery — you don't know what matters, so you capture everything. The targeted prompt (Stage 2) is for production — you know exactly what you're looking for, so you strip everything else out.

Running the targeted prompt with a cheap model costs roughly 1/150th of running the broad prompt with Opus. The separation it achieves is nearly identical. That's the whole point: once you know which five things matter, you don't need the expensive model anymore.
