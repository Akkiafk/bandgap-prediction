# bandgap-prediction
Regeneration of results of the paper “Bandgap prediction of 2D materials using machine learning( doi - https://doi.org/10.1371/journal.pone.0255637 )”

# Bandgap Prediction of 2D Materials using Machine Learning

Reproduction of results from [*Bandgap prediction of 2D materials using machine learning*](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0255637) (PLOS ONE), as a course project. Four regression models are trained to predict the electronic bandgap of 2D materials from computed structural/electronic descriptors, using data derived from the C2DB (Computational 2D Materials Database).

## Overview

The project compares two feature sets:

- **8-feature set** — base descriptors only
- **9-feature set** — base descriptors + `gap_nosoc` (bandgap without spin-orbit coupling), following the paper's approach of using this as an additional predictive feature

For each feature set, four models are trained and tuned:

- Support Vector Regression (SVR) — hyperparameters tuned via `GridSearchCV`
- Random Forest Regressor
- Gradient Boosting Regressor (GBDT)
- Multi-Layer Perceptron (MLP)

Features are scaled with `MinMaxScaler` before training. Models are evaluated with R², MSE, MAE, and RMSE on a held-out test set (90/10 split), plus error-bucket analysis (% of predictions within 0.1 eV, 0.1–0.2 eV, and >0.2 eV of the true value) and parity plots comparing predicted vs. actual bandgap.

## Results

### 8-feature model

| Model | R² | RMSE (eV) | MAE |
|---|---|---|---|
| SVR | 0.738 | 0.417 | 0.244 |
| Random Forest | 0.906 | 0.249 | 0.097 |
| Gradient Boosting | 0.910 | 0.245 | 0.101 |
| MLP | 0.823 | 0.343 | 0.183 |

### 9-feature model (with `gap_nosoc`)

| Model | R² | RMSE (eV) | MAE |
|---|---|---|---|
| SVR | 0.956 | 0.174 | 0.117 |
| Random Forest | 0.982 | 0.109 | 0.037 |
| **Gradient Boosting** | **0.987** | **0.094** | **0.041** |
| MLP | 0.960 | 0.165 | 0.084 |

Adding `gap_nosoc` as a feature substantially improved every model, confirming the paper's finding that it's a strong predictor of the true bandgap. **Gradient Boosting was the best-performing model overall, reaching R² = 0.987.**

## Setup & running

```bash
git clone <your-repo-url>
cd 2d-bandgap-prediction
pip install -r requirements.txt
```

Open either notebook in Jupyter or VS Code and run all cells. (Originally developed in Google Colab — file paths have been updated to relative `data/` paths for local use.)

## What I learned

- Hands-on experience reproducing results from a published paper, including matching preprocessing and hyperparameter choices to get comparable numbers
- Comparative evaluation of classical ML regressors (SVR, RF, GBDT) vs. a neural network (MLP) on a small tabular scientific dataset
- Feature engineering's outsized impact here — a single additional physically-motivated feature (`gap_nosoc`) improved R² by ~0.08–0.22 across all models
- Practical use of `GridSearchCV` for hyperparameter tuning and structured model comparison (error-bucket analysis, parity plots) beyond a single summary metric

## Reference

Original paper: [Bandgap prediction of 2D materials using machine learning, PLOS ONE](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0255637)
