# 🏥 ML Hackathon: Town Financial Cost Minimization & Seasonal Vaccine Prediction

## 📌 Overview
This repository contains our group's end-to-end Machine Learning pipeline for the seasonal influenza vaccine prediction challenge. Rather than optimizing traditional classification metrics like Accuracy or ROC-AUC alone, our approach directly targets the **economic bottom line of the town**: minimizing total financial cost driven by vaccine delivery penalties ($300/dose) and hospitalization expenses ($18,500/hospitalization).

---

## 💰 The Financial Model & Objective
Our town faces a strict cost trade-off:
- **Vaccine Cost ($300 / dose):** Incurred for every vaccine distributed (whether accepted or rejected/wasted).
- **Hospitalization Cost ($18,500 / case):** Incurred when an unprotected resident contracts the flu and requires hospitalization.
- **Cost Optimization Goal:** Minimize:
$$\text{Total Town Cost} = (\text{Total Doses Distributed} \times \$300) + (\text{Total Hospitalizations} \times \$18,500)$$

---

## 🔄 Repository Structure & Pipeline Steps

Our notebook is structured into 5 sequential, reproducible execution blocks:

1. **Step 1 — Data Exploration:** Integrated training (`4,756` rows) and testing (`4,749` rows) datasets, analyzed missing value distributions, and aligned column schemas.
2. **Step 2 — Pre-processing:** Applied median/mode imputation, explicitly preserved missing value indicators (treating missingness as a predictive feature), and one-hot encoded categorical columns (`64` final features).
3. **Step 3 — Modelling:** Trained and evaluated **11 diverse models** (Logistic Regression, Random Forest, Extra Trees, Gradient Boosting, Histogram Gradient Boosting, SVM, MLP Neural Network, and a Top-3 Tuned Ensemble) using 5-fold Stratified Cross-Validation.
4. **Step 4 — Performance Analysis:** Implemented person-specific cost-optimal decision thresholds based on net expected benefit.
5. **Step 5 — Submission of Results:** Applied individual probability cutoffs to `dataset_A_testing.csv` (`4,749` test respondents starting at ID `4757`) to output the 5 required final CSV submission files.

---

## 🛠️ Methodological Refinements & Economic Theory

1. **Explicit Epidemiological Assumptions:** Because the dataset contains no hospitalisation records, our baseline risks ($0.6\%$ to $9.5\%$), chronic condition multipliers ($+40\%$ per condition, capped at $3.5\times$), and vaccine efficacy ($50\%$) are treated as **transparent heuristics and assumptions** rather than learned parameters.
2. **Clinical Risk vs. Behavioral Uptake:** We decouple **clinical risk** (who is vulnerable based on age and health status) from **behavioral uptake** (the model-predicted probability $p_i$ that a resident accepts the vaccine).
3. **Person-Specific Cost-Optimal Thresholds:** Rather than a blunt global cut-off, we allocate vaccines based on resident-specific net expected benefit.
---

## 🏆 Final Model Leaderboard & Selected Submissions

| Rank | Model Name | Threshold Policy | Min Total Cost ($) | Vaccine Cost ($) | Hospital Cost ($) | ROC-AUC |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **1** | **Ensemble (Top 3 Tuned Blend)** | **Per-Person** | **$3,892,501** | **$931,200** | **$2,961,301** | **0.8545** |
| **2** | **GradBoost (lr=0.05, d=3)** | **Per-Person** | **$3,894,812** | **$932,400** | **$2,962,412** | **0.8531** |
| **3** | **GradBoost (lr=0.03, d=4)** | **Per-Person** | **$3,897,249** | **$932,700** | **$2,964,549** | **0.8514** |
| **4** | **HistGradBoost (lr=0.03, d=5)** | **Per-Person** | **$3,897,966** | **$930,000** | **$2,967,966** | **0.8482** |
| **5** | **Random Forest (depth=15)** | **Per-Person** | **$3,902,384** | **$943,200** | **$2,959,184** | **0.8462** |
| 6 | Random Forest (depth=10) | Per-Person | $3,906,030 | $944,700 | $2,961,330 | 0.8459 |
| 7 | LogReg (C=1.0, L2) | Per-Person | $3,907,117 | $936,000 | $2,971,117 | 0.8493 |
| 8 | LogReg (C=0.1, L2) | Per-Person | $3,907,250 | $936,000 | $2,971,250 | 0.8493 |
| 9 | Extra Trees (depth=12) | Per-Person | $3,911,425 | $947,100 | $2,964,325 | 0.8425 |
| 10 | MLP Net (64, 32) | Per-Person | $3,914,103 | $940,500 | $2,973,603 | 0.8374 |
| 11 | SVM (RBF, C=1.0) | Per-Person | $3,915,281 | $938,100 | $2,977,181 | 0.8404 |
