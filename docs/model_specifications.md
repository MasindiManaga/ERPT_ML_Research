
# Model Specifications

## 1. Purpose

This document records the model structures, feature representations, lag-selection procedures, hyperparameter searches and final specifications used in the project.

The final forecast comparison contains:

- three econometric model families;
- three machine-learning model families;
- symmetric and asymmetric variants where applicable; and
- one persistence benchmark.

All final models are evaluated on the same locked January–December 2025 test period.

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

Each algorithm is estimated twice:

- using symmetric exchange-rate features; and
- using asymmetric exchange-rate features.

This produces six final fitted machine-learning pipelines.

## 3. Outcome Variable

The common modelling target is:

```text
Food_Inflation_Pct
```

This represents monthly food-subclass inflation expressed as a percentage.

The current-period target is never included in the model feature matrix.

## 4. Econometric Specifications

## 4.1 Symmetric ARDL

The symmetric Autoregressive Distributed Lag model relates food-price inflation or its error-correction representation to lagged food-price dynamics and symmetric exchange-rate movements.

A general ARDL error-correction representation is:

\[
\Delta p_t =
\alpha +
\phi p_{t-1} +
\theta e_{t-1} +
\sum_{i=1}^{p-1}\lambda_i\Delta p_{t-i} +
\sum_{j=0}^{q-1}\delta_j\Delta e_{t-j} +
\varepsilon_t
\]

where:

- \(p_t\) represents the food-price variable;
- \(e_t\) represents the exchange-rate variable;
- \(p\) is the selected price-lag order;
- \(q\) is the selected exchange-rate-lag order;
- \(\phi\) is the adjustment coefficient;
- \(\theta\) is the exchange-rate level coefficient; and
- \(\varepsilon_t\) is the disturbance term.

The symmetric long-run multiplier is calculated from the level coefficients as:

\[
LRM = -\frac{\theta}{\phi}
\]

A valid long-run model requires:

1. bounds-test support for a long-run relationship;
2. a negative adjustment coefficient; and
3. a statistically significant adjustment coefficient.

## 4.2 Asymmetric NARDL

The NARDL model replaces the single exchange-rate component with separate cumulative positive and negative exchange-rate components.

A general specification is:

\[
\Delta p_t =
\alpha +
\phi p_{t-1} +
\theta^{+}e^{+}_{t-1} +
\theta^{-}e^{-}_{t-1} +
\sum_{i=1}^{p-1}\lambda_i\Delta p_{t-i}
+
\sum_{j=0}^{q-1}\delta^{+}_j\Delta e^{+}_{t-j}
+
\sum_{j=0}^{q-1}\delta^{-}_j\Delta e^{-}_{t-j}
+
\varepsilon_t
\]

where:

- \(e^{+}\) is the cumulative depreciation component;
- \(e^{-}\) is the cumulative signed appreciation component;
- \(\theta^{+}\) and \(\theta^{-}\) are the long-run level coefficients; and
- \(\delta^{+}\) and \(\delta^{-}\) capture short-run responses.

The long-run depreciation multiplier is:

\[
LRM^{+} = -\frac{\theta^{+}}{\phi}
\]

The long-run appreciation-component multiplier is:

\[
LRM^{-} = -\frac{\theta^{-}}{\phi}
\]

Long-run asymmetry is tested using:

\[
H_0: LRM^{+}=LRM^{-}
\]

Short-run asymmetry is tested using equality between the cumulative short-run depreciation and appreciation effects:

\[
H_0:
\sum_j\delta^{+}_j =
\sum_j\delta^{-}_j
\]

## 4.3 ARDL and NARDL lag selection

For each food subclass and model family, the candidate grid contains:

- price lags from 1 to 6; and
- exchange-rate lags from 1 to 6.

This gives:

\[
6 \times 6 = 36
\]

candidate specifications for each model-subclass combination.

The full search therefore contains:

\[
46 \times 2 \times 36 = 3,312
\]

candidate models.

All 3,312 candidate specifications were estimated successfully.

The candidate with the lowest Bayesian Information Criterion is selected for each food subclass and model family.

### Selected-lag distribution

