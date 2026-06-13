# Interpretable Nephropathy Screening

> Machine learning pipeline for early diabetic nephropathy screening using an Explainable Boosting Machine (EBM) on large-scale electronic health records.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)
[![Kaggle Notebook](https://img.shields.io/badge/Kaggle-Notebook-20BEFF?logo=kaggle)](https://www.kaggle.com/)

---

## Overview

This repository contains the full reproducible pipeline associated with the manuscript:

> **"Interpretable Screening of Diabetic Nephropathy Using Explainable Boosting Machine on Electronic Health Records"**  
> *Submitted to a SAGE journal.*

We compare five classifiers — **EBM, Random Forest, XGBoost, LightGBM, and Gradient Boosting** — trained on the **DiabetIA dataset** (IMSS Michoacán EHR, 42,181 records), addressing class imbalance (prevalence ≈ 2.58 %) via SMOTE inside a rigorous nested cross-validation framework.

---

## Repository Structure
---

## Methodology

### Dataset — DiabetIA
- Source: Electronic health records, IMSS Michoacán, México
- Size after preprocessing: **42,181 patients × 13 variables**
- Target: ICD-10 code `e112` (diabetic nephropathy)
- Natural prevalence: **2.58 %**

### Pipeline (3 phases)

**Phase 1 — Data Preparation**
- Variable selection (13 features retained; 10 removed for >40 % missingness)
- Imputation: median (numerical) / mode (categorical)
- Encoding: one-hot encoding
- Normalization: MinMaxScaler

**Phase 2 — Modelling**
- Hold-out split: 80 % train / 20 % test (stratified)
- Class balancing: **SMOTE** applied *only* inside training folds (no leakage)
- Hyperparameter tuning: **RandomizedSearchCV** (n_iter=10, scoring=ROC-AUC)
- Model selection: **Nested Cross-Validation** (outer k=5, inner k=2)
- Models: EBM, Random Forest, XGBoost, LightGBM, Gradient Boosting

**Phase 3 — Evaluation & Explainability**
- Metrics: ROC-AUC, PR-AUC, F1, Balanced Accuracy, Brier Score
- Uncertainty: **Bootstrap 95 % CI** (1,000 resamples) on held-out test set
- Two test scenarios: natural prevalence (n=8,437) and balanced (n=437)
- Calibration: reliability diagrams (10 bins)
- Explainability: **SHAP** values for Random Forest; native EBM shape functions

---

## Key Results

| Model | ROC-AUC (Natural) | PR-AUC (Natural) | Brier Score |
|---|---|---|---|
| Random Forest | **0.922** | **0.395** | 0.0233 |
| XGBoost | 0.894 | 0.267 | — |
| EBM | 0.887 | 0.189 | **0.0257** |
| LightGBM | 0.886 | 0.202 | — |
| Gradient Boosting | 0.838 | 0.105 | 0.1082 |

EBM achieves competitive discrimination with full interpretability via shape functions. Random Forest (highest AUC) is used for SHAP-based global and local explanations.

---

## Requirements
Install all dependencies:
```bash
pip install -r requirements.txt
```

---

## How to Run

### Option A — Kaggle (recommended, free GPU/CPU)
1. Upload the DiabetIA dataset as a Kaggle dataset.
2. Import `diabetic_nephropathy_pipeline.ipynb` into a new Kaggle notebook.
3. Run all cells (estimated runtime: ~2–4 hours on CPU).

### Option B — Local
```bash
git clone https://github.com/HerGa35/interpretable-nephropathy-screening.git
cd interpretable-nephropathy-screening
pip install -r requirements.txt
jupyter notebook notebooks/diabetic_nephropathy_pipeline.ipynb
```

> **Note:** The DiabetIA dataset contains sensitive health records and is not publicly distributed. Contact the corresponding author for data access.

---

## Citation

If you use this code or methodology, please cite:

```bibtex
@article{hernandez2026interpretable,
  title   = {Interpretable Screening of Diabetic Nephropathy Using Explainable
             Boosting Machine on Electronic Health Records},
  author  = {Hernandez-Gaspar, Simon and  Hernández-Ocaña, Betania and  Chávez-Bosquez, Oscar and García-Avalos, Maricela},
  journal = {SAGE Journal},
  year    = {2026}
}
```

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
