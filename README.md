# B105A-statistical-modelling
# Online Shopping Purchasing Intention Analysis

# Project Overview

This project analyses online shopping intention to pull out the factors associated with a customer's decision to make a purchase o

The project code was done in R and covers:

 Dataset exploration and descriptive statistics
 Missing-value and duplicate-value checking
 Data cleaning
 Analysis of numerical variables by purchase outcome
 Analysis of categorical variables by purchase outcome
 Correlation analysis
 Wilcoxon statistical tests
 Chi-square tests
 Logistic regression
 Odds ratio analysis
 Multicollinearity checking
 Logistic regression model diagnostics
 Full and reduced model comparison

The dependent target variable used to analysis in this project is revenue, which indicates whether a purchase was made  or not during a browsing session.

# Repository Structure

```text
Online-Shopping-Purchasing-Intention/
│
├── online_shoppers_analysis.R
├── online_shoppers_intention.csv
└── README.md
```

#Files

| File                            | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| `online_shoppers_analysis.R`    | Main R script containing the complete analysis               |
| `online_shoppers_intention.csv` | Dataset used for the analysis                                |
| `README.md`                     | Documentation explaining the project and how to reproduce it |

---

# Technologies and Packages

The analysis was performed using R

The following R packages are required:

```r
library(tidyverse)
library(corrplot)
library(car)
```

# Package purposes

tidyverse – Data manipulation, transformation and visualisation
corrplot– Visualisation of the correlation matrix
car – Variance Inflation Factor (VIF) analysis for multicollinearity

---

# Dataset

The project is using the `online_shoppers_intention.csv` dataset.

The dataset contains information regarding online shopping sessions, with variables related to:

 Administrative page visits
 Administrative page duration
 Informational page visits
 Informational page duration
 Product-related page visits
 Product-related page duration
 Bounce rate
 Exit rate
 Page value
 Special day
 Month
 Operating system
 Browser
 Region
 Traffic type
 Visitor type
 Weekend
 Revenue

As mentioned before the `Revenue` variable represents the purchase outcome:

`FALSE` = No purchase
 `TRUE` = Purchase

The R script converts this variable into labelled categories for the modelling stage:

No Purchase
Purchase

# Analysis Process

# 1. Dataset Exploration

The early stages asses the basic structure and characteristics of the dataset.

The analysis includes:

```r
dim(data)
nrow(data)
ncol(data)
names(data)
str(data)
head(data)
tail(data)
```

Missing values are also checked using:

```r
colSums(is.na(data))
```

This provides an initial understanding of the dataset before further analysis.

# 2. Revenue Distribution

The distribution of purchase and non-purchase sessions is calculated using counts and percentages.

This helps establish the proportion of sessions that resulted in a purchase compared with those that did not.

# 3. Duplicate Detection and Data Cleaning

Duplicate observations are identified using:

```r
sum(duplicated(data))
```

The analysis also examines duplicate patterns and their frequency.

Exact duplicate observations are subsequently removed:

```r
data_clean <- data[!duplicated(data), ]
```

The cleaned dataset is then checked again to confirm that duplicate observations have been removed.

# 4. Numerical Variable Analysis

Numerical variables are analysed according to purchase outcome.

The analysis calculates means and medians for numerous variables including:

ProductRelated
 ProductRelated_Duration
 BounceRates
 ExitRates
 Administrative
 Administrative_Duration
 Informational
 Informational_Duration
 PageValues

Boxplots are created to compare the distributions between:

No Purchase
Purchase

For example, the project examines product-related page views and duration by purchase outcome.

# 5. Page Value Analysis

`PageValues` receives additional analysis because it can be used to examine the relationship between page value and purchasing behaviour.

The analysis calculates:

Mean
Median
First quartile
Third quartile
Maximum
Number of positive page values
Percentage of sessions with positive page values

Sessions are also grouped into:

-Positive Page Value
-Zero Page Value

The purchase rate for these groups is then calculated.

# 6. Correlation Analysis

A Spearman correlation matrix is calculated for the numerical variables:

```r
cor(
  numeric_vars,
  use = "complete.obs",
  method = "spearman"
)
```

The correlation matrix is visualised using `corrplot`.

This analysis examines relationships between variables such as:

Administrative activity
Informational activity
Product-related activity
Bounce rate
Exit rate
Page value
Special day

# 7. Categorical Variable Analysis

Categorical variables are compared with purchase outcomes.

The analysis investigates:

Visitor Type
Weekend
Month
Special Day
Operating System
Browser
Region
Traffic Type

Percentages are calculated for each category and visualised using bar charts.

These visualisations help identify differences in purchase outcomes across different categories.

# Statistical Analysis

# Wilcoxon Tests

Wilcoxon tests are used to examine whether numerical variables differ between purchase and non-purchase sessions.

The variables tested include:

 Administrative
 Administrative Duration
 Informational
 Informational Duration
 Product Related
 Product Related Duration
 Bounce Rates
 Exit Rates
 Page Values

The results are classified according to a significance level of:

```text
α = 0.05
```

Results with:

```text
p < 0.05
```

are classified as statistically significant in the analysis.

---

