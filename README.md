
# Project - Time Series Analysis of Zillows housing Data. 
# Introduction:
For most Americans, their biggest financial investment is their house. To that end there is a lot of money in real estate for investors, REITs or simply an individual home owner who is interested in buying a house as a retirement investment. With this in mind **the goal** of this project is to determine 5 zipcodes in the US for investing and then forecast their return rates using the Box-Jenkins methods. 

#  Data:
<br/>Zillow's research data used for this analysis can be found [here](https://www.zillow.com/research/data/). The data contained about 15k zipcodes. The data did not require any major cleaning or pre-processin; the only changes that were  made was to convert the data into date time object using pandas and then reshaping it from wide to long format using pandas melt funtion.

In an effort to determine the best real estate markets, zipcodes were filtered  as follows :
* Zip codes with the highest urbanization (top 25%) based on Zillow’s urbanization metric.
* Zip codes that did not depreciate during the great recession (07–08)
* Zip codes that have returned 3 % or more over the past 20 years
* Zip codes that are experiencing an uptrend in appreciation rates for past 2 years.

This resulted in about 100 zip codes as seen below:

![alt text](Images/Filtered_100.png)

<br/>Furthermore to determine whether the expected return of the investment is worth the degree of volatility the downside risk was assessed using the coefficient of variance. Downside risk was a constant and had a strong positive correlation with return rates but a relateively weak positive correlation with the amount invested as seen below.

![alt text](Images/CV%20vs%20return.png)
![alt text](Images/cv_amount.png)

The 100 zipcodes were filtered based on low downside risk regardless of the amount invested. After trying a bunch of subsets the 4th quintile was chosen as it gave the best returns. This resulted in about 25 zipcodes. Out of these 25 zipcodes the top 10 (shown below) with highest returns were chosen for forecasting.

![alt text](Images/top10.png)

# Detrending and assessing/choosing ARMA parameters for modelling

Log transformation, rolling means, and differencing was used to detrend the time series and stationarity was evaluated using the Dickey-Fuller Test. Log transform and rolling mean were not very effective as it yielded only 1/10 to be stationary.First order finite differencing resulted into 5 of the time series to be stationary. Second order resulted differencing resulted in all time series to be almost stationary with a mild seasonal component. This seasonality was assessed using ACF and PACF plots, and a lot of the time seies had more than 1 significant MA and QA orders. So the best option was to chose the optimum parameters using some form of gridsearch or parameter turning. Since this had to be done for 10 zipcodes the most efficent way was to use a  Python wrapper that automatically selects the parameters. More info on this can be found [here.](https://alkaline-ml.com/pmdarima/)

#  ARIMA Modeling
Using Auto arima parameters I chose to utilize the SARIMAX model considering the seasonality in the time series. The projections was perfomed using dynamic forecasting to predict a 5 yr projection of all 10 zipcodes. Based on this projection 5-years return were calculated.  A sample of the outcome for one of the promising zipcodes is as follows:
Outcome for Zipcode 70808 (Baton Rouge)
![alt text](Images/top10.png)
Standardized residuals over time are stationary


#  Interpreting Results

Based on 3-5% yearly appreciation we can expect 15-28% overall return in 5 years . Based on the results the 5 best zipcodes are 70806,70808,73069,70810,78753 which on average will return nearly 44% over the next 5 years
. The reason I selected these 5 zipcodes is because they have 1)The residuals show some changing variation over time but they are relatively stationary,This heteroscedasticity will potentially make the prediction interval slightly inaccurate. 2)Residuals are more or less normally distributed. 3)Although there exists some autocorrelation, it is not particularly large and it is unlikely to have any noticeable impact on the forecasts or the prediction intervals.

# Future Work
Out of nearly 15k zipcodes the 3 i have come to chose are from Baton rouge LA which suggests that some external factors did not affect the housing market in these regions compared to others during the recession and they continute to appreciate over 5% per year.So I wanted to see if certain zipcodes can be clustered together based on the underlying geography as an external factor. 2) Also the model preiction can be improved by including other external factors such as mortgage rates, local economy, income , crime etc 3) I would like to also inlcude and analyze rental data as rental income can be a factor in real estate investment.
