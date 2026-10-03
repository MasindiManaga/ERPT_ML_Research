# Asymmetric Exchange Rate Pass-Through Across Food Price Categories in South Africa : A Comparison of Econometric and Machine Learning Models

**Author**: Masindi Managa

**Institution**: Eduvos

**Programme**: BSc Honours in Information Technology (Data Science)

---

## Project Overview

This repository contains the computational implementation of an Honours research project investigating exchange rate pass-through (ERPT) across 46 South African food subclasses.

The study compares conventional econometric models with machine-learning methods and evaluates whether separating currency depreciation and appreciation improves the modelling and forecasting of food price inflation.

The effective aligned modelling sample covers October 2017 to December 2025. Models were developed using observations ending in December 2024 and evaluated on a locked January–December 2025 test period.

---

## Research Objectives

The project aims to:

- investigate long-run and short-run exchange rate pass-through across disaggregated food subclasses;

- test whether depreciation and appreciation produce asymmetric food-price responses;

- compare econometric and machine-learning forecasting methods;

- compare symmetric and asymmetric exchange-rate representations;

- identify whether forecasting performance varies across food subclasses; and

- evaluate models using a leakage-safe out-of-sample forecasting design.

---

## Data Sources

The analysis uses monthly exchange-rate and food-price data obtained from:

- South African Reserve Bank exchange-rate records;

- Statistics South Africa consumer price index publications;

- CPI data classified according to COICOP 2018 food subclasses; and

- historical USD/ZAR exchange-rate records.

Raw and processed datasets are excluded from version control. The notebooks document the data preparation and transformation process.

---

## Methods

### Econometric models

- Autoregressive Distributed Lag model (ARDL)

- Nonlinear Autoregressive Distributed Lag model (NARDL)

- Vector Autoregression (VAR)

- Bounds testing for long-run relationships

- Error-correction analysis

- Long-run and short-run asymmetry testing

- HAC-adjusted statistical inference

- Ljung–Box serial-correlation testing

- ARCH-LM heteroskedasticity testing

- Jarque–Bera residual-normality testing

- CUSUM parameter-stability testing

- Holm and Benjamini–Hochberg multiple-testing corrections

### Machine-learning models

- Ridge Regression

- Random Forest

- XGBoost

- Symmetric exchange-rate features

- Asymmetric depreciation and appreciation features

### Forecast evaluation

The forecasting design uses rolling one-month-ahead predictions based only on information available before each forecast month. Hyperparameters and lag specifications were selected without using the locked 2025 test outcomes.

Models were evaluated using:

- mean absolute error (MAE);

- root mean squared error (RMSE);

- coefficient of determination ((R^2));

- directional accuracy;

- mean forecast bias;

- monthly HAC forecast-accuracy tests; and

- subclass-level Wilcoxon signed-rank robustness tests.

---
## Key Findings

- Nine models had bounds-test evidence of a long-run relationship, of which eight satisfied the final long-run validity requirements.

- Corrected long-run asymmetry was identified for fruit-bearing vegetables and yoghurt.

- No food subclass retained short-run asymmetry under Holm correction, although three subclasses retained evidence under false-discovery-rate control.

- Symmetric ARDL achieved the lowest pooled test RMSE of 1.841 and the highest test (R^2) of 0.192.

- Symmetric XGBoost achieved the lowest MAE of 1.061.

- Symmetric Random Forest achieved the highest directional accuracy of 63.41%.

- ARDL reduced RMSE by 17.64% relative to the persistence benchmark.

- Machine-learning models achieved the lowest subclass-level RMSE for 25 of the 46 food subclasses.

- Econometric models won 16 subclasses, while the persistence benchmark won five.

- Asymmetric predictors did not provide a universal forecasting advantage.

- Symmetric XGBoost significantly outperformed asymmetric XGBoost after multiple-testing correction.

The findings support a complementary, category-specific modelling strategy. Econometric models provide interpretable long-run evidence and strong pooled RMSE performance, while machine-learning models add forecasting value for many individual food categories.

---

## Repository Structure

