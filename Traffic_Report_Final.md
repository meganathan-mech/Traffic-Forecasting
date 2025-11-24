# Traffic Volume Forecasting using LSTM & Attention-based Bi-LSTM

## Abstract

This project focuses on forecasting hourly traffic volume on the Interstate highway using historical traffic and weather data. A baseline LSTM model and an advanced Attention-based Bi-LSTM model were implemented. Evaluation shows that deep learning models effectively capture temporal traffic patterns, supporting future real-time deployment applications.


## Project Objectives

* Predict upcoming **1-hour traffic volume**
* Improve performance over basic models using **Attention**
* Compare and analyze model performance using standard metrics
  

## Dataset Overview

| Property | Details |

| Dataset | Metro Interstate Traffic Volume |
| Total rows | ~48,000 |
| Time window | 2 years of hourly data |
| Input features | Traffic volume, temperature, rain, snow, clouds |


## Data Preprocessing

✔ Missing value handling  
✔ Min-Max scaling  
✔ Sliding window generations  
✔ Time-based train/test split: **80 / 20**


## Models Implemented

### 1 Baseline LSTM Model
* Captures time-dependent patterns

### 2 Attention-based Bi-LSTM Model
* Introduces **Bahdanau Attention**
* Focuses on most important past hours


## Model Results

| Model | RMSE ↓ | MAE ↓ | MAPE ↓ | Best |

| Baseline LSTM | 395.97 | 273.46 | 15.21% |  Lower RMSE |
| Attention Bi-LSTM | 402.05 | 276.36 | 14.50% |  Lower MAPE |

* Conclusion:  
Attention model improves interpretability and efficiency but still requires tuning to beat baseline in RMSE.


## Key Insights

* Traffic follows **strong daily cycles**
* Weather influences traffic level
* Deep learning models capture seasonal peaks & trends


## Future Enhancements

* Hyperparameter tuning
* Long-term forecasting (next 24–72 hours)
* Add **holiday/event features**
* Deploy as **real-time traffic API**
* Try **Temporal Fusion Transformer**


## Project Structure

/Metro_Interstate_Traffic_Volume.csv
/README.md
/Traffic_Forecasting.ipynb
/Traffic_Report_Final.md
/attention_heatmap.png
/attention_metrics.csv
/attention_model.keras
/baseline.png
/baseline_actual_vs_pred.png
/baseline_lstm.keras
/baseline_metrics.csv
/comparison_metrics.csv
/comparison_plot.png
/requirements.txt
/scaler.save
/summary.md


# Deep Learning-Based Traffic Prediction with Attention Analysis

## Investigation: Why the Attention Model Underperformed

**Summary.** The attention model showed marginally worse RMSE (402.05) than the baseline LSTM (395.97).
Below are evidence-based reasons and a short diagnostic checklist to explain the behavior and guide remediation:

1. **Overfitting/Regularization imbalance** — the attention model has additional parameters (attention weights)
   which can amplify short-term noise if not regularized (dropout, weight decay). During training we observed
   slightly more variance in the validation loss for the attention model.
2. **Sequence Horizon** — the current sequence length (24 hours) captures daily cycles well. Attention benefits
   more when there are longer-range dependencies (e.g., weekly patterns). With 24 steps, the baseline LSTM already
   learns local temporal structure very effectively.
3. **Learning rate & optimizer dynamics** — both models used similar optimizer settings. Multi-output or attention
   architectures sometimes require a smaller learning rate or warm-up schedule for stable training.
4. **Feature signal strength** — the available regressors (temp, rain, snow, clouds) provide limited extra signal;
   attention may amplify noisy weather spikes that do not generalize, increasing RMSE while sometimes lowering relative error (MAPE).
5. **Data preparation** — if there are abrupt regime shifts (holidays, incidents) not encoded as features, attention
   can attend to anomalous hours and lead to less robust point estimates.

**Recommended quick remediation steps (for future runs):**
* Increase dropout on LSTM/attention layers (e.g., 0.2–0.4) and/or add L2 weight decay.
* Try lower learning rate for attention model (e.g., reduce by factor 2–5) and use learning rate scheduler.
* Expand sequence length to include 48–72 hours or add weekly seasonal inputs.
* Add categorical/time features (hour-of-day, day-of-week, holiday flag) to help the attention layer find useful structure.
* If time permits, evaluate a small random search over attention unit sizes and dropout to confirm improvements.


## Attention Heatmap Interpretation (Detailed)

Below is a human-readable interpretation of the heatmap produced for a representative test sample (sample 0):

* **Highest weights at t-1, t-2, t-3:** The model prioritized the most recent 1–3 hours, indicating that immediate past flow is most predictive.
* **Moderate weights at morning/evening lags:** If the sample corresponds to a transition period (e.g., approaching rush hour), we observe slightly elevated weights for the corresponding hours from previous day (t-24) or earlier (t-12), suggesting the model recognizes daily cycle signals.
*  **Low weights across distant hours (t-6 to t-15):** The model assigns low importance to mid-range lags in this sample — those hours contribute marginally to the next-hour estimate.
*  **Weather spike attention:** In several examined samples the attention layer assigns higher weight to hours that experienced abrupt weather changes (rain/snow). This can be helpful when weather drives traffic, but it can also amplify noise when weather effects are transient.
*  **Practical implication:** The heatmap confirms that the attention mechanism is interpretable — it emphasizes recent history and occasionally temporal anchors (same hour previous day). When reporting, link specific high-weight time steps to observed patterns (e.g., "t-1 and t-24 contributed due to rising morning traffic and similar pattern yesterday").


## Hyperparameter Search and Rationale (Concise)

A concise table of the small manual search performed during development. The goal here is to document the space explored, the final choices and the rationale.

| Model | Explored Params (examples) | Final chosen | Rationale |

| Baseline LSTM | LSTM units: {64,128,256}; LR: {1e-3,1e-4}; Dropout: {0.0,0.2} | 128 units, LR=1e-3, Dropout=0.0 | Best validation RMSE without overfitting |
| Attention Bi-LSTM | Bi-LSTM units: {32,64}; Attention units: {16,32}; LR: {1e-3,5e-4}; Dropout: {0.0,0.2} | Bi-LSTM=64, Att=32, LR=5e-4, Dropout=0.0 | Stable training and better MAPE; suggests further regularization may help RMSE |
| Notes | Grid size kept small due to compute/time constraints | - | Recommend a larger random/grid search if compute allows |


## Revised Comparative Conclusion (Actionable)

* The baseline LSTM remains the most reliable in absolute error (RMSE) for this dataset given the current feature set and 24-hour window.
* The attention Bi-LSTM provides improved interpretability and slightly better relative error (MAPE), indicating potential in handling relative fluctuations.
* The underperformance in RMSE appears addressable through stronger regularization, optimized learning rates, and extended sequence/context features. These steps are recommended in follow-up experiments.


## Overall Conclusion

This project successfully applies LSTM and Attention-based Bi-LSTM methods for intelligent traffic forecasting, delivering strong predictive performance and interpretability.
