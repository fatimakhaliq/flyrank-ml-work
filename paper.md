# Google Search Ranking & Discoverability: A Signal Analysis

## Abstract
This project investigates which observable content signals — specifically word count and search impressions — are associated with content performance trends on a real, anonymized slice of FlyRank search data. Using the starter dataset, we examined correlations between page-level signals and trend direction, then built a transparent baseline scoring rule to rank content for review. Results show that word count alone is not a strong predictor of ranking trend, while impression volume shows more meaningful variation, suggesting review prioritization should weigh visibility over content length alone.

## Introduction / Problem Statement
Content teams need a way to decide which pages deserve review first when they have limited time. This project supports that decision by testing whether simple, observable signals (like word count and impressions) can reliably flag pages worth reviewing — before jumping to complex modeling.

## Data
This analysis uses the small anonymized starter dataset shipped in the FlyRank ML Internship starter repo (`data/raw/content_refresh_anonymized.csv`). Access to the full Hugging Face warehouse release (`FlyRank/internship-warehouse`) was requested but blocked by a persistent rate-limit (HTTP 429) error during the internship period, so this analysis relies on the starter dataset only. Fields used include `word_count`, `impressions_90d`, `avg_position`, `ctr`, and `trend_direction`. Product-decision fields (e.g., `health_score`, `priority_score`) were deliberately excluded, since they represent the app's own conclusions rather than raw observable signals.

## Methodology
- **Unit of analysis:** one content page, measured over a defined window.
- **Label/proxy:** `trend_direction` (down/flat/new/stable/up), used as a directional signal, not a causal outcome.
- **Signal checks:** median word count and impressions were compared across trend direction groups.
- **Baseline rule:** a simple weighted score combining normalized impressions and a binary "declining" flag, producing a reason code (`declining_with_demand`) and an action label (`review_for_refresh`).
- **Validation caution:** all claims are observational; no causal or algorithmic claims are made.

## Results
- Word count: 'down' and 'up' trending pages showed nearly identical median word counts, indicating length alone does not explain ranking trend movement.
- Impressions: pages with higher impression volume showed more meaningful variation by trend direction, supporting its use as a review-priority signal.
- A baseline decision-tree model (from earlier notebook work) achieved a Precision@50 of approximately 0.540, compared to 0.240 for hand-written rules on the same starter slice — showing a learned ranking can outperform a fixed rule.

## Limitations & Honest Framing
- All findings are **observed and directional**, not causal.
- Results come from a small (~30,000-row) anonymized starter slice, not the full 79-million-row warehouse.
- Full warehouse access was not available during this internship due to a Hugging Face rate-limit issue, which is documented and disclosed here for transparency.
- No claim is made that any signal *causes* ranking changes.

## Ranked Recommendations
1. Prioritize pages with high impression volume and a declining trend for review first.
2. Do not use word count alone as a refresh-priority signal — it showed no clear relationship with trend direction in this data.
3. Future work should validate these findings against the full warehouse dataset once access is available.

## Reproducibility
All notebooks are available in this repository under `work/notebooks/`:
- `01_first_look_and_discovery.ipynb`
- `02_your_first_readable_model.ipynb`
- `w03_data_contract.ipynb`
- `w04_baseline_score.ipynb`

Repository: https://github.com/fatimakhaliq/flyrank-ml-work

## Acknowledgments & Data Credit
Built on the FlyRank ML Internship dataset. Data source: [https://flyrank.ai](https://flyrank.ai)
