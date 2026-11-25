# Human Activity Recognition from Smartphone Sensor Data

AAI 501, University of San Diego. Instructor: David Friesen.

## Team

| Name | Contribution |
|---|---|
| Francois Lubin | Classification and feature selection. Data setup and checks, baseline models, volunteer-wise tuning, feature selection, and per-volunteer error analysis (Notebooks 01, 02, 02b); Methods and Results sections for classification and feature selection |
| Lernik Avetisyan | PCA, K-Means and Ward hierarchical clustering, cluster-distance classification, and participant-aware validation |
| Alex Denbaugh | Neural network (MLP) comparison with the other models; assembling the final report (introduction, related work, discussion, limitations, references) and leading the presentation |

We compare classification, clustering, and feature selection methods for recognizing six
activities (walking, walking upstairs, walking downstairs, sitting, standing, laying) from
smartphone accelerometer and gyroscope features.

## Setup

1. Install Python 3.10+ and the requirements:
   ```
   pip install -r requirements.txt
   ```
2. Download the dataset from the UCI Machine Learning Repository:
   https://archive.ics.uci.edu/dataset/240/human+activity+recognition+using+smartphones
3. Extract it (the download contains an inner `UCI HAR Dataset.zip`; extract that too) so the
   folder layout is:
   ```
   data/UCI HAR Dataset/
   ├── features.txt
   ├── activity_labels.txt
   ├── train/  (X_train.txt, y_train.txt, subject_train.txt)
   └── test/   (X_test.txt, y_test.txt, subject_test.txt)
   ```
   The `data/` folder is excluded from git because the dataset is about 270 MB.

## Notebooks

| Notebook | Contents |
|---|---|
| `01_baseline_models.ipynb` | Data checks; baseline logistic regression, k-NN, SVM, random forest, gradient boosting; confusion matrices |
| `02_tuning_and_feature_selection.ipynb` | Volunteer-wise tuning (GroupKFold); accuracy per test volunteer; random forest feature ranking with and without removing correlated features; accuracy vs. number of features |
| `02b_volunteer_error_analysis.ipynb` | Why test volunteers 9 and 10 score lowest: per-person confusion matrices, error types, errors over time, phone-orientation (gravity) features |
| `03_clustering_and_pca.ipynb` | PCA (90% variance), K-Means and Ward clustering, moving/stationary grouping, held-out participant evaluation, and cluster-distance classification |
| 04_neural_network.ipynb | Neural network (MLP): baseline and architecture variants (shallow, deep, L2-regularized); per-volunteer accuracy; sitting vs. standing errors; MLP on 561 features, 150 decorrelated features, and 63 PCA components |

### Running the PCA and clustering notebook

`03_clustering_and_pca.ipynb` is designed for Google Colab. It downloads and extracts the official UCI HAR dataset automatically into `/content/uci_har`, rather than using the repository's `data/` directory. Open it in Colab and run the cells in order. The notebook installs and verifies `scikit-learn==1.8.0`; its saved results were generated in that environment. SciPy is also required for hierarchical clustering and is listed in `requirements.txt`.

## Baseline results (9 unseen test volunteers)

| Model | Accuracy | Macro F1 |
|---|---|---|
| Logistic regression | 0.955 | 0.955 |
| SVM (RBF kernel) | 0.952 | 0.951 |
| Gradient boosting | 0.931 | 0.931 |
| Random forest | 0.929 | 0.927 |
| k-nearest neighbors | 0.880 | 0.877 |

After volunteer-wise tuning (Notebook 02), the SVM reached 95.6% accuracy (C = 100, gamma = 0.0001), the best single result in the project.

## Feature selection results (logistic regression, unseen test volunteers)

| Features kept | Plain RF ranking | Decorrelated RF ranking |
|---|---|---|
| 10 | 53.3% | 87.4% |
| 50 | 88.4% | 92.9% |
| 150 | 93.2% | 93.5% |
| 561 (all) | 95.0% | 95.0% |

"Decorrelated" skips any feature with |correlation| > 0.8 to one already selected.

The 561-feature row (95.0%) is lower than the 95.5% baseline because Notebook 02 uses the tuned setting C = 0.1, chosen by volunteer-wise cross-validation, instead of the default C = 1; compare rows within this table rather than across tables.

## Per-volunteer analysis (Notebook 02b)

Test accuracy ranged from about 83% (volunteer 10) to over 99% (volunteer 24), so the person matters more than the model. Volunteer 9's sitting and standing look almost identical in phone orientation; volunteer 10 had a ~20-second stretch of sitting with an unusual phone tilt that models labeled as standing.

## PCA and clustering results (9 held-out test volunteers where applicable)

| Analysis | Result |
|---|---|
| PCA dimensionality reduction | 63 components retain 90.05% of training variance |
| K-Means moving/stationary ARI (training) | 0.9891 |
| K-Means moving/stationary ARI (held-out testing) | 0.9973 |
| Ward moving/stationary ARI (2,000 training windows) | 1.0000 |
| PCA-only logistic regression (test macro F1) | 0.9220 |
| PCA + cluster distances (test macro F1) | 0.9219 |

The clustering results reflect broad moving and stationary activity groups rather than six-activity classification accuracy. The cluster-distance comparison shows no reliable improvement under the tested configuration. Small differences in Logistic Regression performance may vary across numerical environments.

## Neural network (MLP) results (9 unseen test volunteers)

| Model | Features | Accuracy | Macro F1 |
|---|---|---|---|
| MLP (128, 64), alpha = 0.01 | 561 (all) | 0.947 | 0.947 |
| MLP (128, 64) | 561 (all) | 0.944 | 0.944 |
| MLP (128, 64) | 150 decorrelated | 0.940 | 0.940 |
| MLP (128, 64) | 63 PCA components | 0.916 | 0.916 |

All MLP variants scored within about half a point of each other, and none outperformed logistic regression (95.5%) or the tuned SVM (95.6%). Sitting vs. standing accounts for about half of all MLP errors.

## Data source

Reyes-Ortiz, J., Anguita, D., Ghio, A., Oneto, L., & Parra, X. (2013). *Human activity
recognition using smartphones* [Data set]. UCI Machine Learning Repository.
https://doi.org/10.24432/C54S4K
