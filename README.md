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
<img src="images/price_vs_carat.png" alt="Price vs Carat Plot" style="width:50%;">

Carat weight was the strongest predictor in our analysis. Our EDA revealed an exponential relationship between weight and price. After applying a log-transformation, we found that even small proportional increases in mass lead to substantial, statistically significant price increases ($t = 160.8$), confirming that valuation models must account for this non-linear growth.

### 2. The Width Anomaly: Revealing Hidden Preferences
<img src="images/price_vs_width.png" alt="Price vs Width Plot" style="width:50%;">

While the scatterplot above shows that price increases with width (because wider diamonds are usually heavier), our multivariate model revealed the opposite truth. Once we controlled for **Carat** and **Length**, the coefficient for **Width** became significantly negative ($\beta \approx -3048$). This indicates that for two diamonds of the *exact same weight*, the market penalizes the wider (squatter) diamond and pays a premium for the longer (elongated) one. This finding aligns with consumer psychology regarding "optical size"—elongated shapes are perceived as larger than rounder shapes of the same mass.

### 3. Homoscedasticity Correction
<img src="images/residuals_comparison.png" alt="Residuals Comparison" style="width:80%;">

The chart on the left shows the initial model's fanning residuals (heteroscedasticity), where prediction error increased with price. The chart on the right demonstrates how our **Weighted Least Squares (WLS)** approach stabilized the variance. By assigning lower weights to high-variance observations, we satisfied regression assumptions and achieved more precise coefficient estimates.

## Technologies Used

* **Language:** R (Statistical Computing)
* **IDE:** RStudio
* **Libraries:** `ggplot2` (Visualization), `MASS` (Box-Cox), `mgcv` (GAM), `caret` (Cross-Validation).
