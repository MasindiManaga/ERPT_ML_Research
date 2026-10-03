# Model Specifications

## 1. Purpose

This document records the model structures, feature representations, lag-selection procedures, hyperparameter searches and final specifications used in the project.

The final forecast comparison contains:

- three econometric model families;
- three machine-learning model families;
- symmetric and asymmetric variants where applicable; and
- one persistence benchmark.

All final models are evaluated using the same locked January–December 2025 test period.

## 2. Modelling Unit

### Econometric models

ARDL, NARDL and VAR models are estimated separately for each of the 46 food subclasses.

This produces:

- 46 selected ARDL specifications;
- 46 selected NARDL specifications; and
- 46 selected VAR specifications.

### Machine-learning models

Ridge Regression, Random Forest and XGBoost are estimated as pooled models across all food subclasses.

Food-subclass identity is included as a categorical predictor and transformed through one-hot encoding.

Each machine-learning algorithm is estimated twice:

- using symmetric exchange-rate features; and
- using asymmetric exchange-rate features.

This produces six final fitted machine-learning pipelines.

## 3. Outcome Variable

The common modelling target is:

```text
Food_Inflation_Pct
```

This variable represents monthly food-subclass inflation expressed as a percentage.

The current-period target is excluded from every model feature matrix.

## 4. Symmetric ARDL Specification

The symmetric Autoregressive Distributed Lag model relates food-price dynamics to symmetric exchange-rate movements.

A general ARDL error-correction specification is:

```text
Delta_p_t =
alpha
+ phi * p_(t-1)
+ theta * e_(t-1)
+ sum(lambda_i * Delta_p_(t-i), from i = 1 to p-1)
+ sum(delta_j * Delta_e_(t-j), from j = 0 to q-1)
+ error_t
```

Where:

```text
p_t       = food-price variable
e_t       = exchange-rate variable
p         = selected food-price lag order
q         = selected exchange-rate lag order
phi       = adjustment coefficient
theta     = long-run exchange-rate level coefficient
lambda_i  = short-run food-price coefficient
delta_j   = short-run exchange-rate coefficient
error_t   = disturbance term
```

The symmetric long-run multiplier is:

```text
Symmetric_Long_Run_Multiplier = -theta / phi
```

A model is classified as a valid long-run model only when:

1. its bounds statistic supports a long-run relationship;
2. its adjustment coefficient is negative;
3. its adjustment coefficient lies between −1 and 0; and
4. its adjustment coefficient is statistically significant using HAC-adjusted inference.

The adjustment-stability condition is:

```text
-1 < phi < 0
```

## 5. Asymmetric NARDL Specification

The Nonlinear ARDL model separates exchange-rate movements into cumulative depreciation and appreciation components.

A general NARDL error-correction specification is:

```text
Delta_p_t =
alpha
+ phi * p_(t-1)
+ theta_positive * e_positive_(t-1)
+ theta_negative * e_negative_(t-1)
+ sum(lambda_i * Delta_p_(t-i), from i = 1 to p-1)
+ sum(
    delta_positive_j * Delta_e_positive_(t-j),
    from j = 0 to q-1
  )
+ sum(
    delta_negative_j * Delta_e_negative_(t-j),
    from j = 0 to q-1
  )
+ error_t
```

Where:

```text
e_positive_t       = cumulative depreciation component
e_negative_t       = cumulative signed appreciation component
theta_positive     = long-run depreciation coefficient
theta_negative     = long-run appreciation coefficient
delta_positive_j   = short-run depreciation coefficient
delta_negative_j   = short-run appreciation coefficient
```

The long-run depreciation multiplier is:

```text
Depreciation_Long_Run_Multiplier =
-theta_positive / phi
```

The long-run appreciation-component multiplier is:

```text
Appreciation_Long_Run_Multiplier =
-theta_negative / phi
```

### Long-run asymmetry hypothesis

The null hypothesis is:

```text
H0:
Depreciation_Long_Run_Multiplier
=
Appreciation_Long_Run_Multiplier
```

The alternative hypothesis is:

```text
H1:
Depreciation_Long_Run_Multiplier
!=
Appreciation_Long_Run_Multiplier
```

### Short-run asymmetry hypothesis

The null hypothesis is:

```text
H0:
sum(delta_positive_j across all included depreciation lags)
=
sum(delta_negative_j across all included appreciation lags)
```

The alternative hypothesis is:

```text
H1:
sum(delta_positive_j across all included depreciation lags)
!=
sum(delta_negative_j across all included appreciation lags)
```

## 6. ARDL and NARDL Lag Selection

For each food subclass and model family, the candidate grid contains:

- food-price lags from 1 to 6; and
- exchange-rate lags from 1 to 6.

The number of candidates per subclass and model is:

```text
Candidates_per_subclass_per_model =
6 food-price lags * 6 exchange-rate lags
= 36 candidates
```

