# Limitations and Future Work

## 1. Purpose

This document identifies the main limitations of the study and outlines opportunities for extending the research.

The limitations do not invalidate the completed analysis. They define the conditions under which the findings should be interpreted and help distinguish supported conclusions from claims that require further evidence.

## 2. Effective Modelling Period

The final econometric dataset covers April 2017 to December 2025.

After constructing six-month lags and aligning all required machine-learning features, the common forecasting dataset covers October 2017 to December 2025.

The effective modelling period is therefore shorter than the full period represented by the original source files.

This reduces the number of time observations available for:

- estimating dynamic relationships;
- detecting structural change;
- fitting category-specific models; and
- evaluating long-run adjustment.

### Future work

Future research should update and extend the monthly series as new CPI and exchange-rate observations become available.

A longer sample would provide:

- more observations before and after major economic events;
- stronger statistical power;
- more stable lag selection;
- longer out-of-sample evaluation periods; and
- improved analysis of changing pass-through behaviour.

## 3. Twelve-Month Locked Test Period

The final test sample covers January to December 2025.

This provides:

```text
12 test observations per food subclass
46 observations per test month
552 total observations per forecast variant
```

The locked test design prevents the final outcomes from being used during model selection. However, a single 12-month test period may not represent every inflation, exchange-rate or economic regime.

Model rankings could differ during:

- periods of rapid depreciation;
- periods of exchange-rate appreciation;
- low-inflation periods;
- commodity-price shocks;
- supply-chain disruptions; or
- unusually stable economic conditions.

The monthly forecast-accuracy tests also contain only 12 loss observations. This limits their statistical power, particularly after Holm correction.

### Future work

Future studies should use:

- longer out-of-sample periods;
- rolling-origin evaluation;
- expanding-window evaluation;
- repeated yearly test windows; and
- forecast comparisons across different economic regimes.

## 4. Single Validation Period

Machine-learning hyperparameters are selected using the January–December 2024 validation period.

A single validation year may favour specifications that happen to perform well under the economic conditions observed during that year.

### Future work

The model-selection process could be strengthened through time-series cross-validation using multiple expanding or rolling validation windows.

For example:

```text
Fold 1: train through 2021, validate on 2022
Fold 2: train through 2022, validate on 2023
Fold 3: train through 2023, validate on 2024
```

Hyperparameters could then be selected using average performance across the validation folds.

## 5. Forecast Information Set

The machine-learning models use lagged food inflation and lagged exchange-rate variables.

The forecasts should therefore be interpreted according to the information-availability design implemented in the modelling notebook.

In operational use, it is important to distinguish between:

- a rolling one-step-ahead forecast, where the most recently observed inflation value is available before each forecast; and
- a fixed-origin multi-step forecast, where later lagged outcomes are not yet observed and must be generated recursively.

These two designs answer different forecasting questions and may produce different accuracy results.

### Future work

Future research should explicitly compare:

- rolling one-step-ahead forecasts;
- recursive multi-step forecasts;
- direct horizon-specific forecasts; and
- forecasts using projected exchange-rate paths.

All competing models should be evaluated using an identical information set at each forecast origin.

## 6. Limited Explanatory Variables

The final models focus mainly on:

- food-price persistence;
- exchange-rate movements;
- depreciation and appreciation shocks;
- food-subclass identity; and
- calendar seasonality.

Food prices are also affected by other domestic and international variables.

Potential omitted drivers include:

- global food commodity prices;
- fuel and transport costs;
- electricity prices;
- producer prices;
- interest rates;
- wages;
- rainfall and drought;
- agricultural production;
- import intensity;
- administered prices;
- supply-chain disruptions;
- market concentration;
- load shedding; and
- trade-policy changes.

The estimated exchange-rate effects may partially capture the influence of omitted variables that move with the exchange rate.

### Future work

Future models should incorporate carefully selected exogenous variables and evaluate whether they improve:

- forecast accuracy;
- long-run interpretation;
- stability;
- causal identification; and
- category-specific pass-through estimates.

## 7. Bivariate VAR Structure

The VAR model contains only:

- food-price inflation; and
- exchange-rate change.

This parsimonious system is suitable for a consistent category-level comparison but cannot represent the broader macroeconomic transmission process.

### Future work

An extended VAR or Bayesian VAR could include:

- fuel-price inflation;
- interest rates;
- producer-price inflation;
- global food-price indices;
- domestic demand indicators; and
- measures of supply disruption.

Care would be required because the available monthly sample is small relative to the number of possible system parameters.

## 8. Model Parsimony and Small-Sample Constraints

The development period contains 87 monthly observations for each food subclass.

This is limited for highly parameterised time-series models.

Although BIC favours parsimonious specifications, small samples may still produce:

- unstable coefficient estimates;
- uncertain long-run multipliers;
- weak diagnostic-test power;
- sensitivity to individual observations; and
- unstable asymmetry classifications.

