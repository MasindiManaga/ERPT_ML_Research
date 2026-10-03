# Methodology

## 1. Research Design

This study follows a quantitative, comparative modelling design to investigate exchange rate pass-through (ERPT) across disaggregated South African food-price categories.

The analysis combines:

- econometric inference;
- time-series forecasting;
- machine-learning prediction;
- symmetric and asymmetric exchange-rate representations; and
- category-level and month-level forecast evaluation.

All models are assessed using a common locked test period covering January to December 2025. The test outcomes are excluded from model selection and hyperparameter tuning.

## 2. Analytical Workflow

The research pipeline consists of nine sequential stages:

1. Data acquisition and understanding
2. Data preparation
3. Feature engineering
4. Exploratory data analysis
5. Modelling-data preparation
6. Econometric modelling
7. Econometric diagnostics and robust inference
8. Machine-learning modelling
9. Forecast evaluation and comparison

Each stage is implemented in a numbered Jupyter notebook under the `notebooks/` directory.

## 3. Data Sources

The project combines monthly food-price and exchange-rate information from the following sources:

- Statistics South Africa Consumer Price Index data;
- detailed food-category and food-subclass price information;
- historical USD/ZAR exchange-rate data; and
- project-generated transformations and lagged variables.

The source data are stored locally under `data/raw/` and are excluded from Git version control.

## 4. Data Preparation

### 4.1 Food-price data

Food-price records were cleaned and standardised before modelling. The preparation process included:

- standardising column names;
- converting dates into a monthly date format;
- validating food hierarchy descriptions;
- checking food-subclass identifiers;
- converting CPI and weight variables to numeric formats;
- identifying and resolving missing or duplicated records; and
- retaining food subclasses with sufficiently complete monthly coverage.

The final analysis contains 46 food subclasses.

### 4.2 Exchange-rate data

Historical USD/ZAR exchange-rate observations were:

- converted to numeric values;
- assigned valid dates;
- ordered chronologically;
- aligned to a monthly frequency; and
- merged with the food-price data using the corresponding month.

An increase in the exchange-rate series represents a depreciation of the South African rand, while a decrease represents an appreciation.

### 4.3 Dataset alignment

The food-price and exchange-rate series were aligned by month. Validation checks confirmed:

- no missing values in the final modelling datasets;
- no duplicate subclass-month observations;
- consistent food-subclass coverage; and
- exact chronological ordering.

The econometric dataset contains 4,830 observations across 46 food subclasses and 105 months from April 2017 to December 2025.

After constructing the required lags, the common machine-learning and forecast-evaluation dataset contains 4,554 observations across 99 months from October 2017 to December 2025.

## 5. Variable Construction

### 5.1 Food-price outcome

The main outcome is monthly food-price inflation, expressed as a percentage change in the food-subclass CPI.

The CPI and its logarithmic transformation are retained in the econometric data to support level-based and error-correction representations.

### 5.2 Symmetric exchange-rate representation

The symmetric representation uses the percentage change in the exchange rate without separating positive and negative movements.

Lagged exchange-rate changes are included to capture delayed pass-through effects.

The machine-learning symmetric representation contains:

- one categorical food-subclass feature;
- three lagged food-inflation variables;
- two seasonal variables; and
- three lagged exchange-rate-change variables.

This produces nine input columns before categorical encoding.

### 5.3 Asymmetric exchange-rate representation

Exchange-rate changes are decomposed into depreciation and appreciation components.

The decomposition satisfies:

\[
\Delta ER_t = DEP_t + APP_t
\]

where:

- \(DEP_t = \max(\Delta ER_t, 0)\); and
- \(APP_t = \min(\Delta ER_t, 0)\).

Depreciation shocks are therefore non-negative, while appreciation shocks are non-positive.

For machine-learning models, appreciation movements are represented by their positive magnitudes to provide a consistent feature scale. Depreciation and appreciation features are lagged separately.

The asymmetric machine-learning representation contains:

- one categorical food-subclass feature;
- three lagged food-inflation variables;
- two seasonal variables;
- three lagged depreciation-shock variables; and
- three lagged appreciation-magnitude variables.

This produces 12 input columns before categorical encoding.

### 5.4 Long-run asymmetric components

For NARDL estimation, cumulative positive and negative exchange-rate components are constructed from the decomposed exchange-rate changes. These partial-sum processes allow depreciation and appreciation movements to have different long-run coefficients.

### 5.5 Lagged food-price features

Food-price inflation lags are constructed to represent inflation persistence and delayed price adjustment.

The machine-learning models use:

- one-month inflation lag;
- three-month inflation lag; and
- six-month inflation lag.

Only lagged inflation values are used as predictors. The current-period target is excluded from the feature matrix.

### 5.6 Seasonal features

Month-of-year seasonality is represented using cyclical sine and cosine transformations:

\[
MonthSin_t = \sin\left(\frac{2\pi Month_t}{12}\right)
\]

