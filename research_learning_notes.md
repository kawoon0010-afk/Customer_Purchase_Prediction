# Research Learning Notes — Customer Purchasing Behaviour

This project gives me practical experience with **data auditing, exploratory analysis, feature preprocessing, regression, out-of-sample evaluation, cross-validation, baseline comparisons, and transparent reporting of negative results.

## 1. Train/test split and data leakage

The notebook uses an **80% training / 20% test split**, with `random_state=42`. The test set is held out while the models are trained. Five-fold cross-validation is conducted **within the training set only**.

**Data leakage** occurs when information from the test set or otherwise unavailable information influences model training. For example, fitting an encoder or scaler on all observations before splitting can leak information about the test distribution. A `Pipeline` helps fit preprocessing steps only on training data.

**What I learned:** A model must be assessed on data it did not train on. In this project, `Customer ID` is excluded because it is an identifier, and `Review Rating` is excluded because it may be known only after a purchase. Some other predictors may also be available only at checkout; that is a limitation of the prediction setting.


## 2. Why compare three models?

| Model | Purpose | Main consideration |
|---|---|---|
| **DummyRegressor (mean baseline)** | Predicts the training-set average for every observation | Sets a minimum benchmark |
| **Linear Regression** | Learns an additive linear relationship between features and purchase amount | Interpretable benchmark; depends on modelling assumptions |
| **Random Forest Regressor** | Combines decision trees to learn potentially nonlinear patterns | Flexible, but complexity does not guarantee better test results |

**What I learned:** A more complex model is only useful for prediction if it improves performance on unseen data relative to a sensible baseline.

## 3. MAE, RMSE, and R²

Measures the average *absolute* prediction error. Its unit is **USD**, making it easy to explain. **Lower is better.**

**Example:** An MAE of $20.756 means the average absolute prediction error is approximately **$20.76**, not that every prediction misses by exactly that amount.

### RMSE — Root Mean Squared Error

Also measured in **USD**, but penalises large errors more strongly because errors are squared before averaging. **Lower is better.**

### R² — Coefficient of Determination


- `R² = 1`: perfect predictions.
- `R² = 0`: the same sum of squared errors as predicting the test-set mean.
- `R² < 0`: worse squared error than predicting the test-set mean.

**Why did Random Forest model produce a negative R²?**  
The negative test R² indicates that Random Forest produced a larger sum of squared prediction errors than the test-set mean benchmark. It also failed to outperform the training-mean baseline. This suggests that the selected features and model did not provide useful additional predictive information on the held-out data.

Negative R² is not a software error by itself, nor a reason to hide results. It is a prompt to examine feature usefulness, model assumptions, data quality, and the research question. The result does **not** prove that consumer characteristics never matter.


## 4. Diagnostic plots and residuals

A **residual** is `actual − predicted`. The notebook plots observed versus predicted purchase amounts and a residual histogram for the best-MAE model, which is the mean baseline.

**What I learned:** A model can produce outputs without producing useful predictions. A near-horizontal predicted-value pattern is expected for a mean baseline. Residual plots help diagnose systematic errors, but they do not by themselves prove a model is reliable.


**Academic integrity note:** These notes document learning from an improved version of my earlier BSc coursework. 