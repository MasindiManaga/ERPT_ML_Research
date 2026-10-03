# Results Summary

## 1. Purpose

This document summarises the principal econometric and forecasting results produced by the completed research pipeline.

The analysis covers:

- 46 South African food subclasses;
- symmetric and asymmetric exchange-rate representations;
- ARDL, NARDL and VAR econometric models;
- Ridge Regression, Random Forest and XGBoost machine-learning models;
- a lag-one persistence benchmark; and
- a locked January–December 2025 test sample.

The final comparison contains ten forecast variants and 5,520 prediction records.

## 2. Main Findings

The central findings are:

1. Long-run exchange rate pass-through exists for a limited group of food subclasses.
2. Corrected long-run asymmetry is category-specific rather than widespread.
3. Short-run asymmetry is sensitive to the chosen multiple-testing correction.
4. ARDL produces the lowest pooled test RMSE and highest pooled test R².
5. Symmetric XGBoost produces the lowest pooled MAE.
6. Symmetric Random Forest produces the highest directional accuracy.
7. Machine-learning models win in more food subclasses than econometric models, although no single model dominates.
8. Asymmetric feature representations do not provide a general forecasting advantage.
9. Model performance changes across food subclasses and months.
10. Econometric and machine-learning approaches provide complementary evidence.

## 3. Econometric Model Coverage

The econometric analysis estimates:

```text
46 ARDL models
46 NARDL models
46 VAR models
```

The ARDL and NARDL lag search evaluates:

```text
3,312 candidate specifications
```

All candidates are estimated successfully.

The VAR lag search evaluates:

```text
276 candidate specifications
```

All VAR candidates are estimated successfully.

BIC selects a VAR lag order of one for all 46 food subclasses.

## 4. Econometric Diagnostics

The combined diagnostic analysis covers 184 model-diagnostic records.

The results show:

| Diagnostic outcome | Models |
|---|---:|
| Models passing core diagnostics | 9 |
| Models with normal residuals | 5 |
| Models with non-normal residuals | 4 |
| Duplicate model-subclass records | 0 |
| Missing diagnostic values | 0 |

All nine bounds-supported models pass the core serial-correlation, ARCH and parameter-stability requirements.

Four models have non-normal residuals. HAC-adjusted inference is therefore used to improve the robustness of standard errors, confidence intervals and p-values.

HAC adjustment does not change the estimated coefficients.

## 5. Long-Run Model Validity

The bounds-testing and adjustment-coefficient assessment identifies:

| Outcome | Models |
|---|---:|
| Bounds-supported models | 9 |
| Valid long-run models | 8 |
| Bounds evidence without stable adjustment | 1 |

The ARDL model for chocolate, cocoa and cocoa-based food products has bounds-test support but does not have a statistically significant negative adjustment coefficient. It is therefore not classified as a valid long-run model.

The valid ARDL long-run models are:

- other vegetables, fresh or chilled; and
- yoghurt and similar products.

The valid NARDL long-run models are:

- cereals;
- chocolate, cocoa and cocoa-based food products;
- dates, figs and tropical fruits, fresh;
- fruit-bearing vegetables, fresh or chilled;
- other vegetables, fresh or chilled; and
- yoghurt and similar products.

These results show that evidence of long-run ERPT is present, but it is not universal across all 46 food subclasses.

## 6. Adjustment Coefficients

Nine adjustment coefficients are extracted from the bounds-supported ARDL and NARDL models.

The HAC-adjusted results confirm:

- two statistically stable ARDL adjustment processes;
- six statistically stable NARDL adjustment processes; and
- one statistically insignificant ARDL adjustment process.

No adjustment-stability conclusion changes when conventional inference is replaced by HAC-adjusted inference.

This indicates that the long-run validity classifications are robust to the covariance adjustment.

## 7. Long-Run Exchange-Rate Effects

Fourteen HAC-adjusted long-run effects are estimated from the valid ARDL and NARDL models.

The long-run effect analysis finds:

| Result | Effects |
|---|---:|
| Total long-run effects | 14 |
| Conventionally significant | 6 |
| HAC significant | 7 |
| Changed significance conclusions | 1 |

