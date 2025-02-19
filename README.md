# Parkinson's Disease: Predicting and Correcting Bias in Motor Score Evaluation

## Project Overview
This project addresses the challenge of accurately predicting unbiased OFF motor scores in Parkinson's Disease (PD) patients. The OFF motor score serves as a critical proxy for tracking neurodegeneration progression but is often unavailable or affected by biases from medication timing, human subjectivity, and missing data.

Our solution achieved an RMSE of **54.8302**, which represents a **75.9%** improvement over the benchmark score (227.4629).

## Problem Statement
Parkinson's Disease affects over 10 million people worldwide, with numbers projected to double by 2030. Clinical assessment of PD relies on the MDS-UPDRS (Movement Disorder Society-Unified Parkinson's Disease Rating Scale), which measures motor function on a scale from 0 (normal) to 132 (severe).

Key challenges:
- Motor scores fluctuate based on medication timing ("ON" vs "OFF" state)
- Assessment contains inherent human subjectivity
- Clinical data often has significant missing values
- True OFF scores are rarely available due to patient discomfort

## Data Description
The dataset contains synthetic data modeled after real-world PD patient cohorts, with approximately:
- 10,000 patients across multiple visits (~100,000 entries)
- Each patient has irregular visit intervals
- Features include demographics, genetics, medication details, and motor scores
- Significant missing data patterns across multiple variables

Key features:
- **Patient Demographics**: Age, sex, age at diagnosis
- **Genetics**: Relevant gene markers (LRRK2+, GBA+, etc.)
- **Medication**: Levodopa Equivalent Daily Dosage (LEDD)
- **Assessment Timing**: Time since medication intake for ON/OFF evaluations
- **Motor Scores**: ON and OFF scores at each visit
- **Target**: True/unbiased OFF score

## Methodology

### 1. Data Preprocessing

#### Missing Value Analysis
- Performed comprehensive analysis of missing data patterns
- Identified non-random missingness (MAR patterns)
- Found strong correlations between missing values in related fields

#### Imputation Strategy
- Cross-dataset patient-specific information filling
- Group-based imputation based on correlation patterns:
  - MICE for highly correlated features (LEDD, ON scores, intake timing)
  - Random Forest imputation for moderately correlated features
  - KNN for patient-specific categorical features
- Validation via cross-validation with artificial masking

#### Data Transformation
- Created time-since-diagnosis variable
- Handled categorical variables with appropriate encoding
- Ensured proper scaling of numerical features

### 2. Feature Engineering

Our engineered features captured critical clinical aspects of PD:

#### Disease Progression Features
- Disease duration calculation
- ON-OFF difference metrics
- LEDD per year of disease normalization

#### Medication Timing Features
- Time ratios between assessments
- Time categories reflecting medication response phases
- Timing-based interaction terms

#### Patient-Specific Interactions
- Age-LEDD interactions
- Disease duration-symptom interactions
- Medication response patterns

#### Feature Selection
Applied mutual information regression to identify the most predictive features, reducing from 24 engineered features to 20 final features.

### 3. Model Development

#### Cross-Validation Strategy
- Implemented GroupShuffleSplit based on patient_id
- Created 5-fold validation ensuring no patient overlap
- Properly scaled and transformed features within each fold

#### Model Evaluation
Tested multiple regression approaches:
- Linear models (baseline)
- Tree-based models (Random Forest, Decision Tree)
- Gradient Boosting approaches (XGBoost, CatBoost, LightGBM)
- Deep Learning models (LSTM networks)
- Ensemble techniques (AdaBoost, stacking)

#### Hyperparameter Optimization
- Random search for CatBoost parameters
- Optimized for regularization, tree depth, and learning rate
- Neural architecture search for LSTM configurations

## Results

Our final model achieved an RMSE of **54.8302** on the test set, substantially outperforming the benchmark score of 227.4629.

Key insights:
1. OFF score prediction accuracy is heavily influenced by disease duration
2. Medication timing effects show non-linear relationships with motor scores
3. Patient-specific factors (age, genetics) significantly impact progression patterns
4. Time-based features provide crucial information for accurate prediction

## Feature Importance

The most predictive features for OFF score estimation were:
1. Disease duration and ON-OFF interaction
2. OFF score (when available)
3. Time since diagnosis
4. Disease duration
5. ON score
6. Age-LEDD interaction
7. LEDD (medication dosage)
8. Age

## Conclusions

This project demonstrates that:
1. Accurate prediction of unbiased OFF scores is possible with advanced modeling
2. Properly engineered features can capture complex PD progression patterns
3. Ensemble methods effectively handle the heterogeneity in PD patient data
4. Time-series aspects of medication response are critical for accurate assessment

The developed approach could significantly improve clinical assessment by providing estimated unbiased OFF scores even when direct measurement isn't feasible, potentially enhancing treatment decisions and clinical trial evaluations.

## Future Work

Potential improvements include:
- Incorporating additional biomarkers when available
- Developing patient-specific progression models
- Exploring more sophisticated temporal modeling approaches
- Extending the approach to predict individual MDS-UPDRS subscores
