# Hands-On Lab 1: Predictive AI & Machine Learning in Google Colab

**Topic:** Predicting Natural Rubber Compound Tensile Strength from the Recipe  
**Duration:** 25 minutes  
**Audience:** ITUM NDT Polymer & Chemical Engineering Technology students  
**Platform:** Google Colab

---

## Industrial Problem & Learning Objectives
A rubber products plant is developing a carbon-black-filled natural rubber (NR) compound. Every lab trial takes a day (mill mixing, MDR, press cure, conditioning, tensile test). The lab has 120 past trials. Can a model learn which recipe gives the best tensile strength before the next batch is mixed?

In this lab, students:
1. **Load data:** Load 120 lab trials (`rubber_compound_lab_data.csv`).
2. **Data plot:** Plot the data to examine how recipe inputs affect tensile strength (observe the optimum in sulphur and reinforcement from carbon black).
3. **Train/test split:** Split data into inputs ($X$) and single target ($y$: `Tensile_Strength_MPa`), ignoring other measured properties (`Hardness_ShoreA`, `t90_min`, `Delta_Torque_dNm`) to avoid data leakage.
4. **Prediction & model comparison:** Train a **Linear Regression** and a **Random Forest** to predict tensile strength, comparing R², MAE, and predicted vs. measured plots.
5. **Cross-validation:** Validate model stability using **5-fold cross-validation**.
6. **Feature importance:** Analyze which recipe inputs drive **tensile strength** the most.
7. **Formulator & extrapolation:** Use the model to sweep sulphur to find the optimum, then test predictions **outside the trial range**.

---


## 📊 Dataset (`rubber_compound_lab_data.csv`)
Fixed recipe in every trial: NR 100 · ZnO 5 · stearic acid 2 · antioxidant 1 (phr).

| Column | Description | Role |
| :--- | :--- | :--- |
| `Sulphur_phr` | Sulphur, 0.5 – 3.0 phr | Input ($X$) |
| `CBS_phr` | CBS accelerator, 0.5 – 2.0 phr | Input ($X$) |
| `Carbon_Black_N330_phr` | N330 black, 0 – 60 phr (5 phr steps) | Input ($X$) |
| `Cure_Temp_C` | Cure temperature, 140 – 170 °C | Input ($X$) |
| `t90_min` | MDR time to 90 % cure | Measured property (ignored) |
| `Delta_Torque_dNm` | MDR torque rise M<sub>H</sub> − M<sub>L</sub> | Measured property (ignored) |
| `Hardness_ShoreA` | Hardness | Measured property (ignored) |
| **`Tensile_Strength_MPa`** | **Tensile strength** | **Single Target ($y$)** |

> The data are **synthetic**, generated to follow the qualitative behaviour of sulphur-cured NR. Magnitudes are realistic but the numbers are not design data. Tell students this.

---

