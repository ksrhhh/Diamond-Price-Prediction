# Diamond Price Prediction Model

This model was conducted as the final capstone group project for STA302: Data Analysis I in Summer 2025. The original repo is hosted here: https://github.com/YolandaThant/sta302project.


## Project Summary

The Diamond Price Prediction Model is a statistical analysis project that transforms raw dataset attributes into market valuation insights. It addresses the subjectivity in diamond pricing by rigorously quantifying the relationship between the "4Cs" (Carat, Cut, Color, Clarity) and market value.

The analysis moves beyond simple correlation, utilizing complex transformations and weighted models to correct for heteroscedasticity and non-linearity often found in financial data. The final model supports buyers and sellers in determining fair market prices and identifying undervalued assets based on specific physical profiles.

### Structure of the Analysis

The project follows a rigorous data analysis pipeline, focused in statistical validity and interpretability.

1. **Exploratory Data Analysis (EDA):** Uncovering distributions and correlations (e.g., the exponential relationship of Carat).
2. **Model Fitting & Diagnostics:** Moving from standard Multiple Linear Regression (MLR) to Generalized Additive Models (GAM) with Weighted Least Squares (WLS).
3. **Validation:** Using K-Fold Cross-Validation to ensure the model generalizes to unseen market data.

## Methodology and Technical Implementation

### Data Source

**Dataset:** The analysis utilizes the `diamonds` dataset from the `ggplot2` R package (sourced from the Loose Diamond Search Engine), containing 53,940 observations.
**Features:** 10 variables including Price (Target), Carat, Cut, Color, Clarity, and dimensions (x, y, z).

### Model Evolution

The project compared four distinct models to achieve the highest predictive accuracy while satisfying regression assumptions (Linearity, Homoscedasticity, Normality).

#### 1. Transformations & Feature Engineering

* **Box-Cox Transformation:** Applied to the response variable (`Price`) with  to correct severe right-skewness.
* **Log Transformations:** Applied to `Carat` to linearize its exponential relationship with price.
* **Log1p Transformation:** Applied to `Width` to handle values near zero and stabilize variance.

#### 2. Advanced Modeling (WLS-GAM)

To address non-constant error variance (heteroscedasticity) and remaining non-linearity, the final model utilized:

* **Generalized Additive Model (GAM):** To smooth predictor effects.
* **Weighted Least Squares (WLS):** To assign lower weights to observations with higher variance, resulting in more precise coefficient estimates.

### Model Validation Results

We validated the models using an 80/20 Train-Test split and K-Fold Cross-Validation. The Final Transformed Model (Model 4) significantly outperformed standard MLR approaches.

| Model | Adjusted  | AIC | Cross-Validation MSE |
| --- | --- | --- | --- |
| Model 1 (Full MLR) | 0.9198 | 911,551 | 1,279,068 |
| **Model 4 (Final Transformed)** | **0.9789** | **788,582** | **131,312** |

*The final model explains approximately 97.9% of the price variation with a significantly lower Mean Squared Error (MSE).*

## Key Insights & Visualizations

### 1. The Dominance of Carat

Carat weight was the strongest predictor. After log-transformation, we found that a 1% increase in mass leads to a substantial, statistically significant increase in price, confirming an exponential valuation model.

### 2. The Width Anomaly

While `Length` had a positive correlation with price, `Width` showed a significant negative coefficient. This suggests a market preference for elongated shapes (ovals/rectangles) over proportionate ones, likely due to the psychological perception that elongated shapes look "larger."

### 3. Homoscedasticity Correction

The chart on the left shows fanning residuals (heteroscedasticity). The chart on the right demonstrates how the Weighted Least Squares (WLS) approach stabilized the variance, satisfying regression assumptions.

## Technologies Used

* **Language:** R (Statistical Computing)
* **IDE:** RStudio
* **Libraries:** `ggplot2` (Visualization), `MASS` (Box-Cox), `mgcv` (GAM), `caret` (Cross-Validation).