Most selected ARDL and NARDL models use one price lag and one exchange-rate lag.

#### ARDL selections

| Price lag | Exchange lag | Food subclasses |
|---:|---:|---:|
| 1 | 1 | 33 |
| 1 | 2 | 3 |
| 2 | 1 | 3 |
| 3 | 1 | 3 |
| 3 | 2 | 2 |
| 2 | 3 | 1 |
| 5 | 1 | 1 |

#### NARDL selections

| Price lag | Exchange lag | Food subclasses |
|---:|---:|---:|
| 1 | 1 | 34 |
| 2 | 1 | 4 |
| 3 | 1 | 4 |
| 1 | 2 | 2 |
| 3 | 2 | 1 |
| 5 | 1 | 1 |

## 4.4 Robust econometric inference

The selected ARDL and NARDL models are evaluated using conventional and heteroskedasticity-and-autocorrelation-consistent covariance estimates.

HAC estimation changes:

- standard errors;
- confidence intervals;
- test statistics; and
- p-values.

It does not change the fitted model coefficients.

The maximum observed difference between conventional and HAC coefficient estimates was effectively zero and reflected only floating-point precision.

## 4.5 Adjustment-coefficient specification

The adjustment coefficient represents the speed at which deviations from the estimated long-run equilibrium are corrected.

The required stability condition is:

\[
-1 < \phi < 0
\]

The primary long-run validity rule also requires the HAC-adjusted p-value of the adjustment coefficient to be below 0.05.

The long-run evidence assessment identified:

- nine bounds-supported models;
- eight valid long-run models; and
- one bounds-supported model without a statistically stable adjustment process.

## 4.6 Multiple-testing adjustments

The asymmetry analyses report:

- unadjusted p-values;
- Holm-adjusted p-values; and
- Benjamini–Hochberg false-discovery-rate-adjusted p-values.

Holm-adjusted results are treated as the primary confirmatory result because they control the family-wise error rate.

False-discovery-rate results are retained as complementary exploratory evidence.

## 5. VAR Specification

## 5.1 System variables

Each bivariate VAR contains:

\[
Y_t =
\begin{bmatrix}
FoodInflation_t \\
ExchangeRateChange_t
\end{bmatrix}
\]

The general VAR specification is:

\[
Y_t =
c +
A_1Y_{t-1} +
A_2Y_{t-2} +
\cdots +
A_pY_{t-p} +
u_t
\]

where:

- \(c\) is the intercept vector;
- \(A_i\) contains lagged system coefficients;
- \(p\) is the VAR lag order; and
- \(u_t\) is the innovation vector.

## 5.2 VAR lag search

VAR lag orders from 1 to 6 are evaluated for each of the 46 food subclasses.

The full search contains:

\[
46 \times 6 = 276
\]

candidate VAR specifications.

All 276 models were estimated successfully.

BIC selected a lag order of one for all 46 food subclasses.

## 5.3 Final VAR specification

Every final VAR is therefore a bivariate VAR(1) system estimated using the development sample.

The system contains six estimated parameters under the implementation used in the project.

## 6. Machine-Learning Feature Representations

## 6.1 Symmetric representation

The symmetric input specification uses:

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

This gives:

- one categorical feature;
- eight numeric features;
- nine input columns; and
- 54 transformed features after preprocessing.

## 6.2 Asymmetric representation

The asymmetric input specification uses:

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

This gives:

- one categorical feature;
- 11 numeric features;
- 12 input columns; and
- 57 transformed features after preprocessing.

## 6.3 Excluded variables

The following variables are not used as current-period model predictors:

```text
Food_Inflation_Pct
Date
Split
ClassDescription
Subclass_Weight
```

Their roles are limited to:

- target definition;
- chronological splitting;
- identification;
- descriptive reporting; and
- evaluation.

## 7. Ridge Regression

## 7.1 Model structure

Ridge Regression estimates:

\[
\hat{\beta}
=
\arg\min_{\beta}
\left[
\sum_{i=1}^{n}
(y_i-X_i\beta)^2
+
\alpha\sum_{j=1}^{k}\beta_j^2
\right]
\]

The L2 penalty reduces coefficient magnitude and helps control multicollinearity among lagged predictors.

