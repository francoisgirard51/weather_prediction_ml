# Weather prediction with machine learning

We try to predict daily maximum temperature using historical weather data from the Oakland International Airport.

---

## Data source

The dataset was obtained from the [National Centers for Environmental Information (NCEI)](https://www.ncdc.noaa.gov/cdo-web/search). We selected daily summaries from the 1960 to 2022, choosing Oakland International Airport for its high-quality data. The data is stored in `local_weather.csv`.

---

## Project overview

### EDA
- **Data inspection:**  
  We analyzed missing values and data distribution across all variables. Several columns (e.g., DAPR, MDPR) exhibited very high missing rates.
  
- **Core variable selection:**  
  Focusing on key meteorological parameters, we extracted precipitation (`PRCP`), maximum temperature (`TMAX`), and minimum temperature (`TMIN`). Snow and snow depth were dropped due to their near-constant values in this region.

- **Handling missing data:**  
  Missing precipitation values were imputed as 0.0 (assuming no rain), while missing temperature readings were forward-filled to maintain temporal consistency.  
  Specific date ranges (e.g., `2011-12-18` to `2011-12-28`) were inspected to understand the behavior of missing entries, and the distribution of precipitation values was analyzed to ensure realistic imputation.

### Feature engineering & modeling
- **Target variable:**  
  We shifted `temp_max` by one day to create a target variable representing the next day’s maximum temperature.
  
- **Baseline model – "Ridge regression":**  
  We chose Ridge regression to mitigate overfitting. Initial predictors included `precip` (daily precipitations), `temp_max`, and `temp_min`.  

- **Enhanced features:**  
  To capture seasonal effects and improve predictions, we engineered additional features:
  - **Rolling & ratio features:**  
    - `month_max`: 30-day rolling average of `temp_max`.
    - `month_day_max`: Ratio of the rolling average (`month_max`) to the current day’s `temp_max`.
    - `max_min`: Ratio of `temp_max` to `temp_min`.
  - **Expanding seasonal averages:**  
    - `monthly_avg`: Expanding average of `temp_max` for each month, capturing long-term trends.
    - `day_of_year_avg`: Expanding average of `temp_max` for each calendar day, capturing typical day-of-year behavior.

### Model evaluation
- **Performance Metrics:**  
  The final model’s MSE is around 19, corresponding to an RMSE of approximately 4–5°F, which indicates a reasonable level of accuracy given the inherent variability in weather.
  
- **Coefficient analysis:**  
  The model’s coefficients reveal the influence of each feature. For example, a negative coefficient for precipitation indicates that higher rain levels are associated with lower maximum temperatures the following day. Seasonal features provide additional context that further refines predictions.
  
- **Error analysis:**  
  By computing and sorting the absolute differences between actual and predicted temperatures, we identified specific dates with large errors. These outliers may correspond to unusual weather events or other anomalies, suggesting areas for further investigation.

---

## Conclusion

This project demonstrates a rigorous approach to forecasting next-day maximum temperature using historical weather data. Our methodology involved careful data cleaning, thoughtful feature engineering, and the application of Ridge regression to balance model simplicity and performance. With an RMSE of roughly 4–5°F (~3°C), our baseline model performs reasonably well, yet there is room for enhancement. Future work could explore advanced models (e.g., Random Forest, XGBoost, LSTM), multi-day forecasting, and robust backtesting to further improve forecasting accuracy and generalization.
