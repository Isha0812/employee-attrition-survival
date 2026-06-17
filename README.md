# Employee Attrition — Survival Analysis
### Time-to-Attrition Modelling using Kaplan-Meier, Log-Rank Test & Cox Proportional Hazards Regression

---

## Overview
Most attrition analyses ask *"will this employee leave?"* — a binary classification 
problem. This project asks a richer question: **"how long until they leave, and what 
factors accelerate or delay departure?"**

This is a survival analysis problem. Using the IBM HR Analytics dataset (1,470 
employees, 237 attrition events), I apply three statistical methods — Kaplan-Meier 
estimation, log-rank testing, and Cox proportional hazards regression — to model 
employee retention as a time-to-event process.

---

## Motivation
This project extends the theoretical framework from my research on the 
**Lomax-Gompertz Distribution** (presented at the Kerala Statistical Association 
International Conference, 2024) — which focused on parametric survival modelling 
and hazard function construction. Here I apply the non-parametric and semi-parametric 
end of the same spectrum to a real-world HR dataset, demonstrating that survival 
analysis has direct business applications beyond medical and reliability contexts.

---

## Dataset
- **Source:** [IBM HR Analytics Employee Attrition Dataset](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)
- **Size:** 1,470 employees, 35 features
- **Event:** Attrition = Yes (employee left the company)
- **Duration:** YearsAtCompany
- **Censoring:** Employees still employed are right-censored (Attrition = No)
- **Attrition rate:** 16.1% (237 events, 1,233 censored)

---

## Methods

### 1. Kaplan-Meier Estimation
Non-parametric estimator of the survival function S(t) — the probability of remaining 
employed beyond time t. Makes no distributional assumptions. Used to visualise overall 
retention and compare survival curves across groups (overtime vs no overtime, by department).

### 2. Log-Rank Test
Non-parametric hypothesis test comparing the full survival distributions of two groups 
across all time points. Tests whether observed differences in KM curves are statistically 
significant or attributable to chance.

### 3. Cox Proportional Hazards Regression
Semi-parametric regression model estimating the effect of multiple covariates on the 
hazard rate simultaneously. The model takes the form:

h(t|X) = h₀(t) · exp(β₁X₁ + β₂X₂ + ... + βₚXₚ)

where h₀(t) is the unspecified baseline hazard and exp(βᵢ) are hazard ratios — the 
multiplicative effect of each covariate on attrition risk.

---

## Key Findings

| Variable | Hazard Ratio | 95% CI | p-value | Interpretation |
|---|---|---|---|---|
| **OverTime** | **3.24** | 2.50–4.19 | <0.005 | Overtime employees face 3.24× the attrition hazard |
| JobSatisfaction | 0.77 | 0.68–0.86 | <0.005 | Each 1-point increase → 23% lower hazard |
| WorkLifeBalance | 0.78 | 0.65–0.94 | 0.01 | Each 1-point increase → 22% lower hazard |
| Age | 0.95 | 0.93–0.97 | <0.005 | Each additional year → 5% lower hazard |
| MonthlyIncome | ~1.00* | 1.00–1.00 | <0.005 | Significant protective effect (z = −7.92) |
| Department_R&D | 0.70 | 0.38–1.26 | 0.23 | Not significant after controlling for other factors |
| Department_Sales | 1.36 | 0.74–2.51 | 0.32 | Not significant after controlling for other factors |

**Model fit:** Concordance = 0.82 (equivalent to AUC in classification — excellent discriminative ability)

**Key insight:** Department appeared significant in raw KM curves (Sales worse, R&D better) 
but became non-significant in Cox regression once individual-level factors were controlled. 
This indicates department-level differences are confounded by overtime rates and satisfaction 
levels — not driven by department itself.

**Business recommendation:** Reducing mandatory overtime is the single highest-leverage 
retention intervention — its effect size (HR = 3.24) dwarfs all other variables by a 
factor of 4 or more.

---

## Project Structure

employee-attrition-survival/

│

├── data/

│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv

├── visuals/

│   ├── tenure_distribution.png

│   ├── km_overall.png

│   ├── km_overtime.png

│   ├── km_department.png

│   └── cox_hazard_ratios.png

├── attrition_survival.ipynb

└── README.md

---

## Libraries Used
- `lifelines` — Kaplan-Meier, log-rank test, Cox regression
- `pandas` — data manipulation
- `matplotlib` / `seaborn` — visualisation

---

## Author
**Isha Mohamed Rafi**  
MSc Statistics with Data Science  
Rajagiri College of Social Sciences, Kalamassery  
[LinkedIn](linkedin.com/in/isha-rafi) | [Email](mailto:ishamohamedrafi@gmail.com)