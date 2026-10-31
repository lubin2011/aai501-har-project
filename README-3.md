# Human Activity Recognition from Smartphone Sensor Data

AAI 501, University of San Diego. Instructor: David Friesen.

## Team

| Name | Contribution |
|---|---|
| Francois Lubin | (to be filled in) |
| Lernik Avetisyan | (to be filled in) |
| Alex Denbaugh | (to be filled in) |

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

## Baseline results (9 unseen test volunteers)

| Model | Accuracy | Macro F1 |
|---|---|---|
| Logistic regression | 0.955 | 0.955 |
| SVM (RBF kernel) | 0.952 | 0.951 |
| Gradient boosting | 0.931 | 0.931 |
| Random forest | 0.929 | 0.927 |
| k-nearest neighbors | 0.880 | 0.877 |

## Feature selection results (logistic regression, unseen test volunteers)

| Features kept | Plain RF ranking | Decorrelated RF ranking |
|---|---|---|
| 10 | 53.3% | 87.4% |
| 50 | 88.4% | 92.9% |
| 150 | 93.2% | 93.5% |
| 561 (all) | 95.0% | 95.0% |

"Decorrelated" skips any feature with |correlation| > 0.8 to one already selected.

## Data source

Reyes-Ortiz, J., Anguita, D., Ghio, A., Oneto, L., & Parra, X. (2013). *Human activity
recognition using smartphones* [Data set]. UCI Machine Learning Repository.
https://doi.org/10.24432/C54S4K
