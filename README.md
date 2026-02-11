# ⚽ Context-Aware Hazard: Modeling Goal Risk in Women's Euros

This project applies the Cox Proportional Hazards Model (Survival Analysis) to quantify goal risk during the Women's European Championship. Unlike traditional metrics like xG, this model evaluates how the 360° spatial context—including defensive pressure, teammate support, and player isolation—influences the instantaneous "hazard" or intensity of conceding a goal.

---

## 🚀 Project Overview

The goal is to move beyond the final outcome of a play and measure the underlying tactical stress. Using StatsBomb 360 event and positional data, this model estimates a "Hazard Score" for every event based on the configuration of all 22 players on the pitch.

**Key Features:**

- **Probabilistic Approach:** Uses survival analysis to model the time-to-event (next goal).  
- **360° Feature Engineering:** Transforms raw coordinates into tactical metrics like isolation indices and relative pressure.  
- **Robust Validation:** Implements a match-based (`match_id`) Train/Test split to ensure the model generalizes across different teams and playing styles.  

---

## 📊 Methodology

### 1. Feature Engineering

Raw tracking data was converted into meaningful tactical descriptors:

- **rel_pressure:** Proximity of the nearest rival relative to the overall defensive block density.  
- **log_isolation:** The ratio between distance to teammates vs. distance to rivals (log-transformed), identifying players on tactical "islands."  
- **effective_pressure:** The delta between the average block distance and the immediate harrying of the ball carrier.  

### 2. The Model: Cox Proportional Hazards

We utilized the **lifelines** library to model the "survival" of a possession.

- **Objective:** Predict the hazard rate of a goal occurring.  
- **Regularization:** Applied L2 penalization (`penalizer=0.1`) to ensure coefficient stability and prevent overfitting to specific matches.  

---

## 📈 Key Findings

**Out-of-Sample Validation (Test Set):**  

The model achieved a **Concordance Index of 0.56** on unseen data. This confirms that variables like match momentum (`event_time_sec`) and tactical isolation (`log_isolation`) are consistent predictors of goal risk.

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
