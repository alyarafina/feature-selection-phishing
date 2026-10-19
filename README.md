# Comparison of Filter-Based Feature Selection for Phishing URL Detection Using Random Forest and DNN 

This repository contains the Jupyter Notebook developed for a study investigating feature selection strategies in machine-learning-based phishing URL classification. The experiments examine how Chi-Square, ANOVA F-test, and Mutual Information perform when paired with Random Forest and Deep Neural Network (DNN) classifiers.

## Research Team

- Alya Rafina
- Marrylinteri Istoningtyas
- Irawan

Universitas Dinamika Bangsa, Jambi, Indonesia

## Data Source

The experiments are based on the [PhiUSIIL Phishing URL Dataset](https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+dataset), which provides URL-related attributes for distinguishing phishing attempts from legitimate URLs.

The dataset is not stored in this repository. Users should download it from the original source and comply with the applicable data-use conditions.

## Experimental Setup

The notebook evaluates the following components:

- **Selection techniques:** Chi-Square, ANOVA F-test, and Mutual Information
- **Learning algorithms:** Random Forest and DNN
- **Input dimensions:** 10, 20, 30, 40, or 49 features
- **Validation strategy:** Five-fold Stratified Group Cross-Validation
- **Model selection criterion:** Mean Matthews Correlation Coefficient (MCC)

## Performance Summary

The best configuration for each classifier was selected using cross-validation on the training data and subsequently assessed on the held-out test set.

| Classifier | Selected Features | Cross-Validation MCC | Test MCC |
|---|---|---:|---:|
| Random Forest | Chi-Square (49 features) | 0.999718 | 0.999740 |
| DNN | Mutual Information (20 features) | 0.999392 | 0.999349 |

Although both classifiers achieved high MCC scores, the benefit of feature selection varied by model. Further evaluation on independent datasets and temporally separated URLs is necessary before drawing conclusions about real-world deployment.

## Getting Started

1. Download or clone this repository.
2. Open the notebook in Google Colab or a local Jupyter environment.
3. Install the Python packages required by the notebook.
4. Download the dataset and update the file path as needed.
5. Execute the notebook cells in sequence.

## Reference

Rafina, A., Istoningtyas, M., & Irawan. *Comparison of Filter-Based Feature Selection for Phishing URL Detection Using Random Forest and DNN.* Full publication details will be added when available.

## License

Refer to the `LICENSE` file for the applicable usage and redistribution terms.
