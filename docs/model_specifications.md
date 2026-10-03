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
+
