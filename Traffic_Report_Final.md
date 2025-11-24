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


## Overall Conclusion

This project successfully applies LSTM and Attention-based Bi-LSTM methods for intelligent traffic forecasting, delivering strong predictive performance and interpretability.

---