The fact that most ARDL, NARDL and VAR models select low lag orders may partly reflect the strong penalty that BIC applies to complex models in small samples.

### Future work

Future analysis could assess specification stability through:

- rolling estimation;
- recursive coefficient plots;
- bootstrap confidence intervals;
- alternative information criteria;
- Bayesian shrinkage; and
- sensitivity tests using different maximum lag orders.

## 9. Structural Breaks and Regime Changes

The sample includes periods of substantial economic disruption.

Potential regime-changing events include:

- the COVID-19 pandemic;
- supply-chain disruptions;
- global food and energy price shocks;
- shifts in monetary policy;
- domestic electricity constraints; and
- periods of unusual exchange-rate volatility.

The implemented models do not explicitly estimate structural breaks or regime-switching relationships.

A coefficient estimated across the full sample may therefore represent an average across different economic regimes.

### Future work

Future research could apply:

- structural-break tests;
- breakpoint regressions;
- rolling-window estimation;
- threshold models;
- Markov-switching models; and
- regime-specific forecast comparisons.

## 10. Bounds Testing and Long-Run Inference

Nine models have bounds-test support, while eight satisfy the complete long-run validity rule.

Bounds-test conclusions may be sensitive to:

- sample size;
- deterministic terms;
- lag specification;
- critical-value selection; and
- structural breaks.

The error-correction validity rule reduces the risk of treating bounds evidence alone as sufficient, but it does not remove all small-sample uncertainty.

### Future work

Long-run findings could be evaluated using:

- bootstrap bounds tests;
- alternative cointegration procedures;
- small-sample critical values;
- structural-break-adjusted cointegration tests; and
- longer time series.

## 11. Non-Normal Residuals

Four of the nine bounds-supported models have non-normal residuals.

HAC covariance estimation improves the robustness of standard errors and p-values in the presence of heteroskedasticity and autocorrelation. It does not transform the residual distribution or correct all possible forms of model misspecification.

### Future work

Future research could investigate:

- robust regression;
- bootstrap inference;
- alternative error distributions;
- outlier-adjusted estimation;
- quantile models; and
- volatility models.

## 12. Multiple Hypothesis Testing

The project conducts asymmetry tests across several food subclasses.

Without correction, repeated testing increases the probability of false-positive findings.

The analysis addresses this issue by reporting:

- raw p-values;
- Holm-adjusted p-values; and
- false-discovery-rate-adjusted p-values.

However, the difference between the Holm and FDR short-run conclusions shows that the evidence remains sensitive to the correction rule.

### Future work

Future studies should:

- pre-specify primary hypotheses;
- distinguish confirmatory from exploratory testing;
- use larger samples;
- report adjusted and unadjusted results together; and
- assess whether identified asymmetries persist in later samples.

## 13. Asymmetry Definition

The analysis defines asymmetry by separating exchange-rate changes at zero:

```text
Positive exchange-rate change = depreciation
Negative exchange-rate change = appreciation
```

This approach assumes that all positive changes belong to one regime and all negative changes belong to another.

It does not distinguish between:

- small and large depreciations;
- temporary and persistent shocks;
- anticipated and unexpected movements; or
- high-volatility and low-volatility regimes.

### Future work

Alternative asymmetry definitions could examine:

- large versus small shocks;
- threshold effects;
- volatility-adjusted shocks;
- persistent versus transitory movements;
- cumulative shock duration; and
- interaction with inflation regimes.

## 14. Category-Level Heterogeneity

The use of 46 food subclasses is a major strength because it reveals substantial variation hidden by aggregate food-price indices.

However, subclass-level winners are calculated from only 12 test observations per subclass.

A single unusually large error can materially influence subclass RMSE and the selected winner.

### Future work

Category-level findings should be reassessed using:

- longer subclass test periods;
- bootstrap uncertainty around winner selection;
- rank stability analysis;
- model-confidence sets; and
- repeated forecast windows.

## 15. Pooled Machine-Learning Structure

The machine-learning models pool observations across food subclasses and include subclass identity as a categorical feature.

This improves sample size and enables shared learning across categories. However, a pooled model may not fully represent category-specific dynamics.

One-hot encoding identifies the subclass but does not automatically model every possible interaction between subclass characteristics and exchange-rate effects.

### Future work

Potential extensions include:

- separate machine-learning models for sufficiently large subclasses;
- hierarchical models;
- multi-task learning;
- category-specific interactions;
- grouped feature selection; and
- models that use food-class hierarchy explicitly.

## 16. Moderate Hyperparameter Search

The project evaluates structured candidate grids and performs a focused XGBoost refinement.

The search is sufficient for a transparent Honours-level comparison but does not cover every possible hyperparameter configuration.

Different search ranges or optimisation methods could produce stronger models.

### Future work

Future studies could use:

- randomised search;
- Bayesian optimisation;
- successive halving;
- early stopping;
- nested time-series validation; and
- computational-budget-aware optimisation.

Any broader search should preserve the separation between training, validation and locked test data.

## 17. Point Forecasts

