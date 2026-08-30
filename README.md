# toyota-corolla-regression
## Overview

This project applies multiple linear regression in R to examine the relationship between Toyota Corolla vehicle characteristics and observed vehicle prices.

The analysis demonstrates an end-to-end statistical workflow including exploratory analysis, regression modeling, predictor evaluation, multicollinearity diagnostics, train-validation splitting, out-of-sample model comparison, residual diagnostics, and prediction intervals.

The analysis was executed in Posit Cloud using **R 4.6.1** on **Ubuntu 24.04.4 LTS**.

---

## Analytical Question

Which observed Toyota Corolla characteristics are associated with vehicle price, and can those characteristics be used to construct a regression model that provides useful predictions for previously unseen vehicles?

This project evaluates association and prediction. It does not establish causal relationships.

---

## Dataset

The analysis dataset contains:

- **1,381 observations**
- **7 variables**

Variables analyzed:

| Variable | Description |
|---|---|
| Price | Observed vehicle price |
| Age | Vehicle age measure contained in the source dataset |
| KM | Vehicle mileage |
| FuelType | Diesel or Petrol |
| Horsepower | Vehicle horsepower |
| MetColor | Metallic color indicator |
| Automatic | Automatic transmission indicator |

The original source and redistribution license for the dataset are being documented separately before the source data are published in this repository.

---

## Analytical Workflow

The analysis follows this sequence:

1. Data ingestion and validation
2. Exploratory data analysis
3. Correlation analysis
4. Simple linear regression
5. Multiple linear regression
6. Standardized coefficient analysis
7. Variance Inflation Factor analysis
8. Expanded modeling with categorical predictors
9. 70/30 training-validation split
10. Out-of-sample model comparison
11. Residual diagnostics
12. Final model estimation
13. Prediction with 95% prediction intervals

---

## Exploratory Findings

Vehicle Age showed the strongest observed linear relationship with Price.

The correlation between Age and Price was:

**r = -0.872**

Other observed correlations with Price included:

| Variable | Correlation with Price |
|---|---:|
| Age | -0.872 |
| KM | -0.548 |
| Horsepower | +0.319 |

These results indicate that older and higher-mileage vehicles tended to have lower observed prices, while higher-horsepower vehicles tended to have higher observed prices within this dataset.

---

## Simple Regression

A simple linear regression using Age as the only predictor produced:

- **R² = 0.7596**
- **Adjusted R² = 0.7594**
- Residual standard error = **1,600**

The estimated Age coefficient was approximately:

**-162.36**

This indicates a strong negative association between the dataset's Age measure and observed vehicle Price.

---

## Multiple Regression

A multiple regression model using:

- Age
- KM
- Horsepower

improved explanatory performance.

### Numeric Model Performance

- **R² = 0.8056**
- **Adjusted R² = 0.8052**
- Residual standard error = **1,440**

All three predictors were statistically significant in this model.

### Standardized Coefficients

| Predictor | Standardized coefficient |
|---|---:|
| Age | -0.795 |
| KM | -0.107 |
| Horsepower | +0.170 |

Age had the largest standardized coefficient magnitude, indicating that it was the strongest predictor among these three variables within the fitted model.

---

## Multicollinearity Assessment

Variance Inflation Factors for the numeric regression model ranged from approximately:

**1.11 to 1.43**

The expanded model produced VIF values of approximately:

**1.01 to 1.89**

These values did not indicate severe multicollinearity among the included predictors.

---

## Expanded Regression Model

The expanded model included:

- Age
- KM
- Horsepower
- Fuel Type
- Metallic Color
- Automatic Transmission

The expanded model produced:

- **R² = 0.8234**
- **Adjusted R² = 0.8227**
- Residual standard error = **1,374**

Metallic color was not statistically significant in this model:

**p = 0.605**

The remaining modeled predictors were statistically significant.

Because metallic color contributed little explanatory value, a reduced model excluding that variable was evaluated using held-out validation data.

---

## Train-Validation Methodology

The dataset was randomly divided using a reproducible random seed into:

- **966 training observations**
- **415 validation observations**

Approximately 70% of the observations were used for model development and 30% were retained for out-of-sample validation.

The validation observations were not used to estimate the training-model coefficients.

---

## Out-of-Sample Model Validation

Two model specifications were evaluated.

| Model | RMSE | MAE | Validation R² |
|---|---:|---:|---:|
| Full Model | 1430.039 | 1053.714 | 0.823545 |
| Reduced Model | **1429.975** | **1053.636** | **0.823560** |

