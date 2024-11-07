# Air Quality System Dashboard

![Hopsworks Logo](./air-quality/assets/img/logo.png)


The air-quality prediction ML System consists of:


| Dynamic Data  | Prediction Problem | User Interface  |  Monitoring |
| ------------- |:-------------:| ------------:| ------------:|
| Air Quality Sensor Data: [aqicn.org](aqicn.org) & <br> Weather Forecasts: [open-meteo.com](open-meteo.com) | Air Quality Forecasting of the level of PM2.5| Github Pages | Hindcasts |


# The Dashboard

{% include air-quality.html %}

![Forecast](./air-quality/assets/img/pm25_forecast.png)


There is also a Python program to interact with the air quality ML system using language (text, voice),
powered by a [function-calling LLM](https://www.hopsworks.ai/dictionary/function-calling-with-llms).

# Model Performance Monitoring

1-Day Hindcast: Predictions vs Outcomes

![Hindcast](./air-quality/assets/img/pm25_hindcast_1day.png)