The final evaluation compares point predictions.

The study does not provide forecast intervals or full predictive distributions for every model.

Point accuracy alone does not show:

- forecast uncertainty;
- the probability of extreme outcomes;
- interval calibration; or
- risk under alternative scenarios.

### Future work

Future research should evaluate:

- prediction intervals;
- quantile forecasts;
- conformal prediction;
- bootstrap forecast distributions;
- probabilistic scoring rules; and
- interval coverage.

## 18. Machine-Learning Interpretability

The machine-learning comparison focuses on predictive performance.

The study does not include a full explainable-AI analysis of:

- global feature importance;
- local prediction explanations;
- nonlinear feature effects; or
- category-specific interactions.

### Future work

Interpretability could be expanded through:

- permutation importance;
- SHAP values;
- partial-dependence plots;
- accumulated local effects;
- category-specific feature analysis; and
- comparison with econometric multiplier signs.

Interpretability results should be treated as model explanations rather than causal estimates.

## 19. No General Causal Claim

The models identify statistical relationships and forecast performance.

They do not establish that exchange-rate movements are the sole cause of observed food-price changes.

Possible confounding, omitted variables, simultaneous economic shocks and feedback relationships remain.

### Future work

Stronger causal analysis could use:

- external instruments;
- identified structural VAR models;
- natural experiments;
- local projections;
- event studies; and
- policy or commodity-price shock identification.

Such extensions would require assumptions beyond the current predictive comparison.

## 20. Geographic Scope

The study focuses on national South African food-price categories.

It does not estimate separate pass-through effects for:

- provinces;
- rural and urban areas;
- income groups;
- retailers; or
- households.

National averages may conceal substantial geographic and socioeconomic differences.

### Future work

The provincial CPI files could support an extended analysis of geographic heterogeneity where consistent category-level coverage is available.

## 21. Temporal Aggregation

The project uses monthly data.

Monthly aggregation is appropriate for CPI analysis but may conceal:

- within-month exchange-rate volatility;
- rapid retail-price adjustments;
- delayed price changes within the month; and
- short-lived shocks.

### Future work

Where suitable data are available, future analysis could compare:

- daily exchange-rate measures;
- weekly price indicators;
- monthly CPI outcomes; and
- alternative exchange-rate aggregation rules.

## 22. Data Revisions and Real-Time Availability

The analysis uses the final downloaded data files available during the project.

Official statistics can be revised, rebased or reclassified. A model estimated using final revised data may perform differently in a real-time setting where only initial releases are available.

### Future work

A real-time forecast evaluation could use:

- historical data vintages;
- initial CPI releases;
- revision tracking; and
- realistic publication delays.

## 23. Reproducibility Constraints

The repository includes:

- notebooks;
- model specifications;
- package versions;
- tracked result tables;
- tracked figures; and
- validation outputs.

It excludes:

- raw source data;
- generated processed data; and
- trained binary model files.

A complete reproduction therefore requires authorised access to the original source files.

The large fitted model pipelines must be regenerated by running Notebook 8.

### Future work

Reproducibility could be improved through:

- scripted data-download instructions where licensing permits;
- checksum records for source files;
- automated notebook execution;
- containerisation;
- continuous-integration tests; and
- archived environment specifications.

## 24. Model-Winner Instability

No model dominates every evaluation criterion.

The leading models differ by:

- RMSE;
- MAE;
- R²;
- directional accuracy;
- food subclass; and
- test month.

This means that selecting a single winner without specifying the operational objective can be misleading.

### Future work

A practical forecasting system could use:

- criterion-specific model selection;
- subclass-specific model assignment;
- dynamic model switching;
- forecast combinations;
- weighted ensembles; or
- model-confidence sets.

The subclass-winner results provide a starting point for such a system but should be validated on additional periods before deployment.

## 25. Future Research Priorities

The highest-priority extensions are:

1. Extend the out-of-sample period beyond 12 months.
2. Introduce rolling-origin and expanding-window validation.
3. Standardise forecast horizons and information sets across all models.
4. Add global food, fuel, producer-price and macroeconomic variables.
5. Test structural breaks and regime-dependent pass-through.
6. Produce prediction intervals and probabilistic forecasts.
7. Apply explainable-AI methods to the machine-learning models.
8. Reassess subclass winners across multiple test periods.
9. Develop category-specific or hierarchical forecasting models.
10. Evaluate forecast combinations across econometric and machine-learning approaches.

## 26. Final Limitation Statement

The study provides a validated comparison of econometric and machine-learning ERPT models across 46 South African food subclasses.

Its conclusions are strongest for:

- the implemented sample;
- the specified variables;
- the selected lag structures;
- the January–December 2025 locked test period; and
- the reported evaluation criteria.

The findings should not be interpreted as evidence that one model will remain superior in every future period.

The principal contribution is the demonstration that ERPT evidence and forecasting performance are heterogeneous across food categories and that econometric and machine-learning methods provide complementary value.
