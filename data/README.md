# Dataset Instructions

This project uses the European credit-card transactions dataset commonly distributed on Kaggle:

https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

## Expected file

The notebook expects a CSV file named:

```text
creditcard.csv
```

The original dataset used in this project contains 284,807 rows and 31 columns. It includes `Time`, `Amount`, anonymized PCA-transformed features `V1`–`V28`, and the binary target `Class`.

## Setup

1. Open the dataset page and follow its current download/access instructions.
2. Download `creditcard.csv`.
3. Do not commit the CSV to this GitHub repository.
4. In Google Colab, upload or mount the file so it is available at `/content/creditcard.csv`.
5. If running locally, change the notebook's dataset path to your local file location.

The notebook removes 1,081 exact duplicate rows before analysis, resulting in 283,726 rows.

**Important:** This repository does not redistribute the dataset. Review the provider's current terms and any applicable data-use restrictions before downloading or using it.
