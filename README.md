# House Price Prediction with Explainable AI

A machine learning project for predicting house prices using the Kaggle **House Prices: Advanced Regression Techniques** dataset. The project compares Ridge Regression, Lasso Regression, Random Forest, and XGBoost, then uses SHAP to explain the XGBoost predictions.

## Project Overview

This project demonstrates an end-to-end regression workflow:

- Exploratory Data Analysis (EDA)
- Domain-aware missing-value handling
- Feature engineering
- Skewness correction
- One-hot encoding
- Feature scaling for linear models
- Multiple regression models
- Cross-validation and early stopping
- RMSE-based model evaluation
- Prediction diagnostics
- SHAP-based explainable AI

The target variable is `SalePrice`, evaluated primarily as `log1p(SalePrice)`, which matches the metric used by the Kaggle competition.

## Dataset

**Dataset:** House Prices - Advanced Regression Techniques  
**Source:** Kaggle  
**Location:** Ames, Iowa  
**Training samples:** 1,460 houses  
**Features:** 79 explanatory variables  
**Target:** `SalePrice`

The dataset contains numerical and categorical information about residential properties, including quality, size, location, basement, garage, and other housing characteristics.

## Machine Learning Pipeline

```text
Raw Housing Data
       │
       ▼
Exploratory Data Analysis
       │
       ▼
Train / Test Split (80 / 20)
       │
       ▼
Domain-Aware Missing Value Handling
       │
       ▼
Feature Engineering
  ├── TotalSF
  └── HouseAge
       │
       ▼
Skewness Correction
       │
       ▼
One-Hot Encoding
       │
       ▼
Feature Scaling
       │
       ├───────────────┬────────────────┐
       ▼               ▼                ▼
     Ridge           Lasso        Random Forest
       │               │                │
       └───────────────┴────────┬───────┘
                                ▼
                             XGBoost
                                │
                                ▼
                         Model Evaluation
                                │
                                ▼
                          SHAP Explainability
```

## Models

### 1. Ridge Regression
Regularized linear regression using `glmnet`. The regularization parameter is selected using 10-fold cross-validation.

### 2. Lasso Regression
L1-regularized regression using `glmnet`, also using 10-fold cross-validation for lambda selection.

### 3. Random Forest
An ensemble tree-based regression model using 500 trees.

### 4. XGBoost
Gradient-boosted decision trees with:

- Learning rate: `0.03`
- Maximum depth: `3`
- Subsampling: `0.7`
- Column subsampling: `0.7`
- Up to 3,000 boosting rounds
- 5-fold cross-validation
- Early stopping

## Feature Engineering

Two additional features are created:

- **TotalSF** = Total Basement Area + 1st Floor Area + 2nd Floor Area
- **HouseAge** = Year Sold − Year Built

The preprocessing also treats certain missing categorical values as meaningful values such as `"None"` when the missing value represents the absence of a house feature.

## Data Leakage Prevention

The preprocessing pipeline is designed to reduce data leakage:

- The train/test split is performed before statistical preprocessing.
- Training-set medians and modes are used for imputation.
- Training-set skewness determines which numerical features are log-transformed.
- Training-set means and standard deviations are used for scaling.
- Model selection parameters are determined through cross-validation.

## Evaluation

Models are compared using:

- **RMSE on `log1p(SalePrice)`**
- **RMSE in dollar price units**

Lower RMSE indicates smaller prediction error.

The notebook also generates:

- Model comparison plots
- Actual vs. predicted plots
- Residual distributions
- Residual vs. predicted plots

## Explainable AI with SHAP

SHAP is used to investigate how features contribute to XGBoost predictions.

The project includes:

- Global feature-importance plots
- SHAP beeswarm plots
- SHAP dependence plots
- Local waterfall explanations

This makes the model easier to interpret beyond simply reporting prediction accuracy.

## Technologies

- R
- tidyverse
- glmnet
- randomForest
- xgboost
- e1071
- shapviz
- ggplot2
- Kaggle

## Project Structure

```text
house-price-prediction/
│
├── house_price_prediction.Rmd
├── house_price_prediction_report.html
├── README.md
└── data/
    └── train.csv
```

> `train.csv` is the Kaggle dataset and may be excluded from the repository because of dataset licensing/distribution considerations. Users can download it directly from Kaggle.

## How to Run

### 1. Install R

Install R from the official R Project website.

### 2. Install Required Packages

Run:

```r
pkgs <- c(
  "tidyverse",
  "glmnet",
  "randomForest",
  "xgboost",
  "e1071",
  "shapviz"
)

install.packages(pkgs)
```

If you want to render the R Markdown report, also install:

```r
install.packages("rmarkdown")
```

### 3. Download the Dataset

Download the **House Prices - Advanced Regression Techniques** training dataset from Kaggle.

Place `train.csv` in the location expected by the notebook or update the dataset path in the code.

### 4. Run the Notebook

Open the R Markdown/notebook in RStudio or another compatible R environment and run the analysis from top to bottom.

## Important Note

The repository should contain the cleaned and finalized R Markdown/report files rather than relying on the original Kaggle-specific paths. Update paths such as:

```text
/kaggle/input/competitions/house-prices-advanced-regression-techniques/train.csv
```

when running the project locally.

## Key Learning Outcomes

This project demonstrates practical skills in:

- Regression
- Ensemble learning
- Regularization
- Feature engineering
- Cross-validation
- Hyperparameter selection
- Data preprocessing
- Model evaluation
- Residual analysis
- Explainable AI
- R-based machine learning workflows

## License

Add an appropriate license for your source code. Also review the Kaggle dataset's terms before redistributing the dataset itself.