The one changed conclusion concerns the NARDL appreciation component for yoghurt and similar products.

Its conventional p-value is:

```text
0.107991
```

Its HAC-adjusted p-value is:

```text
0.022034
```

The effect therefore becomes statistically significant under HAC-adjusted inference.

## 8. Long-Run Asymmetry

Six NARDL long-run asymmetry tests are completed.

### Unadjusted results

Four food subclasses have unadjusted HAC p-values below 0.05:

- fruit-bearing vegetables, fresh or chilled;
- yoghurt and similar products;
- chocolate, cocoa and cocoa-based food products; and
- dates, figs and tropical fruits, fresh.

### Corrected results

After applying Holm and Benjamini–Hochberg corrections, corrected evidence of long-run asymmetry remains for two subclasses:

| Food subclass | HAC p-value | Holm-adjusted p-value | FDR-adjusted p-value |
|---|---:|---:|---:|
| Fruit-bearing vegetables, fresh or chilled | 0.000359 | 0.002155 | 0.002155 |
| Yoghurt and similar products | 0.007751 | 0.038755 | 0.023253 |

Chocolate and tropical fruits have raw asymmetry evidence, but their conclusions do not survive the multiple-testing corrections.

The final result is therefore:

```text
Holm-supported long-run asymmetry = 2 food subclasses
```

Long-run asymmetry is present, but it is category-specific.

## 9. Short-Run Asymmetry

Short-run HAC tests are completed for all 46 food subclasses.

The short-run results are:

| Decision rule | Significant subclasses |
|---|---:|
| Unadjusted p-value | 6 |
| Holm adjustment | 0 |
| Benjamini–Hochberg adjustment | 3 |

The three FDR-supported subclasses are:

| Food subclass | HAC p-value | FDR-adjusted p-value |
|---|---:|---:|
| Bread and bakery products | 0.001515 | 0.036456 |
| Stone fruits and pome fruits, fresh | 0.001585 | 0.036456 |
| Pulses | 0.002566 | 0.039340 |

No short-run asymmetry result remains significant after the stricter Holm correction.

The interpretation is therefore:

- there is no confirmatory Holm-supported short-run asymmetry;
- there is limited FDR-supported exploratory evidence; and
- short-run asymmetry conclusions are sensitive to the multiple-testing rule.

## 10. Machine-Learning Validation Results

The machine-learning development process compares Ridge Regression, Random Forest and XGBoost using symmetric and asymmetric feature representations.

### Selected validation performance

| Model | Representation | MAE | RMSE | R² | Directional accuracy |
|---|---|---:|---:|---:|---:|
| XGBoost | Symmetric | 1.046677 | 1.722070 | 0.177449 | 63.949275% |
| XGBoost | Asymmetric | 1.058929 | 1.728267 | 0.171518 | 63.405797% |
| Random Forest | Symmetric | 1.059113 | 1.759446 | 0.141356 | 64.855072% |
| Random Forest | Asymmetric | 1.077349 | 1.796596 | 0.104713 | 63.949275% |
| Ridge | Symmetric | 1.113336 | 1.869568 | 0.030509 | 63.768116% |
| Ridge | Asymmetric | 1.118287 | 1.873179 | 0.026759 | 63.586957% |
| Persistence | Lag-1 benchmark | 1.352852 | 2.340242 | −0.519088 | 56.340580% |

Symmetric XGBoost has the lowest validation RMSE.

All six fitted machine-learning pipelines are saved and successfully reproduce their original 552 test predictions with a maximum prediction difference of zero.

## 11. Overall Locked-Test Performance

The final locked-test sample contains:

```text
552 observations
46 food subclasses
12 months
10 forecast variants
5,520 prediction records
```

### Overall performance ranking by RMSE

