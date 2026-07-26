# 5houses — Research Article Site

GitHub Pages site for the article *Can You Spot a Great Company Before It Takes Off — Just By Reading Its Annual Report?*

**Live site:** https://grumpyclimber.github.io/5houses/

---

## Structure

```
index.md          Main article
prompts/          The two AI prompts used (Stage 1 broad + Stage 2 targeted)
data/             Training set, rubric weights, full scoring table
pme/              Pro Medicus FY2016 — signal-by-signal out-of-sample breakdown
forward/          Forward scan — 57 current ASX small-caps scored
```

## Tech

Plain Jekyll with custom layouts — no theme. Two layouts:

- `_layouts/article.html` — main article (no back link)
- `_layouts/subpage.html` — subpages (injects ← Back to article)

All styles in `assets/css/style.css`. No JavaScript.

## Source content

The markdown source files live in the pipeline repo:

```
apps/5houses/scripts/signal/BLOG-DRAFT-V2.md
apps/5houses/scripts/signal/subpages/
```

To update the site after editing source files, copy the updated content into the corresponding `index.md` files here and push to `main`. GitHub Pages rebuilds automatically on push.