The complete search contains:

```text
Total_candidates =
46 food subclasses
* 2 model families
* 36 candidates
= 3,312 candidate models
```

All 3,312 candidate specifications were estimated successfully.

The candidate with the lowest Bayesian Information Criterion is selected for each food subclass and model family.

### Selected ARDL lag distribution

| Price lag | Exchange lag | Food subclasses |
|---:|---:|---:|
| 1 | 1 | 33 |
| 1 | 2 | 3 |
| 2 | 1 | 3 |
| 3 | 1 | 3 |
| 3 | 2 | 2 |
| 2 | 3 | 1 |
| 5 | 1 | 1 |

### Selected NARDL lag distribution

| Price lag | Exchange lag | Food subclasses |
|---:|---:|---:|
| 1 | 1 | 34 |
| 2 | 1 | 4 |
| 3 | 1 | 4 |
| 1 | 2 | 2 |
| 3 | 2 | 1 |
| 5 | 1 | 1 |

Most selected ARDL and NARDL models therefore use one food-price lag and one exchange-rate lag.

## 7. Robust Econometric Inference

The selected ARDL and NARDL models are assessed using both conventional and heteroskedasticity-and-autocorrelation-consistent covariance estimates.

HAC estimation changes:

- standard errors;
- confidence intervals;
- test statistics; and
- p-values.

It does not change the estimated model coefficients.

The maximum difference between conventional and HAC coefficient estimates was effectively zero and reflected only floating-point precision.

## 8. Multiple-Testing Adjustments

The long-run and short-run asymmetry analyses report:

- unadjusted p-values;
- Holm-adjusted p-values; and
- Benjamini–Hochberg false-discovery-rate-adjusted p-values.

Holm-adjusted results are treated as the primary confirmatory evidence because they control the family-wise error rate.

False-discovery-rate results are retained as complementary exploratory evidence.

## 9. VAR Specification

Each bivariate Vector Autoregression contains:

```text
Y_t = [Food_Inflation_t, ExchangeRate_Change_t]
```

The general VAR specification is:

```text
Y_t =
constant
+ A_1 * Y_(t-1)
+ A_2 * Y_(t-2)
+ ...
+ A_p * Y_(t-p)
+ error_t
```

Where:

```text
Y_t     = vector containing food inflation and exchange-rate change
p       = selected VAR lag order
A_i     = coefficient matrix for lag i
error_t = innovation vector
```

### VAR lag selection

VAR lag orders from 1 to 6 are evaluated for each of the 46 food subclasses.

The total number of candidates is:

```text
Total_VAR_candidates =
46 food subclasses * 6 lag orders
= 276 candidates
```

All 276 candidate specifications were estimated successfully.

BIC selected a lag order of one for all 46 food subclasses.

Every final econometric VAR is therefore a bivariate VAR(1) model.

## 10. Machine-Learning Feature Representations

### Symmetric representation

The symmetric model input contains:

```text
SubclassDescription
Food_Inflation_Lag1_Pct
Food_Inflation_Lag3_Pct
Food_Inflation_Lag6_Pct
Month_Sin
Month_Cos
ExchangeRate_Change_Lag1_Pct
ExchangeRate_Change_Lag3_Pct
ExchangeRate_Change_Lag6_Pct
```

This representation contains:

- one categorical feature;
- eight numeric features;
- nine input columns; and
- 54 transformed features after preprocessing.

### Asymmetric representation

The asymmetric model input contains:

```text
SubclassDescription
Food_Inflation_Lag1_Pct
Food_Inflation_Lag3_Pct
Food_Inflation_Lag6_Pct
Month_Sin
Month_Cos
Depreciation_Shock_Lag1_Pct
Depreciation_Shock_Lag3_Pct
Depreciation_Shock_Lag6_Pct
Appreciation_Magnitude_Lag1_Pct
Appreciation_Magnitude_Lag3_Pct
Appreciation_Magnitude_Lag6_Pct
```

This representation contains:

- one categorical feature;
- 11 numeric features;
- 12 input columns; and
- 57 transformed features after preprocessing.

### Excluded variables

The following variables are excluded from the final feature matrices:

```text
Food_Inflation_Pct
Date
Split
ClassDescription
Subclass_Weight
```

They are used only for:

- target definition;
- chronological splitting;
- record identification;
- descriptive reporting; and
- final evaluation.

## 11. Machine-Learning Preprocessing

The machine-learning models are implemented using fitted preprocessing and estimator pipelines.

The preprocessing stage:

- one-hot encodes `SubclassDescription`;
- transforms numeric predictors as required by the estimator;
- fits transformations using development data only;
- applies the fitted transformations to validation and test records; and
- handles food-subclass categories consistently across samples.

Numeric standardisation is applied where required by Ridge Regression. Tree-based models do not depend on feature-scale standardisation.