\[
MonthCos_t = \cos\left(\frac{2\pi Month_t}{12}\right)
\]

These transformations preserve the cyclical relationship between December and January.

## 6. Sample Design

### 6.1 Development and test periods

The common forecast-comparison sample is divided chronologically:

| Sample | Period | Months | Observations |
|---|---|---:|---:|
| Development | October 2017–December 2024 | 87 | 4,002 |
| Locked test | January 2025–December 2025 | 12 | 552 |

Each month contains observations for all 46 food subclasses.

### 6.2 Machine-learning validation period

For machine-learning model selection, the development process uses:

| Sample | Period | Months | Observations |
|---|---|---:|---:|
| Training | October 2017–December 2023 | 75 | 3,450 |
| Validation | January 2024–December 2024 | 12 | 552 |
| Test | January 2025–December 2025 | 12 | 552 |

Hyperparameters and feature representations are selected using validation RMSE. After selection, each final machine-learning pipeline is refitted using the combined training and validation data ending in December 2024.

The locked 2025 test outcomes are used only for final evaluation.

## 7. Econometric Modelling

## 7.1 ARDL

Autoregressive Distributed Lag models are estimated separately for each food subclass.

The ARDL specification represents exchange-rate effects symmetrically and includes:

- lagged food-price dynamics;
- current or lagged exchange-rate movements, depending on the selected specification; and
- an error-correction representation for evaluating long-run adjustment.

Candidate price and exchange-rate lag combinations are estimated for each subclass. The specification with the lowest Bayesian Information Criterion (BIC) is selected.

A total of 36 lag candidates are evaluated for each food subclass.

### 7.2 NARDL

Nonlinear ARDL models extend the symmetric ARDL framework by replacing the single exchange-rate component with separate cumulative depreciation and appreciation components.

NARDL models are estimated separately for all 46 food subclasses. As with ARDL, 36 lag candidates are evaluated for each subclass and the specification with the lowest BIC is selected.

The NARDL framework supports:

- separate short-run depreciation and appreciation effects;
- separate long-run depreciation and appreciation multipliers;
- short-run asymmetry tests; and
- long-run asymmetry tests.

### 7.3 Bounds testing and long-run validity

Bounds-test statistics are used to identify evidence of a long-run relationship.

A bounds-supported model is classified as a valid long-run model only when it also has a negative and statistically significant adjustment coefficient. This requirement helps confirm convergence towards the estimated long-run equilibrium.

### 7.4 Long-run multipliers

For a valid ARDL model, the long-run exchange-rate multiplier is calculated from the estimated level coefficients.

For NARDL models, separate long-run multipliers are calculated for:

- depreciation; and
- the appreciation component.

The uncertainty surrounding each multiplier is calculated using the estimated covariance matrix and the delta method.

### 7.5 Asymmetry testing

Long-run asymmetry is evaluated by testing whether the depreciation and appreciation multipliers are equal.

Short-run asymmetry is evaluated by testing equality between the distributed-lag sums of depreciation and appreciation effects.

Because the study conducts tests across multiple food subclasses, raw p-values are supplemented with:

- Holm-adjusted p-values; and
- Benjamini–Hochberg false-discovery-rate-adjusted p-values.

Holm-adjusted results are treated as the primary confirmatory evidence. False-discovery-rate results are reported as complementary exploratory evidence.

### 7.6 Diagnostics and robust inference

Econometric models are evaluated using residual and parameter diagnostics covering:

- serial correlation;
- ARCH effects;
- residual normality; and
- parameter stability.

Heteroskedasticity-and-autocorrelation-consistent covariance estimates are used to obtain robust standard errors, p-values and confidence intervals.

The HAC adjustment changes inference while leaving the estimated coefficients unchanged.

### 7.7 VAR

A bivariate Vector Autoregression is estimated separately for every food subclass using:

- food-price inflation; and
- exchange-rate change.

VAR lag orders from one to six are evaluated using BIC. The selected model is then refitted using the full development sample and used to produce predictions for the locked test period.

## 8. Machine-Learning Modelling

## 8.1 Preprocessing pipeline

Machine-learning models are implemented using reproducible pipelines.

The preprocessing stage:

- one-hot encodes the food-subclass category;
- standardises numeric predictors where required by the estimator;
- preserves chronological sample separation; and
- applies identical fitted transformations to validation and test data.

The current target, date, split label and descriptive metadata are excluded from model features.

The fitted preprocessing stage produces:

- 54 transformed features for the symmetric representation; and
- 57 transformed features for the asymmetric representation.

### 8.2 Ridge Regression

Ridge Regression provides a regularised linear benchmark.

Seven alpha values are evaluated for each feature representation:

\[
0.001,\ 0.01,\ 0.1,\ 1,\ 10,\ 100,\ 1000
\]

The final symmetric and asymmetric Ridge specifications both use an alpha of 100, selected using validation RMSE.

### 8.3 Random Forest

Random Forest models are evaluated across combinations of:

- tree depth;
- number of candidate features;
- minimum leaf size; and
- number of trees.