# Chi-Square Tests

Chi-square tests are performed to investigate relationships between categorical variables and the purchase outcome.

The variables tested are:

 Visitor Type
 Weekend
 Month
 Special Day
 Operating System
 Browser
 Region
 Traffic Type

For variables where expected frequencies may be too low for the standard chi-square approximation, simulated p-values are calculated using:

```r
simulate.p.value = TRUE
B = 10000
```

This provides an alternative approach when chi-square assumptions are problematic.


# Logistic Regression

# Logistic Regression Model

A logistic regression model is used to investigate the relationship between browsing/session characteristics and the probability of a purchase.

The model includes:

```text
Administrative
Administrative_Duration
Informational
Informational_Duration
ProductRelated
ProductRelated_Duration
BounceRates
ExitRates
PageValues
SpecialDay
Month
Browser
Region
TrafficType
VisitorType
Weekend
```

The model uses:

```r
family = binomial
```

because `Revenue` represents a binary purchase outcome.

# Odds Ratios

Odds ratios are calculated from the logistic regression coefficients:

```r
Odds_Ratio = exp(Estimate)
```

The analysis also calculates 95% confidence intervals for the odds ratios.

The results are used to identify statistically significant predictors and understand their association with purchasing behaviour.

An odds ratio:

 Greater than 1 indicates higher estimated odds of purchase
 Less than 1 indicates lower estimated odds of purchase
 Equal to 1 indicates no change in the estimated odds

These interpretations are considered alongside the statistical significance and confidence intervals.

# Model Diagnostics

Several checks are performed to assess the logistic regression model.

# Multicollinearity

Variance Inflation Factors (VIF) are calculated using the `car` package:

```r
vif_values <- vif(logistic_model)
```

This is used to identify potential multicollinearity among predictors.

# Predictor Distributions

Density plots are generated for continuous predictors to examine their distributions according to purchase outcome.

# Singular Coefficients

The model is also checked for singular coefficients using:

```r
coef(logistic_model)[is.na(coef(logistic_model))]
```

# Model Fit

The project evaluates model fit using:

 Null deviance
 Residual deviance
 Likelihood-ratio chi-square
 Likelihood-ratio p-value
 McFadden pseudo R-squared
 AIC

These measures provide information about how well the logistic regression model represents the observed purchase outcomes.

# Full Model vs Reduced Model

A reduced logistic regression model is also created using a smaller set of predictors:

```text
PageValues
ExitRates
ProductRelated_Duration
Month
VisitorType
```

The reduced model is compared with the full model using:

 Likelihood-ratio testing
 AIC comparison

This analysis examines whether the additional complexity of the full model provides a meaningful improvement in model fit.

#  Overall Analysis

The project combines three main statistical approaches:

| Analysis            | Purpose                                                                     |
| ------------------- | --------------------------------------------------------------------------- |
| Wilcoxon Test       | Examine differences in numerical variables between purchase outcomes        |
| Chi-Square Test     | Examine relationships between categorical variables and purchase outcome    |
| Logistic Regression | Model the relationship between multiple predictors and purchase probability |

The results from these analyses are combined into an overall results table within the R script.

# How to Run the Project

# Step 1: Install R

Install R from the official R Project website.

# Step 2: Install RStudio

RStudio can be used as the development environment for running the analysis.

# Step 3: Download or Clone the Repository

Make sure both files are stored in the same folder:

```text
online_shoppers_analysis.R
online_shoppers_intention.csv
```

# Step 4: Set the Working Directory

Open the project folder in RStudio.

The script expects the CSV file to be located in the same directory as the R script.

The dataset is loaded using:

```r
data <- read.csv("online_shoppers_intention.csv")
```

# Step 5: Install Required Packages

If the packages are not already installed:

```r
install.packages("tidyverse")
install.packages("corrplot")
install.packages("car")
```

Then load them:

```r
library(tidyverse)
library(corrplot)
library(car)
```

# Step 6: Run the R Script

Open:

```text
online_shoppers_analysis.R
```

in RStudio and run the script sequentially.

# Reproducibility

For the analysis to run correctly, the following files should remain in the same repository:

```text
online_shoppers_analysis.R
online_shoppers_intention.csv
```

The R script reads the CSV using a relative file path rather than requiring a manually selected file.

# Project Scope

This project focuses on statistical analysis of online shopping session behaviour and the relationship between session characteristics and purchasing outcomes.

The analysis progresses from:

```text
Data Import
     ↓
Data Exploration
     ↓
Data Cleaning
     ↓
Descriptive Analysis
     ↓
Visualisation
     ↓
Correlation Analysis
     ↓
Wilcoxon Tests
     ↓
Chi-Square Tests
     ↓
Logistic Regression
     ↓
Odds Ratios & Confidence Intervals
     ↓
Model Diagnostics
     ↓
Full vs Reduced Model Comparison
```

# Author

Roopireddy Snisha Reddy

University statistical modelling Project

# Notes

This repository contains the R code used for the analysis. The numerical results and statistical conclusions should be interpreted in the context of the dataset and the assumptions of each statistical method.

The R script should be run sequentially because later analysis sections depend on objects created during earlier sections.
