# Multi-sport backtesting dashboard

A single-file static dashboard summarising a multi-year backtest of a
placement-based scoring model across 13 Olympic / international sports.

## What it contains

- Sport- and event-level accuracy metrics (CumT8, T4, T8, T16, Spearman ρ)
  across all sliding prediction/validation windows.
- A/B toggle between the baseline prediction pool and a quality-filtered
  variant (meets with fewer than 3 recent OW-T8 athletes dropped).
- Interactive Vega-Lite charts per event: hit-rate trajectory with Wilson CIs,
  calibration by predicted-rank bin, recall view (where T8 finishers came
  from in the pool, including athletes absent entirely), predicted-vs-actual
  scatter per window, top-20 score ladder, and top-8 retention across
  consecutive windows.
- Comparison against alternative models (logistic regression, Lasso,
  Random Forest, Gradient Boosting, GAM) via leave-one-OW-year-out
  cross-validation.
- Methodology tab describing the grid searches, generalisability tests,
  and threats-to-validity analysis that led to the production configuration.

## How it was built

Python 3 + pandas for data prep and sliding-window backtest, SQLite for
intermediate storage, Vega-Lite 5 for client-side interactive plots. The
dashboard is a single HTML file with all data embedded as JSON — no server,
no API calls beyond the Vega-Lite CDN, works from any static host.

## Deploying to GitHub Pages

Drop this folder's contents (`index.html`, `.nojekyll`, `README.md`) at the
root of a GitHub repository, then in **Settings → Pages** set source to
*Deploy from a branch*, branch `main`, folder `/`. GitHub Pages will serve
the dashboard at `https://<username>.github.io/<repo-name>/` within a few
minutes.

The `.nojekyll` file tells GitHub Pages not to run Jekyll preprocessing on
the HTML (which could otherwise silently rewrite filenames starting with
underscores).

## File size

The HTML is ~26 MB uncompressed because the entire dataset is embedded.
GitHub Pages gzips automatically, so the wire transfer is roughly 3–5 MB.
First load may take 2–5 seconds; subsequent loads are cached by the browser.
