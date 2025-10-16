# TECHNICAL REPORT: VPN CHURN ANALYSIS

---

## 1. Introduction

**Objective**  
This analysis aims to identify the main drivers of customer churn for **Quark VPN**, a fictional VPN provider, based on two years of customer and external data.  
The goal is to understand which factors most influence churn behavior and to quantify their impact, in order to provide data-driven recommendations to improve retention.

**Analytical questions:**
1. Which variables best explain why customers churn?
2. Are there external or behavioral patterns correlated with churn?
3. What actionable strategies can reduce churn?

**Scope assumptions:**
- Only *paid users* are considered.
- Once a user churns, they never return.

**Data sources:**
- `vpn_customer_data.csv` — user-level subscription and behavior data  
- `vpn_events.csv` — major events by country and date  
- `vpn_external_factors.csv` — monthly country-level indicators (freedom, VPN laws, remote work, etc.)

---

## 2. Data Preparation and Cleaning

**Environment:** Python 3.12 + Poetry (dependencies in `pyproject.toml`)  
**Key libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`

### 2.1 Data inspection and quality checks
- Verified datatypes and missing values.

### 2.2 Cleaning and feature engineering
| Step | Description |
|------|--------------|
| Date parsing | Converted `signup_date` and `churn_date` and `date` to `datetime` |
| Temporal grouping | Derived `signup_month` and `signup_quarter` |
| Churn flag | `churn = 1 if churn_date not null else 0` |
| Device usage | Parsed `devices_used` and computed `num_devices` and  `num_distinct_devices`|
| Support ticket | converted `support_tickets` to frequency columns |
| VPN frienly | converted `False` to 0 |
| Customer lifetime | `lifetime_days = (churn_date - signup_date)` (or ongoing if active) |

Final dataset: **100,000 paid customers** across 8 countries and 24 months.

---

## 3. Exploratory Data Analysis (EDA)

### 3.1 General overview
- **Overall churn rate:** 40.04%  
- **Plan distribution:** 56.8% monthly, 43.2% yearly  

### 3.2 Visual patterns
- **Churn rate by plan type:**  
  Monthly users churn 5× more often than annual ones.  
- **Churn rate by signup quarter:**  
  Q2 (spring) cohorts churn ~13pp more than Q4 cohorts.  
- **Lifetime curve:**  
  90-day retention inflection point — churn rate drops sharply after that.

### 3.3 Observations
- No visual or numerical correlation between churn and usage hours, number of devices, or country.  
- Some seasonal variance visible in signups, but modest compared to plan and lifetime effects.

---

## 4. Statistical Testing

To confirm visual trends, we applied appropriate statistical tests for each variable type.

| Variable | Test | p-value | Effect Size | Interpretation |
|-----------|------|----------|--------------|----------------|
| **Plan type** | Chi-square | <0.001 | **V = 0.50 (large)** | Strong predictor |
| **Days Active** | Chi-square | <0.001 | **V = 0.29 (small)** | Weak effect |
| **Signup month** | Chi-square | <0.001 | **V = 0.14 (small)** | Weak effect |
| **Monthly usage** | t-test | <0.001 | d = 0.09 | Significant with negligible effect |
| **Number used Devices** | t-test | 0.78 | d = -0.00 | No effect |
| **Device diversity** | Chi-square | 0.98 | V = 0.00 | No effect |
| **Countries connected to** | t-test | 0.11 | d = 0.01 | No effect |
| **Number of Support tickets** | t-test | 0.71 | d = 0.00 | No effect |
| **Support tickets** | Chi-square | 0.24 | V = 0.00 | No effect |
| **Signup country** | Chi-square | 0.00 | V = 0.01 | Significant with negligible effect |
| **External factors (freedom, employment, VPN law)** | Chi-square | 0.00 | V = 0.01 | Significant with negligible effect |

**Interpretation:**  
Only **plan type** has meaningful predictive power.  
Customer Lifetime and Seasonality for signup has a weak but noticeable influence.  
Other behavioral and external metrics are non-predictive.

---

## 5. Predictive Modeling

What could be done in a follow-up next step is a **logistic regression** to validate the significance of the identified variables.

**Model setup:**
logit(churn) ~ plan_type + days_active_bucket + signup_month + others

We would expect a Random Forest check to feature importance matching the statistical analysis.
1. Plan type  
2. Days active  
3. Signup quarter  

---

## 6. Interpretation & Discussion

### 6.1 Main findings
1. **Contract structure drives churn.**  
   - Monthly plans churn at 61.6% vs. 11.7% for annual.  
   - Plan type explains most of the churn variance.

2. **The first 90 days are critical.**  
   - 54% churn occurs within first three months.  
   - Customers surviving 90 days have 35% churn afterward.

3. **Seasonal effect exists but secondary.**  
   - Q2 (spring) signups churn 13pp more than Q4.  
   - Possibly due to seasonality in motivation or competing offers.

4. **Behavioral metrics are misleading.**  
   - Usage, support tickets, geography, or devices are not churn drivers.  
   - Suggests product satisfaction is not the issue but rather a structural issue (pricing/onboarding).

### 6.2 Limitations 
- Limited time series depth (2 years).  
- Regulatory events show an important (+35pp) churn effect, but sample size is too small to generalize. We would need more data / experiments to conclude.

---

## 7. Strategic Insights Summary

| Rank | Driver | Churn Effect | Action Levers |
|------|---------|---------------|----------------|
| 1️⃣ | **Plan type (monthly)** | +50pp churn | Incentivize annual upgrades |
| 2️⃣ | **Lifetime <90 days** | +20pp churn | Strengthen onboarding flow |
| 3️⃣ | **Seasonality (Q2)** | +13pp churn | Targeted retention campaigns |

**Combined improvement potential:** -12.4pp churn (≈12,400 customers/year).

---

## 8. Reproducibility Notes

| Component | Details |
|------------|----------|
| **Language** | Python 3.12 |
| **Environment management** | Poetry (`pyproject.toml`, `poetry.lock`) |
| **Notebook** | `DataAnalystHomeAssignment.ipynb` |
| **Data folder** | `/data` (contains CSVs) |
| **Repository** | https://github.com/elorridestizons/DataAnalystHomeAssignement |
| **Re-run instructions** | `poetry install && jupyter lab` |

1. **Clone the repository:**
  git clone https://github.com/elorridestizons/DataAnalystHomeAssignement.git
2. **Install dependencies:**
  poetry install
3. **Launch Jupyter:**
poetry run jupyter lab


**Tests executed:**
- Chi-square (categorical)
- t-test (continuous)
- Logistic regression (not executed here but could be used for validation)
- Random forest (not executed here but could be used for feature importance)
- Visualizations for distributions and churn breakdowns

---

## 9. Conclusion

Through a full statistical and exploratory analysis of 100 000 paid VPN users, we identified that **churn is primarily structural, not behavioral.**  
The **monthly plan structure** and **early retention (0–90 days)** are the main levers for improvement.  

Future analysis could extend to:
- Predictive churn scoring
- Experiment tracking for onboarding campaigns

**Reducing churn by 12pp could save ≈12,400 customers/year and +$124K in annual revenue.**

---

**Author:** Elorri DESTIZONS 
**Repository:**  https://github.com/elorridestizons/DataAnalystHomeAssignement
**Date:** 16 October 2025
