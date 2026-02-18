# Credit Risk Default Model (Probability of Default)

End-to-end Probability of Default (PD) modeling project demonstrating a risk-analyst workflow: portfolio diagnostics → model development → decision policy → stress testing → monitoring plan.

## Executive Summary

### Objective
Build a PD model to support loan approval decisions and demonstrate a practical risk-analytics workflow.

### Data
- **Observations:** 31,677 loans  
- **Target:** `loan_status` (1 = default / bad, 0 = good)  
- **Base default rate:** 21.55%

### Champion model
**Gradient Boosting (GradBoost)** selected based on ranking performance + calibration quality.

### Model performance (held-out test set)
- **AUC:** 0.9318  
- **KS:** 0.7317  
- **Brier:** 0.0600  

### Stability (5-fold stratified CV)
- **CV AUC:** 0.9260 ± 0.0038 (mean ± std)

## Decision policy (example)

Policy rule: **Approve if PD < 0.15**

- **Approval rate:** 71.1%  
- **Bad rate among approved:** 4.93%  
- **% of bad customers declined:** 83.74%  
- **% of good customers kept:** 86.16%

## Stress testing (policy fixed at PD < 0.15)

Scenarios are implemented as feature shocks while holding the approval policy constant.

- **Scenario A:** `loan_int_rate` +2.0pp → approval **-8.76pp**, bad rate among approved **-0.73pp**
- **Scenario B:** `person_income` -10% → approval **-1.96pp**, bad rate among approved **-0.11pp**

Note: In some stress scenarios the approved population becomes more selective (fewer approvals), which can reduce the observed bad rate among approved (selection effect).

## Monitoring plan (production mindset)

- **Data drift:** PSI thresholds **0.20** (investigate) / **0.30** (retrain)
- **Data quality:** schema checks, missingness checks, range checks
- **Performance drift:** AUC/KS tracking over time
- **Backtesting:** PD-band backtesting (predicted vs observed default rates)

## Repository structure

- `risk_cv_ready_fixed.ipynb` — main notebook (end-to-end workflow)
- `data/` — **not included in GitHub** (place dataset locally)
- `.gitignore` — excludes datasets and OS artifacts

## Data

This repository does **not** include the dataset file.

Download the dataset from Kaggle: **Credit Risk Dataset**  
Place the CSV here:

