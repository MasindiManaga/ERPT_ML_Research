# Reproducibility Guide

## 1. Purpose

This guide explains how to recreate the project environment, prepare the required local files, execute the notebook pipeline and validate the generated research outputs.

The project is designed as a sequential notebook workflow. Later notebooks depend on datasets and result files produced by earlier notebooks.

## 2. Repository

The project repository is available at:

```text
https://github.com/MasindiManaga/ERPT_ML_Research
```

Clone the repository using:

```cmd
git clone https://github.com/MasindiManaga/ERPT_ML_Research.git
cd ERPT_ML_Research
```

## 3. Software Requirements

The project was developed using:

```text
Python 3
JupyterLab
Git
Visual Studio Code
```

The main Python packages include:

```text
joblib
matplotlib
numpy
openpyxl
pandas
scikit-learn
scipy
seaborn
statsmodels
xgboost
```

Exact installed package versions are recorded in:

```text
requirements.txt
```

## 4. Windows Environment Setup

Open Command Prompt in the repository root.

Create a virtual environment:

```cmd
py -m venv .venv
```

Activate it:

```cmd
.venv\Scripts\activate
```

Upgrade pip:

```cmd
python -m pip install --upgrade pip
```

Install the project dependencies:

```cmd
python -m pip install -r requirements.txt
```

Verify that the environment contains no broken dependencies:

```cmd
python -m pip check
```

Expected output:

```text
No broken requirements found.
```

## 5. Linux or macOS Environment Setup

Create a virtual environment:

```bash
python3 -m venv .venv
```

Activate it:

```bash
source .venv/bin/activate
```

Upgrade pip:

```bash
python -m pip install --upgrade pip
```

Install the dependencies:

```bash
python -m pip install -r requirements.txt
```

Validate the environment:

```bash
python -m pip check
```

## 6. Verify Key Package Versions

The following command displays the main analytical package versions:

```cmd
python -c "import joblib, matplotlib, numpy, openpyxl, pandas, scipy, seaborn, sklearn, statsmodels, xgboost; print('joblib', joblib.__version__); print('matplotlib', matplotlib.__version__); print('numpy', numpy.__version__); print('openpyxl', openpyxl.__version__); print('pandas', pandas.__version__); print('scipy', scipy.__version__); print('seaborn', seaborn.__version__); print('scikit-learn', sklearn.__version__); print('statsmodels', statsmodels.__version__); print('xgboost', xgboost.__version__)"
```

The validated project environment used:

```text
joblib 1.5.3
matplotlib 3.11.0
numpy 2.5.1
openpyxl 3.1.5
pandas 3.0.3
scipy 1.18.0
seaborn 0.13.2
scikit-learn 1.9.0
statsmodels 0.14.6
xgboost 3.3.0
```

## 7. Required Local Data

Raw and processed datasets are intentionally excluded from Git version control.

The following source files must be placed under:

```text
data/raw/
```

Expected source files:

```text
CPI_Average Prices_All urban(202604).xlsx
CPI_Average prices_Provinces(202604).xlsx
EXCEL - CPI (COICOP 2018 - 8digit) (202604).xlsx
HistoricalRateDetail.csv
```

The exact filenames matter because the acquisition and preparation notebooks refer to the local source files.

The data directories contain `.gitkeep` placeholders so that the folder structure remains available after cloning.

## 8. Data-Management Rules

Do not commit the following files to the public repository:

```text
data/raw/*
data/processed/*
data/external/*
models/machine_learning/*.joblib
```

These paths are excluded through `.gitignore`.

Reasons include:

- source-file size;
- external-data licensing;
- preservation of a clean repository;
- generated-data reproducibility; and
- GitHub file-size restrictions.

## 9. Required Directory Structure

Before running the notebooks, the repository should contain:

