# Analysing-India's-Air-Quality-Index-using-Python
A brief analysis of COVID-19's impact on India's AQI and Time Series Forecasting with ARIMA Model.
![Logo](https://images.indianexpress.com/2018/11/air-pollution-759.jpg)

## Overview:
India ranks among the countries with the highest levels of air pollution globally, with several cities consistently recording poor air quality levels.

The Air Quality Index (AQI) serves as a daily indicator of the air quality, with lower values representing favorable air quality and minimal pollution, while higher values indicate escalated pollutant concentrations, posing serious health risks to humans.

Major pollutants in India include particulate matter (PM2.5 and PM10), nitrogen dioxide (NO2), sulfur dioxide (SO2), carbon monoxide (CO), ozone (O3), ammonia (NH3), and lead (Pb). 

 AQI is classified into categories based on scores: Good (0–50), Satisfactory (51–100), Moderately polluted (101–200), Poor (201–300), Very poor (301–400), and Severe (401–500).

## Project Objective:
* Conducting a study on highly polluted cities, and analyzing major pollutant components.
* Performing time-series analysis with a ARIMA model, using computed orders, to forecast India's AQI in 2021.

## Key Questions:
Uncovering Insights: Key Questions for Exploratory Data Analysis:
1.	What is the overall trend of air quality over a specific time period?
2.	Are there any seasonal patterns in air quality? Does air quality vary throughout the year?
3.	Which pollutants contribute most significantly to poor air quality in a particular region?
4.	Are there any geographical patterns in air quality? Are certain areas consistently experiencing better or worse air quality?
5.	Is there a correlation between certain weather conditions (temperature, humidity, wind speed) and air quality?
6.	Are there any significant changes in air quality over the years? Has air quality improved or worsened?
7.	How does air quality compare across different cities or regions within a country?

## Dataset:
Open-source data set from Kaggle. Downlad dataset from below link:
[India Air Quality Data.](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india)

## Exploratory Analysis:
Analysing the impact of COVID-19 induced lockdown on AQI:
To start off, we will be picking out some cities from the most polluted ones(as inferred above) and try to visualize how their AQI changed in 2020 as compared to 2019.
![Top 10 Most Polluted Cities of India](https://github.com/malvika-mall/Analysing-India-s-Air-Quality-Index-using-Python/blob/main/04%20Analysis/Visualizations/Catplot%2010%20Most%20Polluted%20City%20%26%20AQI.png)

Cities which had underwent the most drastic improvement in Air Quality:
![bar](https://github.com/malvika-mall/Analysing-India-s-Air-Quality-Index-using-Python/blob/main/04%20Analysis/Visualizations/Covid%20impact.jpg)

Major components of Air Pollution in India:![corr](https://github.com/malvika-mall/Analysing-India-s-Air-Quality-Index-using-Python/blob/main/04%20Analysis/Visualizations/Correlation%20Matrix.png?raw=true)
We see that BTX has the lowest correlation with AQI- which is perfectly in sync with the AQI calculation formula. The air quality index is composed of 8 pollutants ((PM10, PM2.5, NO2, SO2, CO, O3, and NH3), but does not directly account for BTX.

## Time Series Analysis & Forecasting:
```
# Decompose the time series using an additive model
decomposition = sm.tsa.seasonal_decompose(cities['India_AQI'], model='additive')

from pylab import rcParams # This will define a fixed size for all special charts.
rcParams['figure.figsize'] = 18, 7

# Plot the separate components
decomposition.plot()
plt.show()
```
![image](https://github.com/malvika-mall/Analysing-India-s-Air-Quality-Index-using-Python/assets/135637670/790ce2b1-7dff-4b74-b874-81692b678c03)

## Testing for Stationarity:
```
# The adfuller() function will import from the model from statsmodels for the test; however, running it will only return 
# an array of numbers. This is why you need to also define a function that prints the correct output from that array.

from statsmodels.tsa.stattools import adfuller # Import the adfuller() function

def dickey_fuller(timeseries): # Define the function
    # Perform the Dickey-Fuller test:
    print ('Dickey-Fuller Stationarity test:')
    test = adfuller(timeseries, autolag='AIC')
    result = pd.Series(test[0:4], index=['Test Statistic','p-value','Number of Lags Used','Number of Observations Used'])
    for key,value in test[4].items():
       result['Critical Value (%s)'%key] = value
    print (result)

# Apply the test using the function on the time series
dickey_fuller(cities['India_AQI'])
```
```
Outpput:
Dickey-Fuller Stationarity test:
Test Statistic                 -0.114224
p-value                         0.948003
Number of Lags Used            10.000000
Number of Observations Used    56.000000
Critical Value (1%)            -3.552928
Critical Value (5%)            -2.914731
Critical Value (10%)           -2.595137
dtype: float64
```
The obtained p-value of 0.94 indicates that the time series lacks stationarity. To address this, we employ first-order differencing to eliminate the trend and subsequently conduct a retest using the ADF test.

```
# Check out a plot of autocorrelations
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf # Here, you import the autocorrelation and partial correlation plots
plot_acf(diff)
plt.show()
```
![image](https://github.com/malvika-mall/Analysing-India-s-Air-Quality-Index-using-Python/assets/135637670/1df50949-5589-4ff2-b07a-b66ccec2b2ee)

## Running and fitting the Model:
```
from statsmodels.tsa.api import ARIMA # Import the model you need
model = ARIMA(train, order=(5, 1, 3))
fitted = model.fit()
print(fitted.summary())  # Check model summary

# Generate forecasted values
forecast = fitted.get_forecast(steps=len(test), alpha=0.05)

# Extract the forecasted values and confidence intervals
fc_series = forecast.predicted_mean  # Forecasted values
conf_int = forecast.conf_int()  # Confidence intervals

# Convert forecasted values to a pandas series
fc_series = pd.Series(fc_series, index=test.index)  # Forecasted values

# Generate lower and upper bounds of the confidence interval
lower_series = pd.Series(conf_int.iloc[:, 0], index=test.index)  # Lower bound of the confidence interval
upper_series = pd.Series(conf_int.iloc[:, 1], index=test.index)  # Upper bound of the confidence interval

# Plot
plt.figure(figsize=(12, 4), dpi=100)
plt.plot(train, label='Training')
plt.plot(test, label='Actual')
plt.plot(fc_series, label='Forecast')
plt.fill_between(lower_series.index, lower_series, upper_series, color='k', alpha=0.05)  # Confidence interval
plt.title('Forecast vs Actuals')
plt.legend(loc='upper left', fontsize=8)
plt.show()
```
![image](https://github.com/malvika-mall/Analysing-India-s-Air-Quality-Index-using-Python/assets/135637670/0eb17c34-634d-45c8-86e4-e731c88f44b9)

                        

