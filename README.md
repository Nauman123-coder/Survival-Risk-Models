# PBC Survival Risk Modeling: Cox PH vs Random Survival Forest

A comprehensive survival analysis project comparing **Cox Proportional Hazards** (linear) and **Random Survival Forest** (non-linear) models to predict patient survival and risk in **Primary Biliary Cirrhosis (PBC)** — a progressive liver disease.

This notebook demonstrates best practices in medical survival modeling: handling censored data, time-to-event prediction, model comparison using Harrell’s C-index, and interpretability through hazard ratios and variable importance.

---

### Why Survival Models Are Essential in Medicine

Standard machine learning models fail when:
- Patients are **censored** (lost to follow-up or still alive at study end)
- We need to predict **not just if, but when** an event occurs

**Cox PH** and **Random Survival Forests** correctly handle both issues, enabling accurate risk stratification and personalized prognosis — widely used in oncology, cardiology, and transplant medicine.

---

### Dataset: Mayo Clinic Primary Biliary Cirrhosis (PBC)

- **Source**: Famous Mayo Clinic trial (1974–1984)
- **Samples**: 258 patients (cleaned)
- **Target**: Time to death or liver transplant (converted to years) + event indicator (1 = event occurred, 0 = censored)
- **Features** (17 clinical variables):
  - Age, sex, treatment (`trt`)
  - Lab values: bilirubin (`bili`), albumin, copper, prothrombin time (`protime`), etc.
  - Clinical signs: ascites, hepatomegaly, spiders, edema
  - Histologic stage (1–4)

---

### Models Implemented

| Model                        | Type           | Strengths                                  |
|------------------------------|----------------|-----------------------------------------------------|
| **Cox Proportional Hazards** | Linear / Semi-parametric | Highly interpretable hazard ratios, baseline hazard |
| **Random Survival Forest**   | Non-linear / Ensemble | Captures interactions & non-linear effects, no PH assumption |

Both use:
- 60/20/20 train/validation/test split
- Standardization of continuous features
- One-hot encoding for categorical variables (`edema`, `stage`)

---

### Performance Results (Harrell’s C-index)

| Split       | Cox PH   | Random Survival Forest |
|-------------|----------|------------------------|
| Training    | 0.827    | —                      |
| Validation  | 0.854    | 0.830 → **0.862**      |
| Test        | 0.848    | **0.862**              |

**Conclusion**: Random Survival Forest slightly outperforms Cox PH on held-out data, showing superior predictive accuracy.

---

### Key Predictive Features

| Feature       | Cox PH (Hazard Ratio) | RSF (VIMP Rank) | Insight |
|---------------------------------------|-----------------------|-----------------|--------------------------------------------------|
| `bili` (bilirubin)    | High risk             | **#1**           | Strongest non-linear predictor in RSF           |
| `edema`               | Significant           | High             | Important in **both** models                    |
| `albumin`             | Protective            | High             | Low levels → worse prognosis                    |
| `age`, `protime`      | Significant           | High             | Consistent clinical relevance                   |
| `stage`               | Significant           | Moderate         | Captured well by both                           |

**Key Insight**: RSF reveals **bilirubin** as the dominant predictor — often underweighted in basic Cox models due to non-proportional or non-linear effects.

---
### How to Run it
- git clone https://github.com/yourusername/Survival-Risk-Models-PBC.git
- cd Survival-Risk-Models-PBC
- jupyter notebook risk_model_training.ipynb
