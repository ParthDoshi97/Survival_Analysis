I can definitely help with that. Here's a well-structured README file for your GitHub repository, based on the R Markdown output.

-----

# Survival Analysis of Serum Free Light Chains (flchain)

This project performs a survival analysis on the `flchain` dataset from the `survival` R package to investigate the relationship between serum free light chain (FLC) levels and all-cause mortality. The analysis reproduces and extends the findings of the original study, examining how FLC levels, both individually and as a ratio, predict survival while adjusting for key demographic and clinical factors.

## Background

Serum free light chains (FLC), specifically the kappa and lambda components of immunoglobulins, have been identified as potential biomarkers for immune dysregulation and overall health. Previous research in a large cohort suggested that elevated FLC levels are a significant predictor of increased all-cause mortality. This analysis aims to validate and expand upon these findings using survival analysis techniques.

## Data Source

The `flchain` dataset is used for this analysis. It contains a stratified random sample of **7,874 adults aged 50 years or older** from Olmsted County. The dataset includes demographic information, FLC levels, clinical markers, and follow-up data for survival analysis.

**Key variables in the dataset include:**

  * `age`: Age in years.
  * `sex`: Gender (Female/Male).
  * `kappa`, `lambda`: Serum free light chain levels (mg/L).
  * `flc.grp`: A categorical grouping of FLC levels based on the original study.
  * `creatinine`: Serum creatinine levels (mg/dL).
  * `mgus`: A binary variable indicating a diagnosis of monoclonal gammopathy of undetermined significance (MGUS).
  * `futime`: Follow-up time in days until death or last contact.
  * `death`: The event status (1 = dead, 0 = alive at last contact).

## Analysis Plan

The primary objective of this analysis is to answer the following questions:

1.  Is there a statistically significant association between higher FLC levels and increased mortality risk?
2.  Do these associations remain significant after adjusting for potential confounders such as age, sex, kidney function (creatinine), MGUS status, and the year of the blood sample?
3.  Does the predictive effect on mortality differ between the FLC components (kappa vs. lambda) or their ratio?

The analysis proceeds through the following steps:

1.  **Data Preparation**: The raw data is cleaned, and a survival object is created using the `futime` and `death` variables. FLC ratios and log-transformed variables are calculated to handle skewed distributions.
2.  **Handling Missing Data**: Missing values in the `creatinine` variable are imputed using the **MICE** (Multivariate Imputation by Chained Equations) package to prevent bias.
3.  **Exploratory Data Analysis (EDA)**: Descriptive tables and plots (histograms, box plots) are generated to understand the distribution of key variables and their relationships.
4.  **Non-Parametric Analysis**: Kaplan-Meier survival curves are plotted to visualize survival probabilities across different FLC ratio groups, and a log-rank test is performed to assess significant differences.
5.  **Semi-Parametric Modeling**: A Cox proportional hazards (`coxph`) model is fitted to estimate the hazard ratios (HRs) for each predictor while controlling for all covariates.
6.  **Model Diagnostics**: The proportional hazards assumption is checked using Schoenfeld residuals.
7.  **Model Performance**: Time-dependent ROC curves are generated to evaluate the model's predictive discrimination at specific time points (12, 24, and 36 months).

## Methodology

The analysis uses R with several key packages: `tidyverse` for data manipulation, `survival` and `survminer` for survival analysis, `mice` for multiple imputation, and `gtsummary` and `gt` for creating publication-ready tables.

**Model Specification:**
The core Cox proportional hazards model is specified as:

```r
cox_model <- coxph(
  Surv(time_months, death_status) ~ flc_ratio_grp + age + sex + creatinine_log + mgus,
  data = dat_for_cox
)
```

This model estimates the hazard of death as a function of the FLC ratio group, age, sex, log-transformed creatinine, and MGUS status.

## Key Findings

  - **Kaplan-Meier Curves**: The survival curves show that individuals in the "Kappa-dominant" and "Lambda-dominant" groups have significantly lower survival probabilities over time compared to the "Normal ratio" group.
  - **Hazard Ratios (HR)**: The Cox model reveals that **age (HR = 1.11)**, **male sex (HR = 1.23)**, and **creatinine levels (HR = 2.44)** are all strong, independent predictors of mortality.
  - **Model Discrimination**: The time-dependent ROC analysis indicates that the model has **good discriminatory power** for predicting mortality, with AUC values of **0.78 at 12 months** and **0.80 at 36 months** .

## Survival Analysis Report
[View Full Analysis]()

## Reproducibility

To reproduce the analysis, run the provided R Markdown file. The code installs necessary packages, loads the data, and performs all the steps to generate the results and figures.

## References

1.  Dispenzieri A, et al. Use of monoclonal serum immunoglobulin free light chains to predict overall survival in the general population. Mayo Clinic Proceedings 2012.
2.  Kyle RA, et al. Prevalence of monoclonal gammopathy of undetermined significance. New England Journal of Medicine 2006.
3.  Therneau TM. A Package for Survival Analysis in R. (Comprehensive R Archive Network).
