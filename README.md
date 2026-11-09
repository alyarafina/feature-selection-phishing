# Comparative Filter-Based Feature Selection for Phishing Detection Using Random Forest and Deep Neural Network

This repository contains the Jupyter Notebook for a study that compares three filter-based feature selection methods (Chi-Square, ANOVA F-test, and Mutual Information) combined with Random Forest and a Deep Neural Network (`MLPClassifier`) on the PhiUSIIL Phishing URL Dataset.

All experiments are in **`Comparative-Feature-Selection-Phising.ipynb`**. The earlier version of the notebook remains available in release v1.1.0.

## Research Team

- Alya Rafina
- Marrylinteri Istoningtyas
- Irawan

## Data Source

[PhiUSIIL Phishing URL Dataset](https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+dataset) (UCI ID 967; Prasad & Chandra, 2024). The dataset is not stored in this repository; the notebook downloads it with `ucimlrepo`. Users should comply with the data-use conditions of the original source.

## Experimental Protocol

- **Cleaning:** 425 duplicate URLs removed (235,370 URLs remain). From 55 columns, the target is separated (54), `URL`, `Domain`, `Title`, and `URLSimilarityIndex` are excluded (50), and `select_dtypes` drops the non-numeric `TLD` column, leaving **49 numerical predictors** (19 binary, 21 integer, 9 continuous).
- **Grouping:** registered parent domain (eTLD+1) obtained with `tldextract` and its bundled ICANN Public Suffix List (175,514 groups), so subdomains of one registered domain never cross a split.
- **Split:** the first fold of `StratifiedGroupKFold(n_splits=5, shuffle=True, random_state=42)` is the frozen test partition (47,074 URLs); 5-fold `StratifiedGroupKFold` on the training partition (188,296 URLs) is used for model selection.
- **Pipeline (refitted inside every fold, identical for all selectors and baselines):** median imputation → MinMaxScaler → `SelectKBest` (k = 10, 20, 30, 40, or all 49; no selector for the baselines) → StandardScaler (DNN only) → classifier.
- **Mutual Information:** `discrete_features` = mask of the 19 binary indicators, `n_neighbors=3`, `random_state=42`.
- **Classifiers:** Random Forest (300 trees, `max_features="sqrt"`, `class_weight="balanced"`); MLP 128-64-32, ReLU, Adam, learning rate 0.001, batch size 256, `max_iter=100`, built-in early stopping (`validation_fraction=0.15`, row-based, monitored by accuracy).
- **Selection criterion:** highest mean cross-validation Matthews Correlation Coefficient (MCC); the test partition is evaluated once after selection.
- **Additional analyses:** provenance check of `TLDLegitimateProb`/`URLCharProb`, univariate label-proxy screening, train–test duplicate vectors, CV without `TLDLegitimateProb`/`URLCharProb`, alternative MI settings, domain-grouped early stopping for the DNN, refit variability over six seeds, and training/inference time and model size.

## Results Summary

| Classifier | Selected configuration | CV MCC (mean ± SD) | Test MCC (seed 42) | Test MCC, 6 seeds (mean ± SD) |
|---|---|---:|---:|---:|
| Random Forest | Mutual Information, Top-40 | 0.999664 ± 0.000169 | 0.999132 | 0.999407 ± 0.000195 |
| Deep Neural Network | No feature selection (49 features) | 0.999436 ± 0.000198 | 0.995795 | 0.998229 ± 0.001695 |

- Feature selection did not materially improve MCC over the 49-feature baselines (RF baseline CV MCC 0.999620).
- Mutual Information was the most robust filter under strong reduction (Top-10 CV MCC for RF/DNN: MI 0.9935/0.9905, ANOVA 0.9650/0.9629, Chi-Square 0.9356/0.9343).
- The DNN test result depends on the random seed; see the seed-variability section of the notebook.

These scores describe held-out registered domains within PhiUSIIL. Cross-dataset and temporal validation are required before drawing conclusions about real-world deployment.

## Getting Started

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute Comparative-Feature-Selection-Phising.ipynb --output executed.ipynb
```

Alternatively, open the notebook in Jupyter or Google Colab and run the cells in order. All outputs (CSV tables, JSON files, and SVG/PNG figures) are written to `hasil_phishing_sederhana/`. Per-fold results are saved as checkpoints in `hasil_phishing_sederhana/checkpoints/`, so an interrupted run resumes without recomputing completed folds. A full run takes about 1.5–2 hours on a 12-thread CPU with 12 GB RAM.

The reported results were produced with Python 3.13.7, scikit-learn 1.9.0, NumPy 2.3.5, pandas 3.0.5, matplotlib 3.11.1, seaborn 0.13.2, joblib 1.5.2, tldextract 5.4.0, and ucimlrepo 0.0.7.

## Reference

Rafina, A., Istoningtyas, M., & Irawan. *Comparative Filter-Based Feature Selection for Phishing Detection Using Random Forest and Deep Neural Network.* Full publication details will be added when available.

## License

This project is released under the MIT License; see the `LICENSE` file.
