<div align="center">

# ⏳ The Recovery Clock

### Predicting Societal Recovery Time After Natural Disasters

**A machine-learning project for estimating historical disaster recovery time from demographic, economic, and disaster-related data.**

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-Machine%20Learning-F7931E?logo=scikitlearn&logoColor=white)
![Status](https://img.shields.io/badge/Project-Academic%20Prototype-6C63FF)
![Dataset](https://img.shields.io/badge/Data-100%20Historical%20Disasters-2E8B57)

</div>

---

## 🌍 Overview

How long does a society take to recover after a natural disaster?

**The Recovery Clock** explores this question using historical disaster records and supervised machine learning. It trains a regression model to estimate recorded societal recovery time in **months**, based on characteristics such as disaster type, fatalities, population, economic loss, and preparedness-related factors.

The project compares a Random Forest model against a simple median baseline, evaluates prediction errors, and uses permutation importance to examine which features the fitted model relies on most.

> **Scope note:** This project predicts the dataset's recorded societal recovery-time target. It does not directly measure day-by-day mobility recovery after a particular disaster.

## 🎯 Objectives

- Predict societal recovery time from historical disaster characteristics.
- Build a reproducible preprocessing and regression pipeline.
- Compare model performance with a simple baseline and alternative regression approaches.
- Evaluate results using MAE, RMSE, R², and cross-validation.
- Explore model feature reliance without interpreting it as causation.

## 🧠 Methodology & Architecture

```text
Historical disaster dataset
          │
          ▼
Select target and predictor features
          │
          ▼
Train/test split (80/20)
          │
          ▼
Preprocessing pipeline
  ├── Numeric: median imputation + scaling
  └── Categorical: mode imputation + one-hot encoding
          │
          ▼
Random Forest Regressor
          │
          ▼
Evaluation and interpretation
  ├── MAE, RMSE, R²
  ├── Median-baseline comparison
  ├── Cross-validation
  └── Permutation feature importance
          │
          ▼
Estimated recovery time (months)
```

### Model configuration

The main model is a `RandomForestRegressor` configured as follows:

```python
RandomForestRegressor(
    n_estimators=300,
    max_depth=4,
    min_samples_leaf=3,
    random_state=42
)
```

The preprocessing steps and model are combined in a scikit-learn pipeline to help keep training and prediction consistent.

## 📊 Dataset

The project uses the **100 Disasters** dataset hosted on Dryad.

- **Source:** [Dryad — 100 Disasters dataset](https://datadryad.org/dataset/doi%3A10.5061/dryad.d7wm37qc0)
- **Workbook:** `100_Disasters.xlsx`
- **Sheet:** `Data_030520`
- **Prediction target:** `SocietySpeed - Months`
- **Records:** 100 records with a non-missing target

The recorded recovery-time target has a mean of approximately **52.13 months** and a median of **36 months**.

### Features

**Numeric**
- Year
- Population
- GDP per Capita
- Economic Loss
- Fatalities
- Displaced
- Affected Population
- Proportion Population Affected

**Categorical**
- Disaster Type
- Disaster Subtype
- Scale
- Income Group
- Preparedness
- Public Participation
- Decision Making

Outcome-related or leakage-prone fields are excluded from the predictor set.

> Download the workbook from Dryad and make sure the notebook or script points to its location. The dataset is not bundled with this README.

## ⚙️ Tech Stack

| Tool | Purpose |
|---|---|
| Python | Project implementation |
| pandas, NumPy | Data loading and preparation |
| scikit-learn | Preprocessing, regression, validation, and metrics |
| Matplotlib, Seaborn | Data visualisation |
| openpyxl | Reading Excel workbooks |
| joblib | Saving/loading fitted objects, when used |

## 🚀 Getting Started

### 1. Get the project files

Clone your repository or download the source files. Make sure the project notebook or Python script is included.

### 2. Set up a virtual environment (recommended)

**macOS / Linux**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows PowerShell**
```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Download `100_Disasters.xlsx` from the [Dryad dataset page](https://datadryad.org/dataset/doi%3A10.5061/dryad.d7wm37qc0) and place it in the location expected by your notebook or script.

### 5. Run the notebook

Open the project notebook in Jupyter or Google Colab and run the cells in order. If you are using Colab, upload the dataset or mount the storage location where it is saved, then update the dataset path if needed.

## 📈 Evaluation Results

The following results were recorded for the current 80/20 train/test split (`random_state=42`).

| Model | MAE (months) | RMSE (months) | R² |
|---|---:|---:|---:|
| Random Forest — original target | **20.44** | **27.28** | **0.570** |
| Median baseline | 29.25 | 45.16 | -0.178 |
| Random Forest — log-target variant | 19.04 | 29.06 | 0.510 |

The original-target Random Forest's test MAE was approximately **30.1% lower** than the median baseline's test MAE.

### Cross-validation snapshot

| Model | Mean MAE (months) | MAE standard deviation | Mean RMSE | Mean R² |
|---|---:|---:|---:|---:|
| Random Forest — original target | 31.22 | 8.93 | 43.10 | 0.18 |
| Median baseline | 36.88 | 14.29 | 52.94 | -0.14 |
| Linear Regression | 47.21 | 17.70 | 76.89 | -1.92 |
| Random Forest — log-target variant | 29.20 | 12.11 | 43.80 | 0.23 |

### Feature importance

Permutation importance identified **Fatalities** as the strongest model-reliance signal in the current evaluation, followed by **Disaster Subtype** and **Disaster Type**.

Permutation importance shows how model error changes when a feature is shuffled. It does **not** establish that a feature causes a longer or shorter recovery time.

<details>
<summary><strong>How should these results be interpreted?</strong></summary>

- The held-out test set contains only 20 records, so individual examples can substantially affect the metrics.
- Cross-validation results are more variable than the single test split suggests.
- The log-target model has a lower test MAE but a higher test RMSE and lower R² than the original-target Random Forest.
- Results are preliminary and should not be treated as proof of reliable performance on unseen future disasters.

</details>

## ⚠️ Limitations

- **Small dataset:** only about 100 labelled disaster records are available.
- **Limited test set:** the test split contains 20 records.
- **Generalisation:** patterns in historical records may not transfer to other regions, disaster types, or future events.
- **Data quality:** missing values and inconsistent reporting can affect model quality.
- **Interpretability:** feature importance describes model reliance, not causal relationships.
- **Decision support:** estimates are not a replacement for expert assessment or official disaster-management guidance.

## 🔭 Future Improvements

- Evaluate on a larger and more diverse collection of historical disasters.
- Test generalisation using region-based or time-based validation.
- Report uncertainty intervals alongside point predictions.
- Compare additional regression methods and robust modelling approaches.
- Improve documentation of data provenance and recovery-time definitions.
- Consider an interactive interface after further model validation.

## 📚 References & Data Citation

The dataset and its associated research publication should be cited using the details provided on the [official Dryad record](https://datadryad.org/dataset/doi%3A10.5061/dryad.d7wm37qc0).

## ⚖️ Disclaimer

The Recovery Clock is an **academic predictive analytics prototype**. Its outputs are exploratory estimates based on historical data and should not be used as the sole basis for emergency response, resource allocation, public policy, or other high-stakes decisions.

---

<div align="center">

**The Recovery Clock** · Predictive Analytics Project

</div>
