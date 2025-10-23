# 📘 Sanctions and Trade: Direct and Spillover Effects

## 🔍 Research Question
This project explores how international sanctions influence trade, both for targeted countries and their land neighbors.  
The central questions are:

1. Do sanctions reduce trade (imports and exports) of the targeted countries?  
2. Do neighboring countries experience positive spillover effects by capturing trade flows diverted from sanctioned nations?

The working hypothesis is that sanctions impose negative trade shocks on targeted economies, while neighboring countries may partially benefit through redirected trade and substitution effects.

---

## 🧹 Data and Cleaning Process

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

## ⚙️ Research Method

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

## 📈 Main Findings

1. **Negative Effect on Targeted Countries:**  
   Sanctions significantly reduce both imports and exports, confirming the expected trade-restrictive impact.

2. **Positive Spillover to Neighbors:**  
   Neighboring countries experience increased trade flows, suggesting **trade diversion** as global partners redirect economic activity toward non-sanctioned markets.

3. **Robustness Across Models:**  
   Both OLS and ML–SHAP analyses consistently show these effects.



