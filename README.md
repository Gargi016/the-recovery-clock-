# The Recovery Clock

**Predicting societal recovery time after natural disasters using
machine learning**

The Recovery Clock is a predictive analytics project that estimates the
number of months required for societal recovery after a historical
natural disaster. It uses disaster-level data and a Random Forest
regression model to explore how demographic, economic, and
disaster-related characteristics relate to recorded recovery time.

> **Important:** This is an educational research prototype, not an
> operational disaster-response tool. Predictions should not be used as
> the sole basis for policy, funding, evacuation, or relief decisions.

## Project objectives

-   Predict recorded societal recovery time in months from selected
    disaster characteristics.
-   Compare a machine-learning model with a simple median baseline and
    other regression approaches.
-   Evaluate performance using held-out test data and cross-validation.
-   Explore which input features the trained model relies on most.

## Dataset

The project uses the **100 Disasters** dataset hosted on Dryad:

-   Dataset: https://datadryad.org/dataset/doi%3A10.5061/dryad.d7wm37qc0
-   Main workbook: `100_Disasters.xlsx`
-   Sheet used: `Data_030520`
-   Target column: `SocietySpeed - Months`

The working sheet contains 103 rows, with 100 non-missing target
records. The target has a median of 36 months and a mean of
approximately 52.13 months.

Download the dataset from Dryad and place the workbook where the
notebook or script expects it. The dataset is not included in this
repository by default; follow the dataset's licence and citation
requirements.

## Features used

### Numeric features

-   `Year`
-   `Population`
-   `GDP per Capita`
-   `Economic Loss`
-   `Fatalities`
-   `Displaced`
-   `Affected Population`
-   `Proportion Population Affected`

### Categorical features

-   `Disaster Type`
-   `Disaster Subtype`
-   `Scale`
-   `Income Group`
-   `Preparedness`
-   `Public Participation`
-   `Decision Making`

Outcome-related or leakage-prone columns are excluded from the predictor
set.

## Methodology

1.  Load the disaster dataset and select the target and approved
    predictor columns.
2.  Split the data into training and test sets (80/20,
    `random_state=42`).
3.  Preprocess numeric features using median imputation and scaling.
4.  Preprocess categorical features using most-frequent imputation and
    one-hot encoding.
5.  Fit the preprocessing and model steps in a scikit-learn pipeline.
6.  Compare the model against a training-set median baseline and
    alternative regression approaches.
7.  Evaluate predictions using MAE, RMSE, and R². Use cross-validation
    to examine variability.
8.  Calculate permutation feature importance on held-out data to inspect
    model reliance on features.

### Main model

The reported Random Forest configuration is:

``` python
RandomForestRegressor(
    n_estimators=300,
    max_depth=4,
    min_samples_leaf=3,
    random_state=42
)
```

The Random Forest is trained on the original target scale in the main
reported model. A log-target variant was also evaluated for comparison.

## Reported results

On the 20-record held-out test set:

  Model                                  MAE (months)   RMSE (months)       R²
  ------------------------------------ -------------- --------------- --------
  Random Forest (original target)               20.44           27.28    0.570
  Median baseline                               29.25           45.16   -0.178
  Random Forest (log-target variant)            19.04           29.06    0.510

The original-target Random Forest reduced test MAE by approximately
**30.1%** relative to the median baseline.

Cross-validation mean MAE was approximately 31.22 months for the
original-target Random Forest, compared with 36.88 months for the median
baseline. The small dataset and limited test set mean results can vary
substantially with different splits. Treat the figures as preliminary
rather than evidence of general performance on future disasters.

### Feature importance

Permutation importance ranked `Fatalities` as the feature with the
largest observed increase in MAE when shuffled, followed by
`Disaster Subtype` and `Disaster Type`.

Permutation importance measures how much this fitted model relies on a
feature for its predictions. It **does not demonstrate causation**.
Negative or small importance values can occur because of sampling
variability, correlated features, and the small evaluation set.

## Installation

Use Python 3.10 or later where possible.

1.  Clone or download this project.
2.  Create and activate a virtual environment (recommended).
3.  Install dependencies:

``` bash
pip install -r requirements.txt
```

Example virtual environment commands:

**macOS / Linux**

``` bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**Windows**

``` powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Running the project

Run the notebook or Python script containing the data-loading,
preprocessing, training, evaluation, and prediction workflow in your
project folder. Ensure the dataset path in that workflow points to the
downloaded `100_Disasters.xlsx` file.

This README describes the implemented workflow and reported metrics;
exact run commands depend on the notebook/script filename included in
your project directory.

## Dependencies

See [`requirements.txt`](requirements.txt). The project uses:

-   NumPy and pandas for data handling
-   scikit-learn for preprocessing, regression, validation, and metrics
-   Matplotlib and Seaborn for visualisation
-   openpyxl for reading Excel workbooks
-   joblib for saving/loading fitted Python objects when used by the
    workflow

## Limitations

-   Only around 100 labelled disaster records are available.
-   The held-out test set contains 20 records, so each observation can
    noticeably affect metrics.
-   Historical disaster recovery outcomes may not generalise to
    different regions, disaster types, or future events.
-   Missing data, inconsistent reporting, and differences in how
    recovery is defined may affect model quality.
-   Feature importance is descriptive of model behaviour, not a causal
    explanation.
-   Estimates should be presented with uncertainty and expert review in
    any real-world setting.

## Future improvements

-   Validate on more historical disaster records and, where possible, an
    independent external dataset.
-   Use repeated cross-validation or bootstrap intervals to better
    communicate uncertainty.
-   Investigate time-based or region-based validation to test
    generalisation.
-   Compare prediction intervals and robust regression methods.
-   Improve documentation of data provenance, target definitions, and
    missing-value handling.
-   Build a user interface only after the model has been more thoroughly
    validated.

## Dataset citation

Please cite the Dryad dataset and its associated research publication
according to the citation information provided on the [Dryad
record](https://datadryad.org/dataset/doi%3A10.5061/dryad.d7wm37qc0).

## Disclaimer

The Recovery Clock is an academic predictive analytics project. It
provides exploratory estimates from historical data and is not a
substitute for local expertise, validated impact assessments, or
official disaster-management guidance.
