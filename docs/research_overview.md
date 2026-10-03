# Research Overview

## Project Title

**Asymmetric Exchange Rate Pass-Through Across Food Price Categories in South Africa: A Comparison of Econometric and Machine Learning Models**

## Author

**Masindi Managa**  
BSc Honours in Information Technology (Data Science)  
Eduvos, South Africa

## Background

Exchange rate pass-through (ERPT) describes the extent to which changes in the exchange rate are transmitted to domestic prices. In South Africa, movements in the rand can affect imported inputs, production costs, transportation expenses and ultimately consumer food prices.

The degree of pass-through may differ across food categories because products have different import dependencies, supply chains, storage requirements, pricing structures and competitive conditions. Exchange rate depreciations and appreciations may also have unequal effects. Producers and retailers may pass cost increases to consumers more quickly than they reverse prices following an appreciation.

Traditional ERPT studies commonly use econometric models that estimate average relationships between exchange rates and prices. These models are valuable for inference and interpretation but may not fully capture nonlinear relationships, interactions and category-specific predictive patterns. Machine-learning models can represent more complex relationships, although they do not automatically provide better forecasts or stronger evidence of economic asymmetry.

This project therefore evaluates econometric and machine-learning approaches within a common, reproducible forecasting framework.

## Research Problem

Aggregate food-price analysis can conceal substantial differences between individual food categories. Similarly, modelling exchange rate movements as symmetric may overlook cases in which depreciations and appreciations have different effects.

However, adding asymmetric predictors also increases model complexity and does not necessarily improve forecasting performance. A systematic comparison is required to determine:

- whether long-run and short-run ERPT asymmetry exists across food subclasses;
- whether asymmetric exchange rate representations improve forecasts;
- whether econometric or machine-learning models perform better;
- and whether model performance differs across food categories and test months.

## Research Aim

The study aims to investigate asymmetric exchange rate pass-through across disaggregated South African food-price categories and to compare the explanatory and forecasting performance of econometric and machine-learning models.

## Research Objectives

The project has the following objectives:

1. Prepare and align monthly South African food-price and USD/ZAR exchange-rate data.
2. Construct symmetric and asymmetric exchange rate representations.
3. Estimate ARDL, NARDL and VAR models for individual food subclasses.
4. evaluate long-run relationships, adjustment dynamics and short-run and long-run asymmetry.
5. Develop Ridge Regression, Random Forest and XGBoost forecasting models.
6. Compare symmetric and asymmetric feature representations within each applicable model family.
7. Evaluate all forecasts on the same locked January–December 2025 test period.
8. Compare forecast accuracy using MAE, RMSE, R², directional accuracy and mean bias.
9. Conduct formal forecast-accuracy and representation-comparison tests.
10. Identify the best-performing model for each food subclass and test month.

## Analytical Questions

The implemented analysis addresses the following questions:

1. Which food subclasses provide valid evidence of a long-run relationship between exchange rate movements and food-price inflation?
2. Is long-run or short-run ERPT asymmetry present after correcting for multiple hypothesis testing?
3. Do asymmetric exchange rate predictors improve machine-learning forecast accuracy?
4. Which model produces the lowest overall forecast errors?
5. Does one modelling approach dominate across all evaluation criteria?
6. How much does model performance vary across food subclasses and months?
7. Are econometric and machine-learning approaches substitutes, or do they provide complementary evidence?

## Data Scope

The analysis uses monthly South African food-price and exchange-rate data ending in December 2025.

The econometric dataset contains:

- 4,830 observations;
- 46 food subclasses;
- 105 monthly periods from April 2017 to December 2025; and
- no missing values in the final aligned dataset.

Following feature engineering and lag alignment, the common machine-learning and forecast-comparison dataset contains:

- 4,554 observations;
- 46 food subclasses;
- 99 monthly periods from October 2017 to December 2025; and
- no duplicate subclass-month records.

The common forecasting design uses:

- a development period from October 2017 to December 2024;
- 4,002 development observations across 87 months; and
- a locked test period from January to December 2025 containing 552 observations.

The locked test outcomes were not used for model selection or hyperparameter tuning.

## Data Sources

The project uses:

- South African food Consumer Price Index data from Statistics South Africa;
- USD/ZAR exchange-rate data from the project’s historical exchange-rate source files; and
- project-generated features, lags, shock decompositions and cumulative exchange-rate components.

Raw and processed datasets are excluded from version control because of file size, licensing and reproducibility considerations.

## Modelling Framework

### Econometric models

The econometric component includes:

- Autoregressive Distributed Lag models (ARDL);
- Nonlinear Autoregressive Distributed Lag models (NARDL); and
- bivariate Vector Autoregression models (VAR).

ARDL represents exchange rate effects symmetrically. NARDL separates depreciation and appreciation movements to evaluate asymmetric effects. VAR models food-price inflation and exchange-rate changes jointly as a dynamic system.

### Machine-learning models

The machine-learning component includes:

- Ridge Regression;
- Random Forest; and
- XGBoost.

Each machine-learning model is estimated using symmetric and asymmetric exchange-rate feature representations. A lag-one persistence forecast is included as the benchmark.

### Forecast variants

The final comparison contains ten forecast variants:

1. ARDL — Symmetric
2. NARDL — Asymmetric
3. VAR — Bivariate system
4. Ridge — Symmetric
5. Ridge — Asymmetric
6. Random Forest — Symmetric
7. Random Forest — Asymmetric
8. XGBoost — Symmetric
9. XGBoost — Asymmetric
10. Persistence — Lag-1 benchmark

Every variant produces 552 predictions for the same 46 food subclasses and 12 test months.

## Evaluation Strategy

Forecasts are evaluated using:

- Mean Absolute Error (MAE);
- Root Mean Squared Error (RMSE);
- coefficient of determination (R²);
- directional accuracy;
- mean forecast bias;
- improvement relative to the persistence benchmark;
- monthly loss-differential tests with HAC-adjusted inference;
- subclass-level Wilcoxon signed-rank tests; and
- Holm corrections for multiple comparisons.

Performance is examined at three levels:

1. pooled performance across the complete locked test sample;
2. performance within each of the 46 food subclasses; and
3. performance within each of the 12 test months.

## Research Contribution

The project contributes a unified comparison of econometric and machine-learning ERPT models across disaggregated South African food categories.

Its main contribution is not the identification of one universally superior model. Instead, it demonstrates that:

- ERPT relationships are category-specific;
- econometric models provide interpretable evidence about adjustment and asymmetry;
- machine-learning models add useful predictive value for many subclasses;
- asymmetric features do not automatically improve forecast accuracy; and
- model selection should account for the food category, evaluation criterion and intended analytical purpose.

The project also provides a reproducible notebook pipeline, validated result tables, forecast-comparison figures and formal statistical tests.

## Repository Navigation

The full analysis is implemented in the numbered notebooks under `notebooks/`.

Supporting documentation is available in:

- [`methodology.md`](methodology.md)
- [`data_dictionary.md`](data_dictionary.md)
- [`model_specifications.md`](model_specifications.md)
- [`reproducibility_guide.md`](reproducibility_guide.md)
- [`results_summary.md`](results_summary.md)
- [`limitations_and_future_work.md`](limitations_and_future_work.md)

Validated outputs are stored under:

- `reports/tables/econometrics/`
- `reports/tables/machine_learning/`
- `reports/tables/model_evaluation/`
- `reports/figures/econometrics/`
- `reports/figures/model_evaluation/`