```text
ERPT_ML_Research/
|-- data/
|   |-- external/                 
|   |-- processed/                
|   `-- raw/                      
|
|-- docs/
|   |-- research_overview.md
|   |-- methodology.md
|   |-- data_dictionary.md
|   |-- model_specifications.md
|   |-- reproducibility_guide.md
|   |-- results_summary.md
|   `-- limitations_and_future_work.md
|
|-- models/
|   `-- machine_learning/         
|
|-- notebooks/
|   |-- 01_Data_Acquisition_and_Understanding.ipynb
|   |-- 02_Data_Preparation.ipynb
|   |-- 03_Feature_Engineering.ipynb
|   |-- 04_Exploratory_Data_Analysis.ipynb
|   |-- 05_Modelling_Data_Preparation.ipynb
|   |-- 06_Econometric_Modelling.ipynb
|   |-- 07_Econometric_Results_and_Diagnostics.ipynb
|   |-- 08_Machine_Learning_Modelling.ipynb
|   `-- 09_Machine_Learning_Evaluation_and_Comparison.ipynb
|
|-- reports/
|   |-- archive/
|   |   `-- var/                 
|   |
|   |-- figures/
|   |   |-- econometrics/
|   |   `-- model_evaluation/
|   |
|   `-- tables/
|       |-- econometrics/
|       |-- machine_learning/
|       `-- model_evaluation/
|
|-- src/                          
|-- tests/                      
|-- .gitignore
|-- README.md
`-- requirements.txt
```
---

## Project Documentation

Detailed technical documentation is available under the [`docs/`](docs/) directory.

| Document | Description |
|---|---|
| [Research Overview](docs/research_overview.md) | Research background, problem, aim, objectives, scope and contribution |
| [Methodology](docs/methodology.md) | Data preparation, feature engineering, modelling and evaluation workflow |
| [Data Dictionary](docs/data_dictionary.md) | Variable definitions, units, transformations and sign conventions |
| [Model Specifications](docs/model_specifications.md) | Econometric structures, lag selection, machine-learning features and hyperparameters |
| [Reproducibility Guide](docs/reproducibility_guide.md) | Environment setup, required source files, notebook order and validation checks |
| [Results Summary](docs/results_summary.md) | Main econometric, forecasting, category-level and statistical findings |
| [Limitations and Future Work](docs/limitations_and_future_work.md) | Study limitations, interpretation boundaries and recommended extensions |

The documentation complements the notebooks:

- the notebooks contain the executable analysis;
- the `docs/` directory explains the research design and findings;
- the `reports/` directory contains validated tables and figures; and
- the README provides the entry point to the complete project.

---

## Notebook Execution Order

Run the notebooks in numerical order because later notebooks consume datasets and results produced by earlier stages:

1. Data acquisition and understanding

2. Data preparation

3. Feature engineering

4. Exploratory data analysis

5. Modelling data preparation

6. Econometric modelling

7. Econometric results and diagnostics

8. Machine-learning modelling

9. Machine-learning evaluation and comparison

---

## Installation

Create and activate a virtual environment:

python -m venv .venv
.venv\Scripts\activate

Install the required packages:

python -m pip install --upgrade pip
python -m pip install -r requirements.txt

Launch JupyterLab:

jupyter lab

The notebooks can also be executed through Visual Studio Code using the project virtual environment as the selected Jupyter kernel.

---

## Generated Outputs

Final outputs are organised as follows:

- reports/tables/econometrics/ — econometric estimation and diagnostic results;

- reports/tables/machine_learning/ — tuning, validation and model-manifest results;

- reports/tables/model_evaluation/ — final combined forecast comparisons;

- reports/figures/econometrics/ — econometric result figures;

- reports/figures/model_evaluation/ — final forecast comparison figures; and

- models/machine_learning/ — locally generated fitted pipelines.

The trained pipeline files are excluded from Git because they are generated artifacts and some exceed GitHub’s file-size limit. They can be reproduced by running Notebook 8.

The legacy VAR files under reports/archive/var/ are retained for traceability but are not used in the final leakage-safe forecast comparison.

---

## Reproducibility

The project includes:

- chronological development and test separation;

- explicit leakage checks;

- validation of prediction counts and model variants;

- saved model manifests;

- output row and column validation;

- saved-pipeline prediction reproduction;

- econometric diagnostic tests; and

- multiple-testing-adjusted statistical inference.

## Project Status

The computational modelling and evaluation pipeline is complete. Final dissertation integration and reporting are in progress.
