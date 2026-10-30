# Human Activity Recognition from Smartphone Sensor Data

AAI 501, University of San Diego. Francois Lubin, Lernik Avetisyan, Alex Denbaugh.

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

## Baseline results (9 unseen test volunteers)

| Model | Accuracy | Macro F1 |
|---|---|---|
| Logistic regression | 0.955 | 0.955 |
| SVM (RBF kernel) | 0.952 | 0.952 |
| Gradient boosting | 0.931 | 0.931 |
| Random forest | 0.929 | 0.927 |
| k-nearest neighbors | 0.880 | 0.877 |

## Data source

Reyes-Ortiz, J., Anguita, D., Ghio, A., Oneto, L., & Parra, X. (2013). *Human activity
recognition using smartphones* [Data set]. UCI Machine Learning Repository.
https://doi.org/10.24432/C54S4K