## 7.2 Hyperparameter grid

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

Seven candidates are fitted for each representation, producing 14 Ridge candidates.

## 7.3 Selected Ridge specifications

| Representation | Alpha | Validation MAE | Validation RMSE | Validation R² | Directional accuracy |
|---|---:|---:|---:|---:|---:|
| Symmetric | 100 | 1.113336 | 1.869568 | 0.030509 | 63.768116% |
| Asymmetric | 100 | 1.118287 | 1.873179 | 0.026759 | 63.586957% |

The symmetric Ridge specification has the lower validation RMSE.

## 8. Random Forest

## 8.1 Model structure

Random Forest combines predictions from multiple regression trees fitted to bootstrap samples and random feature subsets.

The ensemble prediction is:

\[
\hat{y} =
\frac{1}{B}
\sum_{b=1}^{B}\hat{y}_b
\]

where \(B\) is the number of trees.

## 8.2 Search design

The Random Forest search evaluates combinations of:

- 300 estimators;
- unrestricted, 8 or 16 maximum depth;
- square-root or proportional feature sampling; and
- minimum leaf sizes of 1, 3 or 6.

A total of 18 candidates are evaluated for each representation and 36 candidates overall.

## 8.3 Selected Random Forest specifications

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

## 9. XGBoost

## 9.1 Model structure

XGBoost builds an additive ensemble of regression trees:

\[
\hat{y}_i^{(K)}
=
\sum_{k=1}^{K}f_k(X_i)
\]

where each \(f_k\) is a fitted decision tree.

The objective combines prediction loss and model-complexity regularisation.

## 9.2 Initial search

The initial XGBoost search evaluates 48 model candidates across the two feature representations.

The search varies:

- learning rate;
- tree depth;
- row subsampling;
- column subsampling;
- minimum child weight;
- number of estimators; and
- L2 regularisation.

## 9.3 Refinement search

A focused refinement evaluates 12 additional configurations per representation around the strongest initial candidate.

This produces 24 refinement models.

After combining the initial and refinement searches and removing duplicated configurations, the final tuning table contains 70 distinct candidates.

## 9.4 Selected XGBoost specifications

The same final hyperparameters are selected for the symmetric and asymmetric representations.

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

## 10. Persistence Benchmark

The persistence forecast is:

\[
\hat{y}_t = y_{t-1}
\]

The model predicts current food-price inflation using the previous month’s observed inflation value.

It has:

- no estimated hyperparameters;
- no categorical encoding;
- no tuning stage; and
- the representation label `Lag-1 benchmark`.

The validation performance is:

| MAE | RMSE | R² | Directional accuracy |
|---:|---:|---:|---:|
| 1.352852 | 2.340242 | −0.519088 | 56.340580% |

## 11. Machine-Learning Selection Rule

The primary model-selection criterion is validation RMSE.

For each machine-learning algorithm:

1. candidates are fitted using the training sample;
2. predictions are generated for the 2024 validation sample;
3. validation metrics are calculated;
4. the candidate with the lowest RMSE is selected for each representation; and
5. the selected pipeline is refitted using the combined training and validation sample.

The locked 2025 test data are not used for hyperparameter selection.

## 12. Final Development-Sample Refitting

The selected machine-learning models are refitted using:

- October 2017 to December 2024;
- 87 months;
- 46 food subclasses; and
- 4,002 observations.

The final fitted model count is:

| Algorithm | Symmetric | Asymmetric | Total |
|---|---:|---:|---:|
| Ridge Regression | 1 | 1 | 2 |
| Random Forest | 1 | 1 | 2 |
| XGBoost | 1 | 1 | 2 |
| **Total** | **3** | **3** | **6** |

## 13. Saved Machine-Learning Pipelines

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

The corresponding metadata are stored in:

```text
reports/tables/machine_learning/model_manifest.csv
```

Each saved pipeline was reloaded and reproduced all 552 expected predictions with a maximum prediction difference of zero.

## 14. Final Forecast Variants

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

The combined prediction table therefore contains:

\[
10 \times 552 = 5,520
\]

forecast records.

## 15. Specification Files

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
