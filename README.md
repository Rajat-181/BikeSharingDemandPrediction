# BikeSharingDemandPrediction
Bike Sharing Demand Prediction for capital bike share.
Introduction:
 
Bike sharing market is growing all over the world these years. It is meaningful to do some analysis about it because more and more companies are getting into this business. Since bike sharing is a recent phenomenon, not that many related analyses have been done upon this topic. However, more and more bike sharing companies have started to realize the importance of data driven decision making. One of the major aspects that can be addressed by data analysis is to predict the demand of bikes on any given day. Knowing the demand would help us in creating a better supply and subsequently reduce the gap between supply and demand.

Capital bikeshare, established in 2010 and based in Washington DC, is one of the earliest bike sharing programs and still growing at a good pace. To predict the demand, we have a dataset which includes hourly rental information of bikes from Capital bikeshare with corresponding weather and seasonal information for the years 2011 and 2012. By exploring the dataset, we found that the demand for bike is not constant and varies under different weather conditions. The question here is whether there is a way to find the balance between demand and supply to achieve better operational efficiency. Hence, we are conducting this analysis on this dataset is to predict the demand of bikes on any given day and time based on the information we have. This predicted demand can be used to adjust our supply accordingly and achieve lower maintenance cost and maximize the profit. 

We did a series of analysis to find the best fit model. First, distributions of bike rental counts vs. various variables (season, month, hour, weather etc.) have been plotted to find how demand changes under different conditions. Second, OLS regression analysis has been conducted to find the best fit model to predict the future demand. Based on the information we have, we confirmed that the demand of bike changes under different conditions (weather, season, hour, temperature, etc.). After several OLS regression analysis, we found our best fit model to predict the future demand. We also included the consideration of time series in our best fit model. 

Computational Setup / Steps:
Import libraries and read the dataset:
Import python libraries to be used in data analysis: pandas, NumPy, statsmodel, matplotlib, and datetime. Use pandas.read_csv to read the dataset “train.csv”. 

Preprocessing the Dataset:
1.	Remove outliers from the data that skew the datasets. There were several outliers in the number of rentals, which were removed to have a uniform distribution of rentals.
2.	Use datetime function to process the timestamp and split it into year, month, hour, and weekday. Add new columns to the data frame accordingly. Drop the columns that won’t be used in this analysis, such as datetime, feels like temperature, casual, and registered. Delete the rows where data is not available in all the columns. In our dataset the only missing values in this dataset are under wind speed column.
3.	Use groupby function to find the sum of the number of bike being rented in different seasons, weekdays, holiday/non-holiday, workday/non-workday, weather types, years, months and hours.
4.	Plot out the results in box plot using boxplot function by seaborn to compare and visualize if the number of rented bike changes under different conditions mentioned above.

Regression Model:
[Note: Refer to Appendix for the final regression summary]
1.	Build a correlation matrix among all the variables (dependent and independent) to examine the correlations between each variable and eliminate multicollinearity, if there is any correlation between the independent variables. Variables for the regression model are chosen based on this matrix.
2.	We created categorical variables for hours (24 - one for each hour), weather (4), season (4). Then we conducted the linear regression with demand as the dependent variable and hours, season, month, weather, temperature and wind-speed as the independent variables. After finally including the time series, Adjusted R-Squared for the regression is 0.62. On observing the residual plot, we saw a conical pattern indicating heteroskedasticity. (Figure 5)
3.	To account for heteroskedasticity, we made natural logarithmic of demand as the dependent variable and keep independent variables as above and conducted the regression analysis again. R-Squared for this regression was 0.792 and MSE is .681. Residual graph did not show consistent patterns in this graph (Figure 6)
4.	Since the company is very new, there is an increasing number of subscribers and that will not be explained by the weather situations alone. Hence, we added a bias term which increases by 1 every month. Then we conducted the regression again.
5.	This is our final regression model used to predict demand as we have reached a high R-Squared value of 0.817 and MSE = .638. Detailed results are provided in the appendix at the end. 

