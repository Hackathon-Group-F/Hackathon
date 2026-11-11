# 🏥 ML Hackathon: Town Financial Cost Minimization & Seasonal Vaccine Prediction

## 📌 Overview
This repository contains our group's end-to-end Machine Learning pipeline for the seasonal influenza vaccine prediction challenge. Rather than optimizing traditional classification metrics like Accuracy or ROC-AUC alone, our approach directly targets the **economic bottom line of the town**: minimizing total financial cost driven by vaccine delivery penalties ($300/dose) and hospitalization expenses ($18,500/hospitalization).

---

## 💰 The Financial Model & Objective
Our town faces a strict cost trade-off:
* **Vaccine Cost ($300 / dose):** Incurred for every vaccine distributed (whether accepted or rejected/wasted).
* **Hospitalization Cost ($18,500 / case):** Incurred when an unprotected resident contracts the flu and requires hospitalization.
* **Cost Optimization Goal:** Minimize:
$$\text{Total Town Cost} = (\text{Total Doses Distributed} \times \$300) + (\text{Total Hospitalizations} \times \$18,500)$$

---

## 🔄 Repository Structure & Pipeline Steps

Our notebook is structured into 5 sequential, reproducible execution blocks:

1. **Step 1 — Data Exploration:** Integrated training (`4,756` rows) and testing (`4,749` rows) datasets, analyzed missing value distributions, and aligned column schemas.
2. **Step 2 — Pre-processing:** Applied median/mode imputation, explicitly preserved missing value indicators (treating missingness as a predictive feature), and one-hot encoded categorical columns (`64` final features).
3. **Step 3 — Modelling:** Trained and evaluated **11 diverse models** (Logistic Regression, Random Forest, Extra Trees, Gradient Boosting, Histogram Gradient Boosting, SVM, MLP Neural Network, and a Top-3 Tuned Ensemble) using 5-fold Stratified Cross-Validation.
4. **Step 4 — Performance Analysis:** Scanned decision probability thresholds from `0.10` to `0.90` for all 11 models. Discovered that optimal cost thresholds shift left (`0.39` to `0.46`) due to the high cost asymmetry of hospitalizations.
5. **Step 5 — Submission of Results:** Applied winning probability cutoffs to `dataset_A_testing.csv` (`4,749` test respondents starting at ID `4751`) to output the 5 required final CSV submission files.

---

## 🏆 Final Model Leaderboard & Selected Submissions

| Rank | Model Name | Best Threshold | Min Total Cost ($) | Vaccine Cost ($) | Hospital Cost ($) | ROC-AUC |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **1** | **Ensemble (Top 3 Tuned Blend)** | **0.41** | **$3,944,474** | **$977,400** | **$2,967,074** | **0.8545** |
| **2** | **LogReg (C=1.0, L2)** | **0.42** | **$3,948,753** | **$976,500** | **$2,972,253** | **0.8493** |
| **3** | **LogReg (C=0.1, L2)** | **0.44** | **$3,949,253** | **$964,200** | **$2,985,053** | **0.8493** |
| **4** | **HistGradBoost (lr=0.03, d=5)** | **0.41** | **$3,951,282** | **$977,400** | **$2,973,882** | **0.8482** |
| **5** | **GradBoost (lr=0.03, d=4)** | **0.40** | **$3,951,375** | **$988,200** | **$2,963,175** | **0.8514** |
| 6 | GradBoost (lr=0.05, d=3) | 0.39 | $3,952,839 | $990,900 | $2,961,939 | 0.8531 |
| 7 | Extra Trees (depth=12) | 0.46 | $3,956,095 | $979,800 | $2,976,295 | 0.8425 |
| 8 | Random Forest (depth=15) | 0.45 | $3,956,358 | $980,400 | $2,975,958 | 0.8462 |
| 9 | Random Forest (depth=10) | 0.45 | $3,956,882 | $986,400 | $2,970,482 | 0.8459 |
| 10 | SVM (RBF, C=1.0) | 0.41 | $3,976,327 | $985,500 | $2,990,827 | 0.8404 |
| 11 | MLP Net (64, 32) | 0.39 | $3,981,974 | $1,005,000 | $2,976,974 | 0.8374 |

---

## 📊 Key Insights & Feature Drivers
* **The "Big 4" Predictive Columns:** 
  1. `Opinion Seas Risk` (24.6% relative importance)
  2. `Opinion Seas Vacc Effective` (19.4% relative importance)
  3. `Doctor Recc Seasonal` (15.8% relative importance)
  4. `Age Group 65+ Years` (7.5% relative importance)
* **Behavioral Asymmetry:** A resident's personal belief in vaccine effectiveness and a direct recommendation from their physician carry nearly 3x more weight than general demographics.
* **Why Threshold Tuning Works:** Shifting the default `0.50` probability cutoff down to `0.40–0.44` protects borderline high-risk residents from expensive hospitalizations, saving the town between **$14,000 and $26,900** per model.

---
