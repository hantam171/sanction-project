# Sanctions and Trade: Direct and Spillover Effects

## Research Question
This project explores how international sanctions influence trade, both for targeted countries and their land neighbors.  
The central questions are:

1. Do sanctions reduce trade (imports and exports) of the targeted countries?  
2. Do neighboring countries experience positive spillover effects by capturing trade flows diverted from sanctioned nations?

The working hypothesis is that sanctions impose negative trade shocks on targeted economies, while neighboring countries may partially benefit through redirected trade and substitution effects.

---

## Data and Cleaning Process

- **Data Source:** Multiple international datasets combining trade, macroeconomic, and political indicators (1975–2019).  
- **Unit of Analysis:** Country–year observations.

### Key Variables
| Variable | Description |
|-----------|--------------|
| `sanction` | 1 if the country is under any sanction (TRADE, ARMS, MILITARY, FINANCIAL, or TRAVEL) |
| `neighbor` | 1 if at least one of its land neighbors is under sanction |
| `log_import` | Log of total imports from the rest of the world |
| `log_export` | Log of total exports to the rest of the world |
| `log_gdp` | Log of GDP |
| `log_pop` | Log of population |
| `polity` | Political regime index |

### Cleaning Steps
1. **Merge and harmonize** country identifiers across datasets.  
2. **Filter** observations from **1975–2019** to ensure data consistency.  
3. **Log-transform** import, export, GDP, and population to handle scale differences and heteroskedasticity.  
4. **Generate dummy variables** for `sanction` and `neighbor`.  
5. **Remove missing or inconsistent entries** to ensure balanced panel data.  
6. Validate that each country has both sanctioned and non-sanctioned periods to enable within-country comparisons.

---

## Research Method

### 1. Baseline OLS Regression
A linear model is estimated to identify the direct and indirect (neighbor) effects of sanctions on trade:

\[
\text{log\_trade}_{it} = \alpha + \beta_1 \text{sanction}_{it} + \beta_2 \text{neighbor}_{it}
+ \beta_3 \text{log\_gdp}_{it} + \beta_4 \text{log\_pop}_{it} + \beta_5 \text{polity}_{it} + \epsilon_{it}
\]

Separate regressions are run for both **imports** and **exports**.

### 2. Machine Learning Analysis
To capture **nonlinearities** and **heterogeneous treatment effects**:
- The dataset is **split into training and testing sets**, ensuring each country has both `sanction = 0` and `sanction = 1` periods in both sets.  
- ANN model is trained on trade outcomes.  
- **SHAP values** are calculated to interpret model predictions and identify which variables most influence trade changes under sanctions.

---

## Main Findings

1. **Negative Effect on Targeted Countries:**  
   Sanctions significantly reduce both imports and exports, confirming the expected trade-restrictive impact.

2. **Positive Spillover to Neighbors:**  
   Neighboring countries experience increased trade flows, suggesting **trade diversion** as global partners redirect economic activity toward non-sanctioned markets.

3. **Robustness Across Models:**  
   Both OLS and ML–SHAP analyses consistently show these effects.

Dependent var: log_imports
                            OLS Regression Results                            
==============================================================================
Dep. Variable:            log_imports   R-squared:                       0.835
Model:                            OLS   Adj. R-squared:                  0.827
Method:                 Least Squares   F-statistic:                     524.7
Date:                Sun, 19 Oct 2025   Prob (F-statistic):               0.00
Time:                        18:45:25   Log-Likelihood:                -7354.5
No. Observations:                2590   AIC:                         1.495e+04
Df Residuals:                    2470   BIC:                         1.565e+04
Df Model:                         119                                         
Covariance Type:                  HC3                                         
====================================================================================
                       coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------------
Intercept          -41.0838     16.560     -2.481      0.013     -73.541      -8.627
sanction            -1.0513      0.282     -3.734      0.000      -1.603      -0.499
neighbor             0.9986      0.294      3.395      0.001       0.422       1.575
log_rgdp             1.1047      0.509      2.171      0.030       0.108       2.102
log_pop              0.7031      0.990      0.710      0.478      -1.237       2.644
polity2              0.1468      0.033      4.481      0.000       0.083       0.211
==============================================================================
Omnibus:                      312.266   Durbin-Watson:                   0.380
Prob(Omnibus):                  0.000   Jarque-Bera (JB):             2580.445
Skew:                           0.253   Prob(JB):                         0.00
Kurtosis:                       7.864   Cond. No.                     7.14e+03
==============================================================================

Notes:
[1] Standard Errors are heteroscedasticity robust (HC3)
[2] The condition number is large, 7.14e+03. This might indicate that there are
strong multicollinearity or other numerical problems.

Dependent var: log_exports
                            OLS Regression Results                            
==============================================================================
Dep. Variable:            log_exports   R-squared:                       0.837
Model:                            OLS   Adj. R-squared:                  0.829
Method:                 Least Squares   F-statistic:                     533.8
Date:                Sun, 19 Oct 2025   Prob (F-statistic):               0.00
Time:                        18:45:25   Log-Likelihood:                -7330.4
No. Observations:                2590   AIC:                         1.490e+04
Df Residuals:                    2470   BIC:                         1.560e+04
Df Model:                         119                                         
Covariance Type:                  HC3                                         
====================================================================================
                       coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------------
Intercept          -46.1232     16.358     -2.820      0.005     -78.183     -14.063
sanction            -1.0459      0.278     -3.762      0.000      -1.591      -0.501
neighbor             1.0336      0.292      3.541      0.000       0.461       1.606
log_rgdp             1.2662      0.509      2.490      0.013       0.269       2.263
log_pop              0.7829      0.983      0.796      0.426      -1.144       2.709
polity2              0.1416      0.032      4.374      0.000       0.078       0.205
==============================================================================
Omnibus:                      317.842   Durbin-Watson:                   0.381
Prob(Omnibus):                  0.000   Jarque-Bera (JB):             2660.094
Skew:                           0.264   Prob(JB):                         0.00
Kurtosis:                       7.937   Cond. No.                     7.14e+03
==============================================================================

Notes:
[1] Standard Errors are heteroscedasticity robust (HC3)
[2] The condition number is large, 7.14e+03. This might indicate that there are
strong multicollinearity or other numerical problems.

