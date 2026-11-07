# Predicting Airline Satisfaction — Kaggle S6E10

**Course project | Intermediate status report: 13 November 2026**

## Competition, target and metric
- Competition: [Predicting Airline Satisfaction — Playground Series S6E10](https://www.kaggle.com/competitions/playground-series-s6e10)
- Task: binary classification. Predict `satisfaction` (`True` means satisfied).
- Evaluation: **ROC-AUC** of predicted positive-class probabilities (not hard class labels).
- Official final-submission deadline: **31 October 2026**, as published by Kaggle. This competition is now closed for new standard submissions.

## Team and responsibilities

| Member | Proposed role for remaining work | Evidence to provide |
|---|---|---|
| **Emmanuel Gyamfi Kwarteng** | Project coordination, model training, validation, Kaggle evidence and final reporting | Commit model evaluation and final report updates under Emmanuel's GitHub account |
| **Harrison Tenu** | Data dictionary, exploratory data analysis, interpretations | Commit EDA figures/interpretations under Harrison's own GitHub account |
| **Eno Simeon Daniel** | Missing-data audit, preprocessing explanation and constant baseline | Commit data-quality/preprocessing notes and baseline results under Eno's own GitHub account |

**Important:** These are *proposed assignments*, not claims about work already performed by individual members. Agree on duties, make actual edits and commits from each person's account, and replace these proposals with confirmed contributions. Do not manufacture history or have one person impersonate the other members.

## Dataset and data dictionary
- `train.csv`: **699,635 rows × 23 columns** (21 features, `id`, `satisfaction`).
- `test.csv`: **299,844 rows × 22 columns** (21 features and `id`).
- Categorical: `Gender`, `Customer Type`, `Type of Travel`, `Class`.
- Numeric: `Age`, `Flight Distance`, departure/arrival delay (minutes), and 13 ordinal service ratings.
- `id`: unique record key; excluded from predictors.
- Individual predictor meanings and data types: see the feature dictionary in the notebook.

## EDA: verified findings
- Satisfied: **310,339/699,635 = 44.36%**; not satisfied: **389,296 = 55.64%**.
- Satisfaction by cabin: **Business 72.53%**, **Eco 16.76%**, **Eco Plus 24.22%**.
- Business travel: **58.43%** satisfied; personal travel **9.73%**.
- Loyal customers: **49.51%** satisfied; disloyal customers **20.09%**.
- Median `Online boarding`: satisfied **4**, not satisfied **3**.
- These are descriptive associations and do **not** establish causal effects.

## Missingness, anomalies and preparation
- `Arrival Delay in Minutes` missing in **292 training rows**, **130 test rows**; imputed using fold-specific training medians. No other missing values observed.
- No duplicate complete training rows and no duplicate training IDs; negative numeric values were not found in training.
- Extreme positive delays and valid 0–5 rating values are preserved rather than discarded without evidence.
- Categorical variables are handled using CatBoost categories / LightGBM category codes. Optional Logistic Regression uses imputation, one-hot encoding and numeric standardisation. Scaling is unnecessary for boosted decision trees.
- Preprocessing is fitted on the training fold only to protect validation from data leakage.

## Validation and results
**Split:** stratified 80% train / 20% hold-out validation; `random_state=42`; scoring uses ROC-AUC.

| Approach | Validation ROC-AUC | Verification |
|---|---:|---|
| Trivial constant-probability baseline | **0.500000** | Verified independently from uploaded data; notebook cell must be rerun |
| CatBoost | **0.957433** | Output recorded in uploaded executed Colab notebook |
| LightGBM | **0.957573** | Output recorded in uploaded executed Colab notebook |
| 50/50 CatBoost + LightGBM blend | **0.957700** | Output recorded in uploaded executed Colab notebook |

The uploaded `submission.csv` has the correct **299,844** test IDs, two columns, finite probability values between 0 and 1, and no missing values. **This validates its format only — not successful Kaggle acceptance.**

**Actual Kaggle score:** NOT YET VERIFIED.  
**Successful submission URL / screenshot:** NOT YET PROVIDED.

Kaggle listed its final submission deadline as **31 October 2026**, which is earlier than the 13 November status-report date. If the team already entered before the deadline, attach its actual Kaggle submission-history screenshot and leaderboard score. Otherwise ask the lecturer to approve another active competition or a substitute evidence requirement; do not claim a successful submission.

## Files and how to reproduce
- `Airline_Satisfaction_Team_Colab.ipynb` — editable Google Colab notebook.
- `submission.csv` — format-validated probabilities from the supplied files.
- `Intermediate_Status_Report.docx` / `.pdf` — 1–2-page intermediate status report.
- `GITHUB_AND_COLAB_STEPS.md` — step-by-step guide for beginners.
- `requirements.txt` — Python dependencies.
- `EVIDENCE_CHECKLIST.md` — final evidence to collect.

1. Download the data ZIP from the competition (do not upload large datasets to GitHub).
2. Open the notebook in Google Colab, use **Runtime > Run all**, upload ZIP when prompted.
3. Confirm EDA charts, missing-data output, baseline and model ROC-AUC values are visible.
4. Save executed `.ipynb` into GitHub with meaningful version notes.
5. Record any past accepted Kaggle submission evidence, if available.

## Work planned until defence
1. Re-run updated notebook; retain plots, anomaly checks and baseline outputs.
2. Extend to repeated hold-out or stratified cross-validation; compare score variance.
3. Investigate model feature importance and validation errors; review potential overfitting.
4. Finalise report, reproducible repository and individual team commits.
5. Resolve Kaggle-submission evidence with the lecturer due to the closed deadline.


