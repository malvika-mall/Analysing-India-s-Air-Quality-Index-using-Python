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

Major components of Air Pollution in India:
![corr](https://github.com/malvika-mall/Analysing-India-s-Air-Quality-Index-using-Python/blob/main/04%20Analysis/Visualizations/Correlation%20Matrix.png?raw=true)

We see that BTX has the lowest correlation with AQI- which is perfectly in sync with the AQI calculation formula. The air quality index is composed of 8 pollutants ((PM10, PM2.5, NO2, SO2, CO, O3, and NH3), but does not directly account for BTX.