No food subclasses in the validation or test samples were unseen during training.

## 12. Ridge Regression

### Model structure

Ridge Regression estimates coefficients by minimising:

```text
Ridge_Objective =
sum((Actual_i - Predicted_i)^2, from i = 1 to n)
+
alpha * sum((beta_j)^2, across all model coefficients)
```

The first component measures squared prediction error.

The second component is the L2 regularisation penalty.

### Hyperparameter grid

The following alpha values are evaluated:

```text
0.001
0.01
0.1
1
10
100
1000
```

Seven candidates are evaluated for each representation, producing 14 Ridge candidates.

### Selected Ridge specifications

| Representation | Alpha | Validation MAE | Validation RMSE | Validation R² | Directional accuracy |
|---|---:|---:|---:|---:|---:|
| Symmetric | 100 | 1.113336 | 1.869568 | 0.030509 | 63.768116% |
| Asymmetric | 100 | 1.118287 | 1.873179 | 0.026759 | 63.586957% |

The symmetric Ridge specification has the lower validation RMSE.

## 13. Random Forest

### Model structure

Random Forest combines predictions from multiple regression trees fitted using bootstrap samples and random feature subsets.

The ensemble prediction is:

```text
Random_Forest_Prediction =
sum(Tree_Prediction_b, from b = 1 to B) / B
```

Where:

```text
B = total number of fitted trees
```

### Hyperparameter search

The Random Forest search evaluates combinations of:

- 300 estimators;
- unrestricted, 8 or 16 maximum tree depth;
- square-root or proportional feature sampling; and
- minimum leaf sizes of 1, 3 or 6.

A total of 18 candidates are evaluated for each representation and 36 candidates overall.

### Selected Random Forest specifications

| Parameter | Symmetric | Asymmetric |
|---|---:|---:|
| Number of estimators | 300 | 300 |
| Maximum depth | None | 16 |
| Maximum features | `sqrt` | `sqrt` |
| Minimum samples per leaf | 1 | 1 |

### Validation performance

| Representation | MAE | RMSE | R² | Directional accuracy |
|---|---:|---:|---:|---:|
| Symmetric | 1.059113 | 1.759446 | 0.141356 | 64.855072% |
| Asymmetric | 1.077349 | 1.796596 | 0.104713 | 63.949275% |

The symmetric Random Forest has the lower validation RMSE.

## 14. XGBoost

### Model structure

XGBoost constructs an additive ensemble of regression trees.

The prediction for observation `i` is:

```text
XGBoost_Prediction_i =
sum(Tree_Output_k for observation i, from k = 1 to K)
```

Where:

```text
K = total number of boosting trees
```

The training objective combines prediction loss and model-complexity regularisation.

### Initial search

The initial search evaluates 48 XGBoost candidates across the two feature representations.

The search varies:

- number of estimators;
- learning rate;
- maximum tree depth;
- row subsampling;
- column subsampling;
- minimum child weight; and
- L2 regularisation.

### Refinement search

A focused refinement evaluates 12 additional configurations for each representation around the strongest initial specification.

This produces 24 refinement models.

After combining the initial and refinement searches and removing repeated configurations, the final tuning table contains 70 distinct candidates.

### Selected XGBoost specification

The same final hyperparameters are selected for both representations.

| Parameter | Selected value |
|---|---:|
| Number of estimators | 300 |
| Learning rate | 0.08 |
| Maximum depth | 4 |
| Subsample | 0.80 |
| Column subsample | 1.00 |
| Minimum child weight | 3 |
| L2 regularisation | 0.50 |

### Validation performance

| Representation | MAE | RMSE | R² | Directional accuracy |
|---|---:|---:|---:|---:|
| Symmetric | 1.046677 | 1.722070 | 0.177449 | 63.949275% |
| Asymmetric | 1.058929 | 1.728267 | 0.171518 | 63.405797% |

The symmetric XGBoost specification achieves the lowest validation RMSE among all machine-learning candidates.

## 15. Persistence Benchmark

The persistence benchmark is:

```text
Predicted_Food_Inflation_t =
Observed_Food_Inflation_(t-1)
```

It has:

- no estimated hyperparameters;
- no categorical encoding;
- no tuning stage; and
- the representation label `Lag-1 benchmark`.

Its validation performance is:

| MAE | RMSE | R² | Directional accuracy |
|---:|---:|---:|---:|
| 1.352852 | 2.340242 | −0.519088 | 56.340580% |

## 16. Machine-Learning Selection Rule

Validation RMSE is the primary machine-learning model-selection criterion.

For each algorithm:

1. candidates are fitted using the training sample;
2. predictions are generated for the 2024 validation sample;
3. validation metrics are calculated;
4. the candidate with the lowest RMSE is selected for each representation; and
5. the selected pipeline is refitted using the combined training and validation observations.

