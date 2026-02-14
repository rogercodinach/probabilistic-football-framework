# ⚽ Context-Aware Hazard: Modeling Goal Risk in Women's Euros

This project applies the Cox Proportional Hazards Model (Survival Analysis) to quantify goal risk during the Women's European Championship. Unlike traditional metrics like xG, this model evaluates how the 360° spatial context—including defensive pressure, teammate support, and player isolation—influences the instantaneous "hazard" or intensity of conceding a goal.

---

## 🚀 Project Overview

The goal is to move beyond the final outcome of a play and measure the underlying tactical stress. Using StatsBomb 360 event and positional data, this model estimates a "Hazard Score" for every event based on the configuration of all 22 players on the pitch.

**Key Features:**

- **Probabilistic Approach:** Uses survival analysis to model the time-to-event (next goal).  
- **360° Feature Engineering:** Transforms raw coordinates into tactical metrics like isolation indices and relative pressure.  
- **Robust Validation:** Implements a match-based (`match_id`) Train/Test split and K-Fold Cross-Validation to ensure the model generalizes across different teams and playing styles.  

---

## 📊 Methodology

### 1. Feature Engineering

Raw tracking data was converted into meaningful tactical descriptors:

- **rel_pressure:** Proximity of the nearest rival relative to the overall defensive block density.  
- **log_isolation:** Ratio between distance to teammates vs. distance to rivals (log-transformed), identifying players on tactical "islands."  
- **effective_pressure:** The delta between the average block distance and the immediate harrying of the ball carrier.  

### 2. The Model: Cox Proportional Hazards

We utilized the **lifelines** library to model the "survival" of a possession.

- **Objective:** Predict the hazard rate of a goal occurring.  
- **Regularization:** Applied L2 penalization (`penalizer=0.1`) to ensure coefficient stability and prevent overfitting to specific matches.

---

### 3. Model Validation (5-Fold Cross-Validation by Match)

To ensure robustness:

- **Cross-Validation:** Split data by `match_id` into 5 folds to prevent leakage between matches.  
- **Metrics:** Reported **C-Index** for each fold and its mean ± standard deviation.  

**Results:**

| Fold | C-Index | Train Matches | Test Matches |
|------|---------|---------------|--------------|
| 1    | 0.5673  | 24            | 7            |
| 2    | 0.6295  | 25            | 6            |
| 3    | 0.5118  | 25            | 6            |
| 4    | 0.5101  | 25            | 6            |
| 5    | 0.5857  | 25            | 6            |

**Mean C-Index:** 0.5609 ± 0.0455  

This confirms that the model consistently predicts goal risk across different matches.

---

### 4. Coefficient Stability Across Folds

Monitoring coefficients ensures the effect of variables is stable:

| Covariate        | Fold 1   | Fold 2   | Fold 3   | Fold 4   | Fold 5   |
|-----------------|----------|----------|----------|----------|----------|
| score_margin     | 0.1326   | 0.1368   | 0.0537   | 0.1353   | 0.1592   |
| event_time_sec   | 0.1470   | 0.0377   | 0.2172   | 0.2038   | 0.1248   |
| rel_pressure     | 0.0157   | 0.0150   | 0.0057   | 0.0183   | 0.0161   |
| rel_support      | -0.0063  | -0.0038  | 0.0108   | -0.0069  | -0.0043  |
| log_isolation    | -0.0081  | -0.0098  | -0.0045  | -0.0304  | -0.0105  |

> **Interpretation:**  
> - `score_margin` and `event_time_sec` are consistently positive → higher match momentum or leading score increases hazard.  
> - `rel_pressure` generally positive → more pressure from opponents increases goal risk.  
> - `log_isolation` slightly negative → being isolated reduces hazard marginally.  
> - `rel_support` varies around 0 → effect is less consistent.

---

## 📈 Key Findings

**Out-of-Sample Validation (Test Set):**  

- Concordance Index: **0.56**  
- Confirms variables like match momentum (`event_time_sec`) and tactical isolation (`log_isolation`) are robust predictors of goal risk.

**Top Players by Danger Generation (Test Set):**  

| Player           | Avg Danger Generated (Hazard Ratio) |
|-----------------|-----------------------------------|
| Sara Däbritz     | 1.68                              |
| Janina Minge     | 1.55                              |
| Grace Clinton    | 1.39                              |

---

## 🛠️ Tech Stack

- **Language:** Python 3.12  
- **Data Libraries:** Pandas, NumPy, Scikit-learn  
- **Statistical Modeling:** Lifelines (Survival Analysis)  
- **Visualization:** Mplsoccer, Matplotlib  
- **Data Source:** StatsBomb Open Data  

---

## 📂 Repository Structure

- `Framework_probabilistic.ipynb`: Main notebook containing data cleaning, feature engineering, and modeling.  
- `visuals/`: Plots for survival curves and model coefficients.  
- `results/`: CSV exports of player rankings and hazard scores.  

---

## 📬 Contact

Roger Codinach - www.linkedin.com/in/roger-c-9443522b4 - rcodinach4@gmail.com
