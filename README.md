# Credit Card Fraud Detection

An end-to-end machine learning project that explores highly imbalanced credit-card transaction data, compares classification approaches, optimizes a decision threshold using validation data, and evaluates a final XGBoost model on an independent test set.

## Project Overview

Fraud detection is an imbalanced binary-classification problem: legitimate transactions substantially outnumber fraudulent ones. In this project, the target variable is `Class` (`0` = legitimate, `1` = fraud).

The workflow includes:
- Dataset inspection and duplicate removal
- Exploratory data analysis (EDA)
- Stratified data splitting and feature scaling where appropriate
- Class-weighting and SMOTE experiments
- Comparison of Logistic Regression, Random Forest, Decision Tree, K-Nearest Neighbors, and XGBoost
- Validation-based threshold optimization
- Final XGBoost training and independent test evaluation

## Dataset

This project uses the European credit-card transactions dataset commonly distributed through the Kaggle dataset page:

https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

The dataset contains 284,807 transactions and 31 columns (30 input features and the `Class` target). After removing 1,081 exact duplicate rows, the working dataset contains 283,726 transactions, including 473 fraud cases.

- `Time`: elapsed seconds from the first transaction
- `Amount`: transaction amount
- `V1`–`V28`: anonymized PCA-transformed features
- `Class`: 0 for legitimate transactions and 1 for fraudulent transactions

**The CSV dataset is not included in this repository.** See [`data/README.md`](data/README.md) for download and setup instructions. Check the dataset provider's current terms before downloading or redistributing it.

## Final Test Results

The selected XGBoost model was evaluated on an independent test partition of 42,559 transactions (42,488 legitimate and 71 fraudulent). The classification threshold was selected on validation data and carried forward to the final test.

| Metric | Final test result |
|---|---:|
| Accuracy | 0.9995 |
| Precision | 0.9298 |
| Recall | 0.7465 |
| F1-score | 0.8281 |
| ROC-AUC | 0.9713 |
| Average Precision (PR-AUC) | 0.8304 |

### Final confusion matrix

|  | Predicted legitimate | Predicted fraud |
|---|---:|---:|
| Actual legitimate | 42,484 | 4 |
| Actual fraud | 18 | 53 |

The model identified 53 of the 71 fraud cases in the final test partition and missed 18. It produced 4 false-positive predictions. Accuracy is not considered in isolation because of the severe class imbalance.

## Repository Structure

```text
credit-card-fraud-detection/
├── data/
│   └── README.md
├── notebooks/
│   └── 01_credit_card_fraud_detection.ipynb
├── results/
│   ├── eda/
│   ├── confusion_matrices/
│   ├── roc_curves/
│   ├── precision_recall_curves/
│   └── model_comparison/
├── models/
│   └── final_model/
│       └── README.md
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/credit-card-fraud-detection.git
cd credit-card-fraud-detection
```

### 2. Create and activate a virtual environment (optional but recommended)

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

macOS/Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Download the dataset

Download `creditcard.csv` from the dataset provider and place it at the path expected by the notebook. In Google Colab, the notebook currently expects:

```text
/content/creditcard.csv
```

If running locally, update the dataset path in the notebook to the location of your downloaded CSV.

### 5. Open the notebook

```bash
jupyter notebook notebooks/01_credit_card_fraud_detection.ipynb
```

You can also upload the notebook to Google Colab and run it there.

## Methodology Notes

- The dataset is stratified when split so the rare fraud class is represented across partitions.
- Scaling is fitted on training data only in the experiments that require it.
- SMOTE is applied only to training data, never to validation or test data.
- The earlier exploratory model comparison is kept separate from the later validation/test workflow.
- The threshold (0.31) was selected using validation data. It was not tuned using final test labels.
- The final XGBoost model is trained on the combined training and validation partitions and evaluated on the held-out test partition.

## Limitations

- The dataset is highly imbalanced; a high accuracy score alone can be misleading.
- `V1`–`V28` are anonymized features, limiting domain-level interpretation.
- This is an offline experiment on historical data, not a live payment-processing system.
- The validation-selected threshold was transferred to a model refitted on the combined training and validation data. Refitting may change probability calibration; threshold stability should be checked with further validation or calibration before deployment.
- Performance on one held-out sample does not guarantee performance on future transactions.
- The best operational threshold depends on the relative costs of missed fraud and false-positive alerts.

This repository is an educational machine-learning project and is **not a production-ready fraud prevention system**.

## License

The project code is released under the MIT License. The dataset is not included and remains subject to its provider's terms.
