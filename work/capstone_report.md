# Capstone Report

**Author:** Faran Khan

**Lane:** Machine Learning

**Repo:** https://github.com/farankhandev/flyrank-ml-internship

**Date:** September 6, 2026

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

The evaluation used a **client-level holdout split** with `GroupShuffleSplit`.

Approximately 75% of the modeling rows were used for training and 25% for testing, while keeping clients separated between the two sets. The final test set contained **51,702 rows from 13 clients**.

This design tests whether the model can generalize to clients that were not included in training.

### Results

The Random Forest and Logistic Regression baseline were evaluated on the **same test set**.

| Metric        | Logistic Regression | Random Forest |
| ------------- | ------------------: | ------------: |
| ROC-AUC       |               0.504 |     **0.604** |
| Precision@50  |               0.460 |     **0.880** |
| Precision@100 |               0.550 |     **0.870** |
| Precision@500 |               0.546 |     **0.840** |

The observed declining-content base rate was **63.3%**.

The Random Forest achieved higher ROC-AUC and substantially higher Precision@K than the baseline. At the top 50 ranked items, **88% were labeled as declining**, compared with **46% for the baseline**.

### Error analysis

The Random Forest was not simply more accurate overall. Its test accuracy was approximately **61.3%**, compared with **65.8%** for Logistic Regression.

This is important because the goal of the project is **prioritization**, not maximizing overall classification accuracy. The model's stronger top-ranked performance is therefore more relevant to the intended human-review workflow.

The results should be treated as measured ranking performance on this holdout test set, not as proof of future content-refresh success.

## 6. Interpretation

The Random Forest identified several signals as important for distinguishing content items labeled as declining.

### Feature importance

The Random Forest feature importances were:

| Feature           | Importance |
| ----------------- | ---------: |
| `rare_share`      |      0.236 |
| `anon_share`      |      0.233 |
| `imp_prev30`      |      0.222 |
| `top_query_share` |      0.192 |
| `visible_queries` |      0.117 |

The largest contributions came from rare-query impression share and anonymized-query impression share, followed by previous 30-day impressions and top-query impression share.

These values describe how much each feature contributed to the Random Forest's decisions. They should be treated as **directional model interpretation**, not as evidence that any feature causes search-performance decline.

### What the model found

The strongest practical result was in the ranking metrics. The Random Forest concentrated more declining items near the top of its ranking than the Logistic Regression baseline.

At the top 50 items, the Random Forest achieved Precision@50 of **0.880**, compared with **0.460** for the baseline.

### Negative result / important surprise

The Random Forest did **not** have higher overall accuracy than the baseline. Random Forest accuracy was approximately **61.3%**, while Logistic Regression accuracy was approximately **65.8%**.

This reinforces that the model's value in this experiment is primarily its ability to **prioritize items for review**, rather than to maximize overall classification accuracy.

The results should not be interpreted as identifying causal drivers of declining search performance.

## 7. Recommendation

The model should be used to **prioritize human review**, not to automatically change content.

### Ranked actions

1. **Review the highest-ranked pages first.**
   Start with pages receiving the highest model scores because the Random Forest achieved a Precision@50 of **0.880** on the holdout test set.

2. **Investigate search and query signals.**
   Review historical impressions and available query-level signals to understand why a high-priority page may be showing signs of decline.

3. **Review the content before making changes.**
   A human reviewer should consider search intent, content quality, freshness, competition, and whether the page still meets user needs.

4. **Use the ranking when review capacity is limited.**
   If a team cannot investigate every page, the ranking can help focus attention on a smaller high-priority group.

5. **Monitor results after any content change.**
   If a page is refreshed, its subsequent search performance should be monitored. The model does not establish that the refresh caused an improvement.

### Recommended workflow

**Model ranking → Human review → Content decision → Refresh/test → Monitor performance**

### Confidence and limits

Confidence in the ranking result is **moderate and experimental**. The Random Forest performed better than the Logistic Regression baseline on ROC-AUC and Precision@K, but the experiment has a potential temporal leakage concern because the available 90-day query signals may overlap with the outcome period.

The model should therefore be treated as a **directional decision-support tool**, not a production forecasting system or an automated content recommendation engine.

## 8. Reproducibility

The project materials are organized in the repository so that the analysis can be reviewed and rerun from a fresh clone.

### Key files

* `work/notebooks/capstone.ipynb` — capstone analysis that mirrors the research paper.
* `notebooks/03_working_with_the_full_release.ipynb` — full-release data analysis and modeling workflow.
* `work/notebooks/` — weekly research, modeling, validation, and recommendation notebooks.
* `work/scripts/` — scripts used for feature preparation, modeling, validation, and report generation.
* `work/README.md` — project and reproducibility documentation.

### Model settings

* Random Forest: 200 trees.
* Random seed: 42.
* Validation: client-level `GroupShuffleSplit`.
* Approximate train/test ratio: 75% / 25%.
* Test set: 51,702 rows from 13 clients.

The analysis does not commit private datasets or credentials to the repository. Hugging Face access credentials are supplied at runtime when required.

### Re-running the analysis

The repository contains the notebooks and scripts needed to review and rerun the analysis.

After cloning the repository, the notebooks can be opened in Jupyter or Google Colab. The full-release notebook loads the hosted data through the documented DuckDB workflow, prepares the modeling features, trains the models, evaluates the results, and generates the paper artifacts.

The final capstone notebook can then be run top-to-bottom with **Runtime → Run all** to verify that the documented analysis remains executable.

All reported metrics in this report correspond to the completed analysis and documented holdout evaluation.



---

> **Claims checklist before submitting:** observed / measured / directional / decision-support
> **Metrics vs. base rate:** report your task's base rate (majority-class %) next to any
> precision@K or accuracy — a high score can just be a high base rate. AUC / lift over
> baseline are the honest discrimination numbers.
> language everywhere · no causal claims without an experiment or causal design · no
> "predicted Google's algorithm" · no client-identifying details · numbers in this report
> match a fresh re-run.
