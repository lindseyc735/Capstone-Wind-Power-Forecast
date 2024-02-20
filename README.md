# Capstone-Wind-Power-Forecast  

Are windmills an effective source of power? Do they generate enough power to offset the cost and loss of land required to install and maintain the windmills? How sustainable are the windmills themselves? 
In the controversial energy debates, renewable energy sources are highly sought after. As a starting point to assess the wind energy avenue, this project forecasts the Active Power generated by one windmill. 
The issue of windmill sustainability and cost analysis goes beyond this project's scope.
  
## Data Sourcing and Cleaning 

Data for this project was sourced from Kaggle. The data set is comprised of 19 features measured every 10 minutes for one windmill over two years. The featured measures included both external features such as time, wind speed, wind direction, and temperature, as well as features internal to the windmill such as rotor RPMs, blade positions, and turbine status. The data tracked both active and reactive power. This study focuses only on the active power. The original data set can be found [here](https://www.kaggle.com/datasets/theforcecoder/wind-power-forecasting).  
  
In the initial data wrangling stage, non-essential features that were more than 50% null values were dropped. Numerous missing values were still evident. As I explored the correlation of missing values, it appeared that the windmill was recording feature measurements before the windmill was active. Instead of imputing missing values from feature averages or other imputation methods, the initial section of data comprised of missing values was deleted.  
  
## EDA

A Profile Report is run to more extensively explore the data. The report gives 20 alerts regarding features with high cardinality or high correlation, data distribution, and unique values. The report also gives a summary of each feature, and visualizations, as well as notes features that are unique, common, rare, most frequent, and least frequent. This report indicates that the Timestamp feature is comprised of unique values. However, given the nature of time-based data, this is expected and acceptable. The report also indicated that Wind speed, generator RPM, rotor RPM, and generator winding temperature are highly correlated with the active power target feature. Any other area of interest or concern for this data set is explained by the nature of wind and the resulting windmill status and function.  

## Pre-Processing  
  
It became evident that actions were needed to simplify the data to accommodate computational complexity requirements. All features except for the time stamp, the target active power, and four of the highly correlated features (mentioned previously) were dropped. The time stamp feature was converted to DateTime format and set as the index. Finally, the data was aggregated to provide daily averages rather than exact measurements recorded every 10 minutes. To reserve a final comparison of my model with the actual data gathered, I separated the last 15 days of data and saved it as its own data set.   
  
## Modeling & Visualization

Based on the nature of wind and windmill functions, as well as initial visualizations revealing seasonality in the data, SARIMAX, XGBoost with GridSearchCV parameter tuning, and LMST modeling were selected. The models were evaluated by MAE, RMSE, and MAPE metrics. The XGBoost model with GridSearchCV() proved to be the best model with the lowest RMSE and MAPE values, and the second-lowest MAE of all three models. The SARIMAX model produced the next best metrics and may have outperformed the XGBoost model with additional parameter tuning or increased computational abilities available. Development of the current SARIMAX model involved several instances of multi-hour processing times, program crashing, and lack of available memory. After comparing model performance metrics and selecting XGBoost, the mean and standard deviation of supporting features during the last 60 days were utilized to generate 15 days' worth of new supporting feature data for model training. This data in the XGBoost model forecasted 15 data points for the active power feature. As a visual comparison, I created a line graph displaying the last 30 days of historical data (before the separated final 15 days), the predicted data, and the actual data reserved in the pre-processing stage. Adding the respective data points revealed that the XGBoost model forecasted a total of 6696.6279296875 units of active power, while the actual data totaled units of data in the final 15 days.  

## Next Steps  
  
Advocates for wind energy programs claim that implementing wind power for your home or business will reduce your electricity bills by large percentages. If customers requested similar data logs from the windmill company and used the XGBoost model developed in this program, they could then project how much power the suggested windmill would generate in the future. Similarly, the model could be used on historic customer energy usage data to predict how much energy the customers will need over the same period. This, along with analysis of windmill installation and maintenance cost and sustainability will better equip potential wind energy customers in their energy sourcing decisions.