| RMSE rank | Approach | Model | Representation | MAE | RMSE | R² | Directional accuracy |
|---:|---|---|---|---:|---:|---:|---:|
| 1 | Econometric | ARDL | Symmetric | 1.106859 | 1.841097 | 0.192124 | 59.963768% |
| 2 | Econometric | NARDL | Asymmetric | 1.106109 | 1.848766 | 0.185379 | 60.507246% |
| 3 | Econometric | VAR | Bivariate system | 1.061515 | 1.895828 | 0.143378 | 62.862319% |
| 4 | Machine learning | XGBoost | Symmetric | 1.060854 | 1.929075 | 0.113069 | 59.782609% |
| 5 | Machine learning | Random Forest | Symmetric | 1.066689 | 1.964122 | 0.080550 | 63.405797% |
| 6 | Machine learning | XGBoost | Asymmetric | 1.090656 | 1.973192 | 0.072038 | 61.231884% |
| 7 | Machine learning | Ridge | Symmetric | 1.089515 | 1.984985 | 0.060913 | 60.869565% |
| 8 | Machine learning | Ridge | Asymmetric | 1.092858 | 1.986590 | 0.059394 | 61.050725% |
| 9 | Machine learning | Random Forest | Asymmetric | 1.075816 | 1.991030 | 0.055185 | 61.231884% |
| 10 | Benchmark | Persistence | Lag-1 benchmark | 1.333117 | 2.235414 | −0.190988 | 54.347826% |

## 12. Metric Leaders

Different models lead under different evaluation criteria.

| Criterion | Leading model | Representation | Value |
|---|---|---|---:|
| Lowest RMSE | ARDL | Symmetric | 1.841097 |
| Highest R² | ARDL | Symmetric | 0.192124 |
| Lowest MAE | XGBoost | Symmetric | 1.060854 |
| Highest directional accuracy | Random Forest | Symmetric | 63.405797% |
| Lowest absolute mean bias | Persistence | Lag-1 benchmark | 0.025886 |

The persistence benchmark has the lowest absolute mean bias but the highest MAE and RMSE and a negative R². Low average bias therefore does not imply high forecast accuracy.

## 13. Improvement Over Persistence

Every fitted econometric and machine-learning model improves on persistence in terms of pooled RMSE and MAE.

ARDL produces:

```text
RMSE improvement over persistence = 17.639557%
MAE improvement over persistence = 16.972105%
```

Symmetric XGBoost produces:

```text
RMSE improvement over persistence = 13.703894%
MAE improvement over persistence = 20.423091%
```

ARDL controls large squared errors most effectively, while symmetric XGBoost produces the smallest typical absolute error.

## 14. Interpreting the RMSE and MAE Difference

ARDL ranks first by RMSE but ninth by MAE.

Symmetric XGBoost ranks first by MAE but fourth by RMSE.

This difference indicates that:

- ARDL is more effective at controlling relatively large forecast errors;
- XGBoost produces smaller absolute errors for the typical observation; and
- model rankings depend on the loss function used.

A single performance metric is therefore insufficient for selecting a universally best model.

## 15. Subclass-Level Winners

The subclass analysis contains:

```text
46 food subclasses
10 forecast variants per subclass
460 subclass-model results
12 test observations per result
```

### Winners by modelling approach

| Winning approach | Food subclasses |
|---|---:|
| Machine learning | 25 |
| Econometric | 16 |
| Persistence benchmark | 5 |

### Winners by model and representation

| Approach | Model | Representation | Food subclasses won |
|---|---|---|---:|
| Machine learning | Random Forest | Symmetric | 8 |
| Econometric | VAR | Bivariate system | 8 |
| Machine learning | XGBoost | Symmetric | 7 |
| Benchmark | Persistence | Lag-1 benchmark | 5 |
| Econometric | ARDL | Symmetric | 4 |
| Econometric | NARDL | Asymmetric | 4 |
| Machine learning | XGBoost | Asymmetric | 3 |
| Machine learning | Ridge | Symmetric | 3 |
| Machine learning | Random Forest | Asymmetric | 2 |
| Machine learning | Ridge | Asymmetric | 2 |

Machine learning provides the best subclass forecast for 25 of the 46 food subclasses.

However, no single machine-learning model dominates. The largest individual winner counts are shared by symmetric Random Forest and VAR, with eight subclasses each.

## 16. Monthly Winners

The monthly comparison contains:

```text
12 test months
10 forecast variants per month
120 monthly-model results
46 observations per monthly result
```

### Winning model by month