Predictive performance was essentially identical.

Because removal of metallic color reduced model complexity without materially reducing predictive performance, the reduced model was selected on the basis of parsimony.

---

## Final Regression Model

The selected model uses:

**Price ~ Age + KM + Horsepower + FuelType + Automatic**

The final model fitted to the complete analysis dataset produced:

- **R² = 0.8234**
- **Adjusted R² = 0.8228**
- Residual standard error = **1,374**

### Final Coefficients

| Predictor | Estimate |
|---|---:|
| Intercept | 15,671.92 |
| Age | -137.58 |
| KM | -0.01655 |
| Horsepower | +53.20 |
| Petrol relative to Diesel | -1,791.54 |
| Automatic relative to non-automatic | +906.63 |

These coefficients represent conditional associations within this dataset.

For example, holding the other included predictors constant, an additional 1,000 KM is associated with approximately **16.55 lower units of Price**.

No causal interpretation is implied.

---

## Model Diagnostics

Residual and Normal Q-Q plots were reviewed for the fitted regression models.

The diagnostic plots show that the linear model captures a substantial amount of the observed variation, but the model does not satisfy all ideal regression assumptions perfectly.

The residual plots show systematic structure rather than a completely random distribution around zero, particularly across portions of the fitted-price range.

The Normal Q-Q plots follow the reference line reasonably closely through much of the center of the distribution but show noticeable departures in both tails.

These findings suggest:

- possible nonlinear structure
- influential or extreme observations
- departures from residual normality in the tails
- potential opportunities for future model improvement

The model should therefore be interpreted as a useful predictive and explanatory model rather than a perfect representation of vehicle pricing behavior.

---

## Prediction Example

The final model was applied to five additional vehicle records.

For one example vehicle, the model produced:

- Predicted Price: **20,261.93**
- 95% prediction interval: **17,524.14 to 22,999.73**

Prediction intervals were used rather than reporting point estimates alone in order to communicate uncertainty around individual predictions.

---

## Key Findings

The analysis supports several conclusions:

**Age was the dominant predictor among the continuous variables examined.**

Mileage provided additional negative predictive information, while horsepower was positively associated with Price after controlling for the other modeled characteristics.

Adding categorical variables improved the regression model, although metallic color contributed little additional explanatory information.

Removing metallic color produced essentially unchanged held-out validation performance, supporting selection of the reduced model on parsimony grounds.

The final model explained approximately **82% of observed Price variation**, while diagnostic analysis indicated that additional nonlinear structure or other predictors could potentially improve performance.

---

## Limitations

This analysis has several important limitations.

The dataset is observational, so regression coefficients represent associations rather than causal effects.

The model assumes linear relationships between continuous predictors and Price.

Residual diagnostics indicate departures from ideal linear-model assumptions, particularly in the tails and across portions of the fitted-value range.

Additional vehicle characteristics not contained in the analyzed dataset may influence observed prices.

Model performance has been evaluated using a single reproducible train-validation split rather than repeated cross-validation.

---

## Future Improvements

Possible extensions include:

- nonlinear Age effects
- polynomial regression
- interaction terms
- log transformations
- influence and leverage diagnostics
- Cook's distance analysis
- repeated cross-validation
- regularized regression
- comparison with tree-based regression methods
- additional vehicle characteristics
- formal model-performance comparison

These extensions would provide an opportunity to evaluate whether more flexible modeling approaches improve predictive performance while preserving interpretability.

---

## Reproducibility

The analysis was executed using:

- R 4.6.1
- Ubuntu 24.04.4 LTS
- ggplot2 4.0.3
- lm.beta 1.7-3
- car 3.1-5
- carData 3.0-6

Complete environment information is available in:

`environment/session-info.txt`

The primary analysis script is:

`toyota-corolla-regression-analysis.R`

---

## Skills Demonstrated

This project demonstrates applied experience with:

- R
- exploratory data analysis
- statistical modeling
- correlation analysis
- simple linear regression
- multiple linear regression
- categorical predictors
- standardized regression coefficients
- model diagnostics
- Variance Inflation Factors
- train-validation methodology
- out-of-sample evaluation
- RMSE and MAE
- predictive modeling
- prediction intervals
- statistical interpretation
- model limitations
- evidence-based model selection

---

## Author

**Stanley Guinn, M.S.**

Data & Decision Analytics  
Statistical Analysis  
Enterprise Data Management  
Decision Support  
AI-Augmented Analytics