The locked 2025 test outcomes are not used for hyperparameter selection.

## 17. Development-Sample Refitting

The selected machine-learning models are refitted using:

- October 2017 to December 2024;
- 87 months;
- 46 food subclasses; and
- 4,002 observations.

The final fitted machine-learning model count is:

| Algorithm | Symmetric | Asymmetric | Total |
|---|---:|---:|---:|
| Ridge Regression | 1 | 1 | 2 |
| Random Forest | 1 | 1 | 2 |
| XGBoost | 1 | 1 | 2 |
| **Total** | **3** | **3** | **6** |

## 18. Saved Machine-Learning Pipelines

The final fitted pipelines are saved locally as:

```text
models/machine_learning/ridge_symmetric_pipeline.joblib
models/machine_learning/ridge_asymmetric_pipeline.joblib
models/machine_learning/random_forest_symmetric_pipeline.joblib
models/machine_learning/random_forest_asymmetric_pipeline.joblib
models/machine_learning/xgboost_symmetric_pipeline.joblib
models/machine_learning/xgboost_asymmetric_pipeline.joblib
```

These binary files are excluded from Git because trained model files can be large.

The corresponding model metadata are stored in:

```text
reports/tables/machine_learning/model_manifest.csv
```

Each saved pipeline was reloaded and reproduced all 552 expected predictions with a maximum prediction difference of zero.

## 19. Final Forecast Variants

The complete locked-test comparison contains ten variants.

| Approach | Model | Representation | Predictions |
|---|---|---|---:|
| Econometric | ARDL | Symmetric | 552 |
| Econometric | NARDL | Asymmetric | 552 |
| Econometric | VAR | Bivariate system | 552 |
| Machine learning | Ridge | Symmetric | 552 |
| Machine learning | Ridge | Asymmetric | 552 |
| Machine learning | Random Forest | Symmetric | 552 |
| Machine learning | Random Forest | Asymmetric | 552 |
| Machine learning | XGBoost | Symmetric | 552 |
| Machine learning | XGBoost | Asymmetric | 552 |
| Benchmark | Persistence | Lag-1 benchmark | 552 |

The total number of predictions is:

```text
Total_Predictions =
10 forecast variants
* 46 food subclasses
* 12 test months
= 5,520 predictions
```

## 20. Forecast-Performance Formulas

### Forecast error

```text
Error_i = Prediction_i - Actual_i
```

### Mean Absolute Error

```text
MAE =
sum(
    absolute_value(Actual_i - Prediction_i),
    from i = 1 to n
  )
/ n
```

### Root Mean Squared Error

```text
RMSE =
square_root[
    sum(
        (Actual_i - Prediction_i)^2,
        from i = 1 to n
      )
    / n
]
```

### Coefficient of determination

```text
R2 =
1
-
[
    sum((Actual_i - Prediction_i)^2)
    /
    sum((Actual_i - Mean_Actual)^2)
]
```

### Directional accuracy

```text
Directional_Accuracy_Pct =
100
* Number_of_predictions_with_correct_direction
/ Total_number_of_predictions
```

### Mean bias

```text
Mean_Bias =
sum(Prediction_i - Actual_i, from i = 1 to n)
/ n
```

Interpretation:

```text
Mean_Bias > 0  means average overprediction.
Mean_Bias < 0  means average underprediction.
Mean_Bias = 0  means no average forecast bias.
```

### RMSE improvement relative to persistence

```text
RMSE_Improvement_vs_Persistence_Pct =
100
* (Persistence_RMSE - Model_RMSE)
/ Persistence_RMSE
```

### MAE improvement relative to persistence

```text
MAE_Improvement_vs_Persistence_Pct =
100
* (Persistence_MAE - Model_MAE)
/ Persistence_MAE
```

## 21. Specification Files

| Content | File |
|---|---|
| ARDL lag selections | `reports/tables/econometrics/symmetric_lag_selection.csv` |
| NARDL lag selections | `reports/tables/econometrics/asymmetric_lag_selection.csv` |
| Machine-learning model manifest | `reports/tables/machine_learning/model_manifest.csv` |
| Ridge tuning results | `reports/tables/machine_learning/ridge_tuning_results.csv` |
| Selected Ridge models | `reports/tables/machine_learning/selected_ridge_models.csv` |
| Random Forest tuning results | `reports/tables/machine_learning/random_forest_tuning_results.csv` |
| Selected Random Forest models | `reports/tables/machine_learning/selected_random_forest_models.csv` |
| XGBoost tuning results | `reports/tables/machine_learning/xgboost_tuning_results.csv` |
| Selected XGBoost models | `reports/tables/machine_learning/selected_xgboost_models.csv` |
| Final forecast comparison | `reports/tables/model_evaluation/overall_model_performance.csv` |