```text
ERPT_ML_Research/
|-- data/
|   |-- external/
|   |-- processed/
|   `-- raw/
|-- docs/
|-- models/
|-- notebooks/
|-- reports/
|   |-- archive/
|   |-- figures/
|   `-- tables/
|-- src/
|-- tests/
|-- .gitignore
|-- LICENSE
|-- README.md
`-- requirements.txt
```

Generated model pipelines will be written to:

```text
models/machine_learning/
```

Generated econometric figures will be written to:

```text
reports/figures/econometrics/
```

Generated model-evaluation figures will be written to:

```text
reports/figures/model_evaluation/
```

Generated result tables will be written to:

```text
reports/tables/econometrics/
reports/tables/machine_learning/
reports/tables/model_evaluation/
```

## 10. Start JupyterLab

From the repository root, run:

```cmd
jupyter lab
```

Open the notebooks from the `notebooks/` folder.

Use the Python kernel associated with the active `.venv` environment.

If the environment does not appear as a kernel, run:

```cmd
python -m ipykernel install --user --name erpt_ml_research --display-name "Python (ERPT ML Research)"
```

Restart JupyterLab and select:

```text
Python (ERPT ML Research)
```

## 11. Notebook Execution Order

The notebooks must be run in numerical order.

### Notebook 1

```text
01_Data_Acquisition_and_Understanding.ipynb
```

Purpose:

- load the original source files;
- inspect file structures;
- identify relevant CPI and exchange-rate fields;
- examine date coverage;
- assess missing values; and
- confirm the source-data hierarchy.

### Notebook 2

```text
02_Data_Preparation.ipynb
```

Purpose:

- clean CPI records;
- standardise column names;
- clean the exchange-rate data;
- align monthly dates;
- merge CPI and exchange-rate observations;
- resolve data-quality issues; and
- save the prepared merged dataset.

### Notebook 3

```text
03_Feature_Engineering.ipynb
```

Purpose:

- calculate food-price inflation;
- calculate exchange-rate log changes;
- construct depreciation and appreciation shocks;
- construct cumulative asymmetric components;
- generate lagged predictors;
- generate seasonal variables; and
- save the featured dataset.

### Notebook 4

```text
04_Exploratory_Data_Analysis.ipynb
```

Purpose:

- describe the food-price and exchange-rate series;
- examine category-level patterns;
- analyse distributions and correlations;
- visualise trends and volatility; and
- investigate potential symmetric and asymmetric relationships.

### Notebook 5

```text
05_Modelling_Data_Preparation.ipynb
```

Purpose:

- create the econometric modelling dataset;
- create the machine-learning modelling dataset;
- define chronological samples;
- validate feature completeness;
- exclude current targets from predictors; and
- save the final modelling files.

### Notebook 6

```text
06_Econometric_Modelling.ipynb
```

Purpose:

- estimate ARDL lag candidates;
- estimate NARDL lag candidates;
- select subclass-specific specifications using BIC;
- conduct bounds testing;
- estimate long-run multipliers;
- estimate error-correction models;
- conduct asymmetry tests; and
- export econometric model results.

### Notebook 7

```text
07_Econometric_Results_and_Diagnostics.ipynb
```

Purpose:

- combine model diagnostics;
- assess serial correlation;
- assess ARCH effects;
- assess residual normality;
- assess parameter stability;
- apply HAC covariance estimation;
- recalculate long-run inference;
- apply multiple-testing adjustments;
- classify econometric evidence; and
- export validated econometric tables and figures.

### Notebook 8

```text
08_Machine_Learning_Modelling.ipynb
```

Purpose:

- define symmetric and asymmetric features;
- create preprocessing pipelines;
- calculate persistence validation performance;
- tune Ridge Regression;
- tune Random Forest;
- tune and refine XGBoost;
- select models using validation RMSE;
- refit selected pipelines using the full development period;
- generate locked-test predictions;
- save fitted pipelines locally; and
- export validated machine-learning results.

### Notebook 9

```text
09_Machine_Learning_Evaluation_and_Comparison.ipynb
```

Purpose:

- load machine-learning predictions;
- generate ARDL, NARDL and VAR forecasts;
- combine all ten forecast variants;
- calculate overall performance;
- calculate subclass-level performance;
- calculate month-level performance;
- identify forecast winners;
- conduct formal forecast-accuracy tests;
- compare symmetric and asymmetric variants;
- produce final figures;
- create the evidence summary; and
- validate all exported outputs.

## 12. Recommended Notebook Procedure

For every notebook:

1. Open the notebook.
2. Restart the kernel.
3. Select **Run All Cells**.
4. Wait for execution to finish.
5. Review the final validation output.
6. Confirm that the completion flag is `True`.
7. Save the executed notebook.
8. Continue to the next notebook.

Do not run isolated later cells before the earlier setup cells. Many notebook variables are created in memory and are required by subsequent cells.

## 13. Expected Processed Data

After running the data-preparation and feature-engineering notebooks, the following local files should exist:

```text
data/processed/merged_data.csv
data/processed/featured_data.csv
data/processed/eda_relationship.csv
data/processed/econometric_model_data.csv
data/processed/ml_model_data.csv
```

These files are generated locally and remain excluded from Git.

## 14. Expected Machine-Learning Models

After running Notebook 8, the following local files should exist:

```text
models/machine_learning/ridge_symmetric_pipeline.joblib
models/machine_learning/ridge_asymmetric_pipeline.joblib
models/machine_learning/random_forest_symmetric_pipeline.joblib
models/machine_learning/random_forest_asymmetric_pipeline.joblib
models/machine_learning/xgboost_symmetric_pipeline.joblib
models/machine_learning/xgboost_asymmetric_pipeline.joblib
```

The expected fitted-pipeline count is:

```text
6
```

These model files are generated artifacts and are not tracked by Git.

## 15. Expected Econometric Outputs

The econometric output folder is:

```text
reports/tables/econometrics/
```

Important files include:

```text
symmetric_lag_selection.csv
asymmetric_lag_selection.csv
final_bounds_results.csv
error_correction_results.csv
long_run_effects.csv
long_run_asymmetry_results.csv
short_run_asymmetry_results.csv
combined_model_diagnostics.csv
hac_adjustment_results.csv
hac_long_run_effects.csv
hac_long_run_asymmetry_results.csv
hac_short_run_asymmetry_results.csv
primary_econometric_model_assessment.csv
econometric_evidence_summary.csv
```

The main econometric figure is:

```text
reports/figures/econometrics/hac_long_run_exchange_rate_effects.png
```

## 16. Expected Machine-Learning Outputs

The machine-learning output folder is:

```text
reports/tables/machine_learning/
```

Expected files include:

```text
ridge_tuning_results.csv
selected_ridge_models.csv
random_forest_tuning_results.csv
selected_random_forest_models.csv
xgboost_tuning_results.csv
selected_xgboost_models.csv
validation_model_comparison.csv
feature_representation_comparison.csv
leakage_and_output_audit.csv
ml_test_predictions.csv
model_manifest.csv
```

## 17. Expected Final Evaluation Outputs

The final evaluation table folder is:

```text
reports/tables/model_evaluation/
```

Expected files:

```text
combined_forecast_evaluation.csv
combined_test_predictions.csv
final_evidence_summary.csv
forecast_accuracy_tests.csv
monthly_model_performance.csv
monthly_model_winners.csv
monthly_representation_tests.csv
overall_model_performance.csv
subclass_accuracy_tests.csv
subclass_model_performance.csv
subclass_model_winners.csv
subclass_representation_tests.csv
subclass_winner_counts.csv
```

The final evaluation figure folder is:

```text
reports/figures/model_evaluation/
```

Expected figures:

```text
monthly_test_rmse_comparison.png
overall_test_forecast_performance.png
subclass_forecast_model_winners.png
symmetric_asymmetric_rmse_comparison.png
```

## 18. Expected Final Dimensions

The final combined prediction table must contain:

```text
10 forecast variants
552 predictions per variant
5,520 total prediction records
46 food subclasses
12 test months
```

The expected evaluation-table dimensions include:

| File | Rows | Columns |
|---|---:|---:|
| `combined_test_predictions.csv` | 5,520 | 6 |
| `combined_forecast_evaluation.csv` | 5,520 | 7 |
| `overall_model_performance.csv` | 10 | 15 |
| `subclass_model_performance.csv` | 460 | 11 |
| `subclass_model_winners.csv` | 46 | 11 |
| `subclass_winner_counts.csv` | 10 | 5 |
| `monthly_model_performance.csv` | 120 | 10 |
| `monthly_model_winners.csv` | 12 | 10 |
| `forecast_accuracy_tests.csv` | 9 | 11 |
| `subclass_accuracy_tests.csv` | 9 | 10 |
| `monthly_representation_tests.csv` | 4 | 12 |
| `subclass_representation_tests.csv` | 4 | 12 |
| `final_evidence_summary.csv` | 9 | 3 |

## 19. Required Validation Results

A successful complete run should confirm:

```text
Missing modelling values = 0
Duplicate subclass-month rows = 0
Missing predictions = 0
Non-finite predictions = 0
Duplicate prediction records = 0
```

It should also confirm:

```text
All leakage and output checks passed = True
All saved pipelines validated = True
All ARDL and NARDL forecast checks passed = True
All VAR forecast checks passed = True
All combined forecast checks passed = True
All exported evaluation tables validated = True
All evaluation figures validated = True
Notebook 9 successfully completed = True
```

## 20. Expected Benchmark Results

Small floating-point differences may occur across operating systems or package builds, but a successful reproduction should produce results close to:

| Model | Representation | Test RMSE |
|---|---|---:|
| ARDL | Symmetric | 1.841097 |
| NARDL | Asymmetric | 1.848766 |
| VAR | Bivariate system | 1.895828 |
| XGBoost | Symmetric | 1.929075 |
| Random Forest | Symmetric | 1.964122 |
| XGBoost | Asymmetric | 1.973192 |
| Ridge | Symmetric | 1.984985 |
| Ridge | Asymmetric | 1.986590 |
| Random Forest | Asymmetric | 1.991030 |
| Persistence | Lag-1 benchmark | 2.235414 |

The expected metric leaders are:

```text
Lowest RMSE: ARDL — Symmetric
Lowest MAE: XGBoost — Symmetric
Highest R2: ARDL — Symmetric
Highest directional accuracy: Random Forest — Symmetric
```

## 21. Reproducing Saved Pipeline Predictions

Notebook 8 reloads each saved machine-learning pipeline and compares its predictions with the exported prediction table.

A successful reproduction requires:

```text
Expected predictions per pipeline = 552
Matched predictions per pipeline = 552
Maximum prediction difference = 0
Predictions reproduced = True
```

## 22. Common Errors

### `NameError`

Example:

```text
NameError: name 'validation_comparison' is not defined
```

Cause:

- a required earlier cell was not executed;
- the kernel was restarted; or
- a variable was created under a different name.

Resolution:

1. restart the notebook kernel;
2. run all cells from the beginning;
3. use the variable names created by the current notebook; and
4. avoid relying on objects from a previous notebook session.

### Missing raw file

Example:

```text
FileNotFoundError
```

Resolution:

- confirm that all required source files exist under `data/raw/`;
- confirm that filenames match exactly;
- confirm that the notebook is running from the project environment; and
- inspect the input path defined near the beginning of the notebook.

### Missing package

Example:

```text
ModuleNotFoundError
```

Resolution:

```cmd
.venv\Scripts\activate
python -m pip install -r requirements.txt
python -m pip check
```

### XGBoost or scikit-learn compatibility issue

Confirm the installed versions:

```cmd
python -c "import sklearn, xgboost; print(sklearn.__version__); print(xgboost.__version__)"
```

Validated versions:

```text
scikit-learn 1.9.0
xgboost 3.3.0
```

### Git line-ending warning

Windows may display:

```text
LF will be replaced by CRLF the next time Git touches it
```

This is a line-ending warning rather than a modelling or data error.

### Large model file

The Random Forest pipeline files can be large. They must remain excluded from Git.

Check the ignore rule using:

```cmd
git check-ignore -v models\machine_learning\random_forest_symmetric_pipeline.joblib
```

## 23. Git Validation

After modifying notebooks or tracked outputs, inspect the repository using:

```cmd
git status --short
git diff --check
git diff --stat
```

Before committing, inspect staged files using:

```cmd
git diff --cached --stat
git diff --cached --check
```

Confirm that no model binaries are staged:

```cmd
git diff --cached --name-only | findstr /i ".joblib"
```

No output indicates that no `.joblib` files are staged.

## 24. Reproducibility Boundary

The repository provides:

- the complete notebook implementation;
- package-version information;
- model specifications;
- tracked result tables;
- tracked figures;
- validation outputs; and
- documentation of the required source files.

The repository does not distribute:

- the original raw datasets;
- generated processed datasets; or
- fitted binary model artifacts.

A complete reproduction therefore requires authorised access to the original CPI and exchange-rate source files.

## 25. Final Completion Checklist

A full reproduction is complete when:

- all nine notebooks run in order;
- every final notebook validation returns `True`;
- the processed datasets contain no missing or duplicate modelling records;
- all ten forecast variants are present;
- every forecast variant contains 552 predictions;
- the combined prediction table contains 5,520 rows;
- all 46 subclass winners are identified;
- all 12 monthly winners are identified;
- all 13 final evaluation tables are validated;
- all four final evaluation figures are validated; and
- Notebook 9 reports successful completion.
