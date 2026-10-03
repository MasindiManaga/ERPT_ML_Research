# Research Overview

## Project Title

**Asymmetric Exchange Rate Pass-Through Across Food Price Categories in South Africa: A Comparison of Econometric and Machine Learning Models**

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

1. Which food subclasses provide valid evidence of a long-run relationship between exchange rate movements and food-price