A total of 36 Random Forest candidates are evaluated across the two feature representations.

The selected models use 300 trees and square-root feature sampling. The symmetric model uses unrestricted tree depth, while the asymmetric model uses a maximum depth of 16. Both selected models use a minimum leaf size of one.

### 8.4 XGBoost

XGBoost candidates are evaluated using combinations of:

- number of boosting estimators;
- learning rate;
- maximum tree depth;
- row subsampling;
- column subsampling;
- minimum child weight; and
- L2 regularisation.

The initial search is followed by a focused refinement around the strongest validation specification.

The final symmetric and asymmetric XGBoost models use:

- 300 estimators;
- learning rate of 0.08;
- maximum depth of 4;
- subsample of 0.80;
- column subsample of 1.00;
- minimum child weight of 3; and
- L2 regularisation parameter of 0.50.

### 8.5 Persistence benchmark

The persistence benchmark predicts the current food-price inflation rate using the previous month’s observed inflation rate.

This benchmark provides a simple reference for determining whether the fitted models add predictive value beyond recent inflation persistence.

## 9. Forecast Generation

Final selected models are refitted using observations available up to December 2024.

The prediction outputs contain:

- date;
- food class;
- food subclass;
- model;
- representation; and
- predicted food-price inflation.

The final combined forecast table contains:

- ten forecast variants;
- 552 predictions per variant; and
- 5,520 prediction records in total.

All prediction records are checked for:

- missing values;
- non-finite values;
- duplicate model-subclass-month keys; and
- complete food-subclass and monthly coverage.

## 10. Forecast Evaluation

### 10.1 Performance measures

Forecast accuracy is assessed using the following measures.

#### Mean Absolute Error

\[
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i-\hat{y}_i|
\]

#### Root Mean Squared Error

\[
RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
\]

#### Coefficient of determination

\[
R^2 = 1-\frac{\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
{\sum_{i=1}^{n}(y_i-\bar{y})^2}
\]

#### Directional accuracy

Directional accuracy measures the percentage of observations for which the predicted and observed inflation rates have the same sign.

#### Mean bias

\[
Bias = \frac{1}{n}\sum_{i=1}^{n}(\hat{y}_i-y_i)
\]

Positive bias indicates average overprediction, while negative bias indicates average underprediction.

### 10.2 Evaluation levels

Performance is calculated at three levels:

1. overall performance across all 552 test observations;
2. subclass performance using 12 observations per subclass; and
3. monthly performance using 46 observations per month.

The best-performing model at the subclass and monthly levels is selected using the lowest RMSE.

## 11. Statistical Forecast Comparisons

### 11.1 Monthly loss-differential tests

Squared-error loss is aggregated by test month. The average loss differential between a reference model and each alternative is tested using HAC-adjusted inference.

There are 12 monthly loss observations in each comparison.

Holm-adjusted p-values are calculated across the family of model comparisons.

### 11.2 Subclass-level comparisons

Model loss is also aggregated across the 46 food subclasses.

Paired Wilcoxon signed-rank tests evaluate whether the distribution of subclass-level losses differs between competing models.

Holm adjustments are applied to account for multiple comparisons.

### 11.3 Symmetric-versus-asymmetric comparisons

Symmetric and asymmetric variants are compared within:

- Ridge Regression;
- Random Forest;
- XGBoost; and
- ARDL versus NARDL.

The analysis distinguishes between:

- overall differences in pooled RMSE;
- differences across the 12 test months; and
- differences across the 46 food subclasses.

This prevents an overall average from concealing category-specific predictive differences.

## 12. Leakage Prevention and Validation

The implementation includes explicit leakage and output checks confirming that:

- the current target is excluded from model features;
- date and split labels are excluded from model features;
- descriptive metadata are excluded from the fitted feature matrices;
- exchange-rate predictors are lagged;
- the development sample ends before the test period;
- test outcomes are absent from the prediction files;
- all required model variants are present;
- every model produces the expected number of predictions;
- all predictions are finite; and
- prediction records are unique.

Saved machine-learning pipelines are reloaded and their predictions are reproduced exactly before final validation.

## 13. Software Environment

The analysis is implemented in Python using:

- pandas;
- NumPy;
- SciPy;
- statsmodels;
- scikit-learn;
- XGBoost;
- Matplotlib;
- Seaborn;
- Joblib; and
- JupyterLab.

Exact package versions are listed in the root `requirements.txt` file.

## 14. Output Validation

Generated tables and figures are programmatically checked after export.

Table validation compares:

- expected row counts;
- saved row counts;
- expected column counts; and
- saved column counts.

Figure validation confirms that each expected figure:

- exists;
- is non-empty; and
- has a valid file size.

The validated outputs are stored under:

- `reports/tables/econometrics/`;
- `reports/tables/machine_learning/`;
- `reports/tables/model_evaluation/`;
- `reports/figures/econometrics/`; and
- `reports/figures/model_evaluation/`.
