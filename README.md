# DSA 210: Algal Growth

# Abstract

This project investigates algal growth patterns using two complementary datasets: (1) a controlled laboratory algae‐growth experiment and (2) the COIL 1999 environmental water-quality dataset. The goal is to apply core data-science techniques—including data cleaning, exploratory data analysis (EDA), outlier assessment, feature transformation, and hypothesis testing—to understand how environmental variables influence algal populations. The analysis reveals two contrasting systems: a laboratory dataset dominated by strong non-linear (unimodal) light–response behavior, and an environmental dataset with heavy skew, extreme nutrient variability, and moderate correlations among water-quality indicators. A Pearson hypothesis test confirms the absence of linear effects of light on population in Dataset 1, supporting the conclusion that the system is governed by non-linear ecological responses. The project demonstrates how data preprocessing and statistical testing shape the interpretability of biological data.

# Motivation

I selected algae growth datasets because my background and ongoing work in biotechnology involve studying algae as biological systems. Since I already conduct laboratory research on algal biology, this project allowed me to connect data-science methods with a domain I am personally involved in. Algae are sensitive to environmental factors such as light and nutrient availability, making them ideal for exploring how experimental variables shape biological responses. This project therefore served both as a technical exercise in EDA and hypothesis testing, and as a way to deepen my understanding of the environmental drivers influencing algae growth.

# Data Sources
Dataset 1 — Research on Algae Growth in the Laboratory
Source: Kaggle
Link: https://www.kaggle.com/datasets/rukenmissonnier/research-on-algae-growth-in-the-laboratory/data

Description:
A controlled experimental dataset with ~9,800 measurements of algal population under varying environmental conditions including Light, Nitrate, Iron, Phosphate, Temperature, pH, and CO₂. The dataset uses fixed treatment levels for each factor, producing a full-factorial design that enables analysis of multi-variable effects on growth.

Dataset 2 — COIL 1999 Competition Data
Source: UCI Machine Learning Repository
Link: https://archive.ics.uci.edu/dataset/118/coil+1999+competition+data

Description:
A field dataset containing water-quality measurements from European rivers and streams. Includes nutrient concentrations (NO₃, NH₄, Phosphate, Orthophosphate, Chloride), chlorophyll levels, algal group abundances (seven groups), and categorical descriptors (Season, Size, River_Size). Unlike Dataset 1, it represents natural environmental variability with strong skew, extreme values, and heterogeneous distributions.

# Methodology
1. Data Cleaning

Dataset 1
Checked for missing values (none found).
Verified numeric ranges for all variables.
Identified 169 rows with Population = 0 and retained them as valid biological responses.

Dataset 2
Converted numeric columns from object to float.
Standardized column names.
Verified no missing values post-conversion.
Kept categorical features (Season, Size, River_Size) as non-numeric.

2. Outlier Detection and Distribution Analysis

Dataset 1
Histograms and boxplots showed no IQR-defined outliers.
Population contained many zeros, which are biologically meaningful.

Dataset 2
Extreme right-skew and large outlier counts across nutrients and algal groups.
123 of 200 rows had at least one outlier.
Outliers retained as legitimate ecological extremes.

3. Log Transformation

Applied log1p to all heavily skewed features in Dataset 2:
Nutrients: Cl, NO₃, NH₄, Orthophosphate, Phosphate
Chlorophyll
All Algae_Groups (1–7)

Not transformed:
Max_pH
Min_O₂

Transformation greatly reduced skew and compressed extreme values.

4. Exploratory Data Analysis (EDA)

Dataset 1: Laboratory Experiment
Correlation matrix showed near-zero linear correlations.
Scatterplots revealed a clear unimodal (U-shaped) relationship between Light and Population—strong biologically, invisible to linear correlation.
No simple linear patterns detected across other environmental variables.

Dataset 2: COIL Environmental Dataset
Moderate nutrient intercorrelations (e.g., Cl–Orthophosphate ≈ 0.79 post-log).
Nutrient–chlorophyll correlations around 0.59.
Weak-to-moderate nutrient–algal group relationships.
Balanced categorical distributions.
Log-transformed correlation matrix provided much cleaner interpretation vs. raw correlations.

# Hypothesis Testing
Hypothesis (Dataset 1)

H₀: Light intensity and Algal Population have no statistically significant linear correlation.
H₁: Light intensity and Algal Population have a statistically significant linear correlation.

Method
Performed a Pearson correlation test to evaluate linear dependency between Light and Population.

Results
Pearson r: 0.0062
p-value: 0.5368
Degrees of Freedom: 9782

Interpretation
The correlation is effectively zero and statistically non-significant. Although scatterplots show a strong unimodal relationship (Population rises with Light until an optimum, then declines), Pearson only measures linearity and therefore fails to capture this curve.
We fail to reject H₀, confirming there is no linear relationship between Light and Population in this experimental dataset.

# Expected Findings / Outcomes
Dataset 1 exhibits strong non-linear biological behavior rather than linear correlations.
Dataset 2 behaves as expected for environmental water-quality data, with moderate nutrient–chlorophyll relationships and group-specific algal responses.
Outlier analysis and log transformation were essential to properly interpret Dataset 2.
Hypothesis testing validates the EDA conclusion: Dataset 1 contains no linear dependencies, despite clear ecological structure.

# Future Work
A natural next step is developing machine learning models to predict algal population or group abundances using environmental variables. This would involve:

Feature Engineering: Using the log-transformed nutrient variables, encoding categorical features (Season, Size, River_Size), and exploring interaction terms or polynomial features to capture non-linear ecological effects.

Model Selection: Comparing linear models (Ridge, Lasso), tree-based models (Random Forest, XGBoost), and non-linear models (k-NN, SVR, or neural networks) to identify which best captures the complex dynamics of algae growth.

Handling Non-Linearity: Since Dataset 1 exhibits unimodal light–response curves, models capable of learning non-linear relationships (Random Forest, Gradient Boosting, GAM-style methods) would likely outperform linear regression.

Cross-Validation & Evaluation: Using train/test splits and k-fold cross-validation, evaluating models with RMSE, MAE, and R² to ensure generalization.

Model Interpretation: Applying SHAP values or permutation importance to understand which environmental variables contribute most to predicted algal growth.

Generalization Across Datasets: Testing whether a model trained on the COIL dataset can generalize to laboratory-like conditions and vice versa, highlighting differences between controlled experiments and environmental systems.
