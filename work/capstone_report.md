# Capstone Report — <your lane>

- **Author:**
- **Lane:**
- **Repo:**
- **Date:**

> Copy this file to `work/capstone_report.md` and fill it in as you build. The eight
> sections mirror the Pass / Needs-Work rubric axes, so nothing here is optional.

## 1. Problem framing

This project supports the decision of **which content items should receive human review first when their search performance shows signs of decline**.

* **Unit of analysis:** Content item/page.
* **Output:** A model score and ranking of content items by likelihood of belonging to the defined declining-performance group.
* **Human action:** A reviewer uses the ranking to investigate high-priority pages before deciding whether any content change is appropriate.
* **Cost of a wrong call:** A high-priority page may receive unnecessary review, while a genuinely declining page may be missed or reviewed later.
* **Why ML helps:** A large content collection can contain many pages, making manual prioritization inefficient. Machine learning can combine multiple historical and query-level signals to produce a consistent ranking for human review.

The model is a **decision-support tool**, not an automatic content-change system. It does not prove that changing a page will improve future search performance.


## 2. Data safety

This analysis uses the **FlyRank internship warehouse full release** hosted on Hugging Face.

### Data used

The analysis used:

* `dim_clients` — client-level metadata and data-history information.
* `dim_content` — content-level metadata.
* `fact_content_daily_performance` — daily search performance data.
* `fact_content_query_90d` — aggregated content/query-level signals.

The modeling dataset used historical performance and query-level signals from the available data.

### Deliberately excluded data

Client names, domains, URLs, search queries, and other client-identifying information were excluded from the public-facing analysis.

Pseudonymous client identifiers were used only to create the **client-level train/test split**. They were not used as model features.

### Leakage risks

The decline label was based on the change in impressions between the previous 30-day period and the latest 30-day period.

Label-derived fields such as `trend_direction` and `trend_pct` were not used as model features because they would directly reveal information about the outcome.

The query-level signals came from the available 90-day query table. Because this window can overlap with the outcome period, these features create a potential temporal leakage concern. This limitation is explicitly disclosed in the analysis.

No client-identifying information is intentionally included in the public-facing `work/` materials.


## 3. Baseline

The baseline for this analysis is a **Logistic Regression classifier** trained using the same five features and the same client-level train/test split as the Random Forest model.

This provides a fair comparison because both models receive the same input data and are evaluated on the same unseen test set.

On the client-level holdout test set:

| Metric        | Logistic Regression |
| ------------- | ------------------: |
| ROC-AUC       |               0.504 |
| Precision@50  |               0.460 |
| Precision@100 |               0.550 |
| Precision@500 |               0.546 |

The observed declining-content base rate in the modeling dataset was **63.3%**. This provides important context when interpreting the Precision@K results.

The baseline is intentionally simple so that the Random Forest can be evaluated against a transparent, reproducible reference model rather than against an arbitrary or weaker comparison.


## 4. Model / analysis

I trained a **Random Forest Classifier** with 200 trees and a fixed random seed of 42.

### Features

The model used five features:

* `imp_prev30` — previous 30-day impressions
* `visible_queries` — visible query count
* `rare_share` — rare-query impression share
* `anon_share` — anonymized-query impression share
* `top_query_share` — top-query impression share

I deliberately excluded client identifiers and label-derived fields such as `trend_direction` and `trend_pct` from the model features.

### Target definition

A content item was labeled as declining when its latest 30-day impressions were **less than 80% of its previous 30-day impressions**, representing an observed decline of more than 20%.

### Why this model

A Random Forest can capture nonlinear relationships and interactions between the available signals while still providing feature-importance measures for interpretation.

The model is used for **ranking and prioritization**, not for automatically deciding which content should be changed.

## 5. Evaluation

Your split (grouped by client? time-aware?) and why. Metrics, model vs baseline **on the same
split**. What the errors look like — a short error analysis beats a big metric table.

## 6. Interpretation

What the model/clusters actually found. Feature importances or cluster profiles in plain
words. Surprises and negative results — a well-understood "no effect" is a valid result.

## 7. Recommendation

The ranked actions or decisions your output supports, and how a FlyRank editor would use them
tomorrow. State your confidence and the limits explicitly.

## 8. Reproducibility

The exact commands to re-run everything from a fresh clone, your random seeds, and your
environment (`pip freeze` highlights or `requirements.txt` deltas).

---

> **Claims checklist before submitting:** observed / measured / directional / decision-support
> **Metrics vs. base rate:** report your task's base rate (majority-class %) next to any
> precision@K or accuracy — a high score can just be a high base rate. AUC / lift over
> baseline are the honest discrimination numbers.
> language everywhere · no causal claims without an experiment or causal design · no
> "predicted Google's algorithm" · no client-identifying details · numbers in this report
> match a fresh re-run.
