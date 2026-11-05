# Evaluating Machine Learning Models for Customer Purchase Prediction

## Project overview

This project investigates whether demographic and transactional characteristics can help explain and predict customer purchase amounts. It builds on my undergraduate BSc Data Science coursework and uses the original `shopping_trends.csv` dataset containing **3,900 observations and 19 columns**.

The analysis explores customer age, product category, season and spending patterns, then compares a mean-prediction baseline with Linear Regression and Random Forest Regression. The focus is on **reproducible empirical analysis and honest evaluation**, including results where machine learning does not improve on a simple benchmark.

## Project Background

This project is an improved and extended version of my original BSc Data Science assignment. Both versions use the same customer shopping dataset to investigate purchasing behaviour.

The updated version improves data preprocessing, categorical encoding, and model evaluation, incorporating baseline comparisons and cross-validation to strengthen the reliability of the analysis.

**[View my original university project report on Dropbox](https://www.dropbox.com/scl/fi/iy1oyupju667s4z8b5fh1/Evaluating-Machine-Learning-Models-for-Customer-Purchase-Prediction.pdf?rlkey=5koq5vszwf7c8s0brh7wbvib4&st=6b4lcmsd&dl=0)**

## Research question

**To what extent can demographic and transactional characteristics explain and predict customer purchase amounts?**

## Research objectives

1. Explore associations between customer characteristics and purchase amounts using exploratory data analysis (EDA).
2. Build and compare Linear Regression and Random Forest Regression against a mean-prediction baseline.
3. Evaluate generalisation using a held-out test set and five-fold cross-validation.
4. Interpret model performance critically and identify limitations of the available observational data.

## Dataset

- **File:** `shopping_trends.csv`
- **Size:** 3,900 rows × 19 columns
- **Prediction target:** `Purchase Amount (USD)`
- **Selected exploratory dimensions:** age, category, season and purchase amount
- **Data quality checks in the notebook:** missing values and duplicate customer IDs
- **Original dataset:** [Consumer Behavior and Shopping Habits Dataset on Kaggle](https://www.kaggle.com/datasets/zeesolver/consumer-behavior-and-shopping-habits-dataset)

**Data rights:** The dataset source is credited above, but its current licence and redistribution terms have **not yet been verified**. Check the Kaggle dataset licence before uploading the CSV to a public repository. If redistribution is not permitted, omit the CSV and provide download instructions instead.

## Methods

The notebook implements:

- An **80/20 random train/test split**, with `random_state=42`.
- A preprocessing pipeline with **one-hot encoding** for categorical variables, fitted on training data only.
- Exclusion of `Customer ID` and `Review Rating` from predictors.
- A **mean-prediction baseline**, **Linear Regression**, and **Random Forest Regression**.
- **Five-fold cross-validation on training data only**.
- Holdout evaluation using **MAE**, **RMSE** and **R²**.

The dataset is cross-sectional, so the analysis does not constitute time-series forecasting. Predictive associations should not be interpreted as causal effects.

## Results

### Held-out test set

| Model | MAE (USD) ↓ | RMSE (USD) ↓ | R² ↑ |
|---|---:|---:|---:|
| **Baseline (mean)** | **20.756** | **23.731** | **-0.006** |
| Linear Regression | 20.945 | 24.049 | -0.034 |
| Random Forest | 21.027 | 24.089 | -0.037 |

### Five-fold cross-validation (training set)

| Model | Mean CV MAE (USD) ↓ | SD of CV MAE | Mean CV R² ↑ |
|---|---:|---:|---:|
| **Baseline (mean)** | **20.567** | 0.214 | **-0.001** |
| Linear Regression | 20.725 | 0.334 | -0.049 |
| Random Forest | 20.718 | 0.237 | -0.033 |

*Numbers are reproduced from the saved outputs in `customer_purchasing_research.ipynb`; results depend on the specified split, preprocessing and model configuration.*

## Discussion

The **mean-prediction baseline** achieved the lowest holdout MAE and RMSE. Neither Linear Regression nor Random Forest improved on this benchmark in the evaluated setup. Five-fold cross-validation produced a similar ordering by MAE, suggesting that the result is not confined to the single held-out split.

All three holdout R² values were negative. R² compares model errors with the error from predicting the **mean of the test targets**. The baseline in this project instead predicts the **training-set mean**, so its holdout R² can also be slightly negative if the two sample means differ.

These findings **do not prove that customer characteristics have no relationship with spending**. They indicate that the selected predictors and models did not deliver useful out-of-sample improvements over a simple baseline in this experiment. Possible explanations include weak predictive signal, omitted relevant factors, dataset limitations, and model specification.

## Limitations and future work

- **Observational data:** Associations cannot establish causal effects of age, payment method or season on spending.
- **Limited predictors:** Relevant influences such as income or broader economic conditions may be unavailable.
- **Cross-sectional dataset:** The `Season` column alone is insufficient to establish a reliable temporal forecasting model.
- **Generalisability:** Results from this dataset may not transfer to other populations, retailers or periods.
- **Further robustness:** Investigate alternative feature sets, repeat the analysis across multiple random splits, and examine model calibration and subgroup errors.

## How to run

1. Place `customer_purchasing_research.ipynb` and `shopping_trends.csv` in the same folder.
2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Launch the notebook:

   ```bash
   jupyter notebook customer_purchasing_research.ipynb
   ```

4. Select **Restart Kernel and Run All Cells** to reproduce the analysis and figures.

## Repository files

```text
customer-purchasing-research/
├── README.md
├── customer_purchasing_research.ipynb
├── requirements.txt
└── shopping_trends.csv   # include publicly only if licence permits
```

## Academic integrity

This project is a **revised and extended portfolio version of an earlier BSc Data Science assignment**, not an independently published academic paper. The original coursework notebooks and results are retained separately. The revised workflow improves preprocessing, model validation and baseline comparison; it does not claim novel academic findings.
