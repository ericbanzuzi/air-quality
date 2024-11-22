# Air Quality Prediction Service for Amsterdam, Vondelpark  

This project builds an **Air Quality Forecasting Service** using data from air quality sensors and weather forecasts. The service predicts air quality levels for Amsterdam's Vondelpark and provides an interactive interface to explore both predictions and historical data. The project is based on Chapter 3 of the [O'Reilly book - Building Machine Learning Systems with a Feature Store](https://github.com/featurestorebook/mlfs-book).

---

## Overview  

| **Dynamic Data**                     | **Prediction Problem**                            | **User Interface**                                                                | **Monitoring**   |
|--------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------------|------------------|
| - Air Quality Sensor: [aqicn.org](https://aqicn.org/city/netherland/amsterdam/vondelpark/) <br> - Weather Forecasts: [open-meteo.com](https://open-meteo.com/) | Daily forecast of PM2.5 levels for the next 9 days at the Amsterdam Vondelpark sensor. | Interactive [Dashboard](https://ericbanzuzi.github.io/air-quality/air-quality) hosted on GitHub Pages for viewing predictions and trends. | Hindcasts for model evaluation. |

---

## Features  

### **Daily Predictions**  
- **Model**: Uses an XGBRegressor model to predict PM2.5 levels in Amsterdam near Vondelpark.  
- **Automation**: Scheduled via GitHub Actions to run at 7:00 UTC daily.  

### **Dashboard**  
- An [interactive dashboard](https://ericbanzuzi.github.io/air-quality/air-quality/)  displays air quality predictions and trends.  

---

## Application Architecture  
![architecture](https://github.com/user-attachments/assets/c2eba349-1dd0-4878-ada5-f64b8470550b)


### **Key Components**  
#### 1. **Data Connection**  
   - Integrates with Open Meteo and World Air Quality Index APIs to access weather forecasts and air quality data. It also accesses historical data from a local  `.csv` file which contains air quality measurements throughout the years until 17.11.2024. 

#### 2. **Historical Data Backfill**  
   - Processes and stores historical air quality sensor and weather data.  
   - Creates the feature groups `weather` and  `air_quality` with lagged air quality metrics for the previous 3 days.  

#### 3. **Feature Pipeline**  
   - Updates feature groups `weather` and  `air_quality` daily with real-time data from the air quality and weather APIs.  

#### 4. **Training Pipeline** 
   - The component retrieves the data from the feature groups `weather` and  `air_quality` and creates a feature view which allows to define a schema used to define model target feature/labels.  
   - Trains an XGBoost regression model using the following features: temperature, wind speed and direction, amount of rain and air quality for the previous 3 days.
   - Saves the model as a model registry in  `air_quality_xgboost_model`.

#### 5. **Inference Pipeline**  
   - Retrieves the trained model and uses the feature groups to generate daily predictions for the next 9 days.  
   - Stores predictions in the feature group `aq_predictions`.  


### **Pipeline Scheduling**  

The architecture diagram shows that components 2 and 4 operate on a daily schedule, ensuring updates to the feature groups and generating predictions. In contrast, components 1 and 3 are triggered only when there is a need to generate or modify the feature groups, model, or feature view. The resulting inferences are then displayed through a dashboard hosted on GitHub Pages.


---

## Model Insights

### Performance on Test Set

| **With Lagged Air Quality Features**                                   | **Baseline Model (No Lagged Air Quality Features)**                              |
|------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| ![image](https://github.com/user-attachments/assets/f32199a8-0adf-4a8e-95bd-083f903e5783) | ![no_lag_features](https://github.com/user-attachments/assets/b2ad8ffe-4498-423b-807c-20889d53fd7a) |

### Feature Importance

| **With Lagged Air Quality Features**                                   | **Baseline Model (No Lagged Air Quality Features)**                              |
|------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| ![image](https://github.com/user-attachments/assets/af80887f-497b-4991-95cd-9b05ab8b5486) | ![no_lag_featurs_importance](https://github.com/user-attachments/assets/e1a484d4-5a17-4537-851d-f2d900c8d6f8) |




---
## Getting Started  

### **Prerequisites**  
- Python 3.8 or higher.  
- Required libraries (listed in `requirements.txt`).  

### **Installation**  
1. Clone the repository:  
   ```bash  
   git clone https://github.com/ericbanzuzi/air-quality.git  
   cd air-quality  
   ```  
2. Install dependencies:  
   ```bash  
   pip install -r requirements.txt  
   ```  

Refer to the [Google Doc Tutorial](https://docs.google.com/document/d/1YXfM1_rpo1-jM-lYyb1HpbV9EJPN6i1u6h2rhdPduNE/edit?usp=sharing) for step-by-step instructions for setting up the service.  

 
---

## Acknowledgments  

- **Data Sources**:  
  - [World Air Quality Index](https://waqi.info/)  
  - [OpenMeteo](https://open-meteo.com/)  
- **Book Reference**:  
  - [Building Machine Learning Systems with a Feature Store](https://github.com/featurestorebook/mlfs-book).  
- Inspired by real-world applications of air quality forecasting and interactive ML systems.  
 