| Month | Approach | Model | Representation | Winning RMSE |
|---|---|---|---|---:|
| January 2025 | Machine learning | Random Forest | Symmetric | 1.932682 |
| February 2025 | Econometric | ARDL | Symmetric | 1.143124 |
| March 2025 | Econometric | ARDL | Symmetric | 1.385422 |
| April 2025 | Econometric | ARDL | Symmetric | 1.692168 |
| May 2025 | Benchmark | Persistence | Lag-1 benchmark | 1.640086 |
| June 2025 | Econometric | NARDL | Asymmetric | 1.470251 |
| July 2025 | Benchmark | Persistence | Lag-1 benchmark | 1.713075 |
| August 2025 | Machine learning | XGBoost | Symmetric | 1.554883 |
| September 2025 | Benchmark | Persistence | Lag-1 benchmark | 1.566576 |
| October 2025 | Benchmark | Persistence | Lag-1 benchmark | 1.764813 |
| November 2025 | Econometric | NARDL | Asymmetric | 1.614874 |
| December 2025 | Machine learning | Random Forest | Symmetric | 1.359381 |

### Months won

| Model | Representation | Months won |
|---|---|---:|
| Persistence | Lag-1 benchmark | 4 |
| ARDL | Symmetric | 3 |
| NARDL | Asymmetric | 2 |
| Random Forest | Symmetric | 2 |
| XGBoost | Symmetric | 1 |

Performance therefore changes materially across test months.

Persistence remains competitive in four months despite ranking last across the pooled 12-month sample.

## 17. Forecast-Accuracy Tests Against ARDL

ARDL is used as the reference model because it has the lowest pooled RMSE.

### Monthly HAC loss-differential tests

The monthly comparisons use 12 monthly squared-loss observations.

Before Holm adjustment, ARDL has statistically lower loss than:

- persistence;
- asymmetric Random Forest;
- asymmetric Ridge; and
- symmetric Ridge.

After applying the Holm correction, none of the nine monthly comparisons remains statistically significant.

For ARDL versus persistence:

```text
Raw p-value = 0.038639
Holm-adjusted p-value = 0.347748
```

The pooled RMSE advantage is therefore not sufficient to establish broad monthly superiority after controlling for multiple comparisons.

### Subclass-level Wilcoxon tests

The subclass comparison uses losses from all 46 food subclasses.

ARDL has lower loss than persistence for:

```text
34 of 46 food subclasses
```

The result remains significant after Holm adjustment:

```text
Raw p-value = 0.000779
Holm-adjusted p-value = 0.007008
```

ARDL therefore provides statistically stronger subclass-level performance than persistence.

No other ARDL-versus-alternative comparison remains significant after Holm adjustment.

## 18. Symmetric Versus Asymmetric Forecasting

### Pooled RMSE comparison

The symmetric representation has a lower pooled RMSE for:

- Ridge Regression;
- Random Forest;
- XGBoost; and
- ARDL compared with NARDL.

The pooled differences are:

| Comparison | Symmetric RMSE | Asymmetric RMSE | Lower RMSE |
|---|---:|---:|---|
| Ridge | 1.984985 | 1.986590 | Symmetric |
| Random Forest | 1.964122 | 1.991030 | Symmetric |
| XGBoost | 1.929075 | 1.973192 | Symmetric |
| ARDL versus NARDL | 1.841097 | 1.848766 | Symmetric |

### Monthly representation tests

| Comparison | Raw p-value | Holm-adjusted p-value | Significant after Holm |
|---|---:|---:|---|
| XGBoost | 0.007953 | 0.031813 | Yes |
| Ridge | 0.032583 | 0.097748 | No |
| Random Forest | 0.165416 | 0.330831 | No |
| ARDL versus NARDL | 0.656671 | 0.656671 | No |

Only XGBoost shows a significant monthly advantage for the symmetric representation after Holm adjustment.

### Subclass representation tests

| Comparison | Symmetric better | Asymmetric better | Holm-adjusted p-value | Significant after Holm |
|---|---:|---:|---:|---|
| Ridge | 35 | 11 | 0.001363 | Yes |
| XGBoost | 30 | 16 | 0.027044 | Yes |
| Random Forest | 23 | 23 | 1.000000 | No |
| ARDL versus NARDL | 20 | 26 | 1.000000 | No |