The most computationally heavy operation and the slowest part in the program is running the regression in the code. The time taken to run the regression can also be partly attributed to the large number of dummy variables used for the OLS regression analysis.  As we keep increasing the size of the dataset, the amount of time that we need to run the regression analysis increases even further. So, we can say that the time taken increases linearly as the data size increases. 

The runtime vs data size is plotted in Figure 1 below.

 
. 
Fig 1: Relationship between size of the data and regression run time

Results:


 
Figure 2. Box Plot of count (number of bike get rented) vs. season (left above), weekday (right above), month (left below), and hour (right below)

Observations form Figure 2:
1)	The demand is lowest in winter and highest in summer.
2)	For any day, maximum demand is between rush hours: 7-9 AM and 5-7 PM

 
Figure 3. Box Plot of count (number of bike get rented) vs. user type (left above), holiday indicator (right above), weather (left below), and year sub-setting season (right below)

Observations form Figure 3:
●	Bike rentals is far more high for registered users as compared to casual users
●	The count in fall has a higher median value on holidays as compared to the non-holidays.
●	The number of bikes rented decreases as the weather gets more severe
●	Demand of bikes has shown huge growth from year 2011 to 2012.  

A correlation matrix was made to check if there was any correlation between the independent variables to avoid multicollinearity. 
 
Figure 4. Correlation Matrix

Based on the trends we observed in this dataset, we conducted the OLS regression analysis and found the best fit model to predict the future demand using independent variables hour, month, weather, temp, humidity and wind speed (1st model). However, we found that the residual of this model is not normal distributed, which means there was some heteroskedasticity (Fig. 5). So, we converted the dependent variable (y) to logarithmic form and re-conduct the regression, we found a better fit model with better residual distribution (2nd model) plotted in Fig 6.  Finally, to incorporate the growth across the time of two year (2011 to 212), we included the time as a variable into our regression model (3rd model). The results are shown in below in Table 1 and the regression report is shown in appendix at the end.


 	 
Fig 5: Heteroskedasticity in case of Regression Model 1 	Fig 6: Residuals in case of Regression Model 2



	1st model	2nd model	3rd model
Adj. R squared	0.622	0.792	0.817
MSE	213.49	0.681	0.638

Table 1. Adj. R squared value and MSE for regression analysis

Conclusion:

In conclusion, the analysis we did on this dataset shows that the demand of bike changes in different conditions. The regression model explains 81.7% of the variability in demand with a MSE of 0.638. After including the factor of time in the equation the model became more robust with a better R-Squared and lower RMSE.

From the exploratory analysis, we saw how clear and sunny weather attracts more riders as compared to rainy and snow weather. We also found that the demand is maximum during morning (7-9 AM) and evening (4-7 PM) travel hours. We also saw how the demand of bikes drop in winter as compared to other seasons and how it has raised in the span of just one year. With the developed model using all these variables, we could have strategically maintained the supply depending on the 30-day weather forecast. And this could have helped our company to reduce the gap between the demand and supply leading to a higher revenue and a better profit margin. This also gives the company scope to change the revenue model by introducing price surge during peak hours based on the forecast of demand.

Although this model is very robust for short-term demand prediction we need to keep in mind that we cannot predict the demand far too ahead in time because of two reasons: 

(1) Growth rate or the coefficient of time might be higher in the predicted model since company has newly ventured two years before. The growth would slow down and stabilized later in time which the model doesn’t incorporate. 
(2) To forecast the demand, we need the forecast of the weather-related variables as well and we usually don’t have sources to obtain the accurate forecast of weather too much in advance 

Also, currently we have done the analysis predicting demand for the whole city and it can be further enhanced by predicting demand in specific areas/ bike stations. To do that we would need information like population density, proximity if any to high density areas like educational institutes, corporate offices, market and other popular places.

Appendix:

Regression Summary:
 