Ridge and XGBoost show significant subclass-level advantages for their symmetric representations.

Random Forest is evenly divided across subclasses.

NARDL wins in more subclasses than ARDL, but the paired loss difference is not statistically significant.

## 19. Predictive Value of Asymmetry

The results do not support a general claim that asymmetric exchange-rate features improve forecasts.

The evidence shows:

- symmetric variants have lower pooled RMSE in all four direct comparisons;
- symmetric XGBoost has a significant monthly advantage;
- symmetric Ridge and XGBoost have significant subclass-level advantages;
- Random Forest has no consistent representation preference;
- ARDL and NARDL have statistically similar forecast performance; and
- asymmetric variants still win for selected individual subclasses and months.

Asymmetry is therefore useful selectively rather than universally.

## 20. Relationship Between Econometric and Machine-Learning Results

The econometric and machine-learning results answer different but related questions.

Econometric models provide:

- tests of long-run relationships;
- adjustment coefficients;
- interpretable long-run multipliers;
- short-run and long-run asymmetry tests; and
- explicit statistical inference.

Machine-learning models provide:

- flexible nonlinear prediction;
- category interactions;
- competitive absolute-error performance;
- improved directional accuracy; and
- strong forecasts for many individual subclasses.

The results indicate complementarity rather than replacement.

ARDL performs best under pooled RMSE, while machine learning produces the largest number of subclass winners.

## 21. Research Questions Answered

### Does long-run ERPT exist across all food subclasses?

No. Bounds support is limited to nine model-subclass combinations, and eight satisfy the complete long-run validity rule.

### Is ERPT generally asymmetric?

No. Corrected long-run asymmetry is identified for two food subclasses. Confirmatory short-run asymmetry is not supported after Holm correction.

### Do asymmetric features improve machine-learning forecasts?

Not generally. Symmetric variants perform better overall for Ridge, Random Forest and XGBoost.

### Does machine learning outperform econometric modelling?

Not under every criterion. ARDL has the lowest pooled RMSE and highest R², while XGBoost has the lowest MAE and Random Forest has the highest directional accuracy.

### Does model performance vary by category?

Yes. Machine learning wins 25 subclasses, econometric models win 16 and persistence wins five.

### Is there one universally best model?

No. Rankings change across metrics, subclasses and months.

## 22. Overall Interpretation

The study does not identify one universally superior forecasting method.

Instead, it finds that:

- symmetric ARDL provides the strongest pooled protection against large forecast errors;
- symmetric XGBoost provides the smallest average absolute errors;
- symmetric Random Forest provides the strongest directional performance;
- machine learning adds substantial category-specific predictive value;
- long-run ERPT asymmetry exists for selected food subclasses;
- short-run asymmetry evidence is weaker and correction-sensitive; and
- model choice should depend on the food category and analytical objective.

The absence of universal machine-learning or asymmetric dominance is not a research failure. It is an empirical result demonstrating that ERPT and forecast performance are heterogeneous across South African food categories.

## 23. Supporting Files

The main supporting results are stored in:

```text
reports/tables/econometrics/econometric_evidence_summary.csv
reports/tables/econometrics/primary_econometric_model_assessment.csv
reports/tables/model_evaluation/overall_model_performance.csv
reports/tables/model_evaluation/subclass_model_winners.csv
reports/tables/model_evaluation/monthly_model_winners.csv
reports/tables/model_evaluation/forecast_accuracy_tests.csv
reports/tables/model_evaluation/subclass_accuracy_tests.csv
reports/tables/model_evaluation/monthly_representation_tests.csv
reports/tables/model_evaluation/subclass_representation_tests.csv
reports/tables/model_evaluation/final_evidence_summary.csv
```

The principal figures are:

```text
reports/figures/econometrics/hac_long_run_exchange_rate_effects.png
reports/figures/model_evaluation/overall_test_forecast_performance.png
reports/figures/model_evaluation/monthly_test_rmse_comparison.png
reports/figures/model_evaluation/symmetric_asymmetric_rmse_comparison.png
reports/figures/model_evaluation/subclass_forecast_model_winners.png
```
