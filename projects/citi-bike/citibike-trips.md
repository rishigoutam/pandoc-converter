---
title: Predicting Citi Bike Trip Demand
subtitle: Using a neural network model to predict ridership based on time and weather
author: Rishi Goutam, Srikar Pamidi, James Goudreault
author-url: 'https://github.com/rishigoutam/citibike'
date: 'April 7, 2022'
abstract: In this article, we show how we analyzed the Citi Bike dataset and built a model to predict ridership based on seasonality and weather.
keywords: LSTM; SARIMA; Citi Bike; NYC Data Science Academy
description: 
return-url: '..'
return-text: '← Return home'
---

# Predicting Citi Bike Trip Demand
In this article, we show how we analyzed the Citi Bike dataset and built a model to predict ridership based on seasonality and weather.

## Introduction
Citi Bike opened in New York City in 2013[^scope] and has since grown in ridership, bikes, and bike dock stations. Predicting demand for bikes is important for Motivate, Citi Bike's parent company, in order to both reduce operating costs and increase ridership. Costs are incurred by having under-utilized bikes on the streets by wear-and-tear on bikes due to exposure to the elements or other forms of damage, so it is necessary to warehouse bikes if they are not serving riders. However, having too few a number of bikes available leads to a poor customer experience and loss of revenue due to dissatisfied customers.

We aim to first understand what features drive demand and then to create a predictive model. Finally, we compare our model against the actual number of trips to evaluate its usefulness.

[^scope]:
    Citi Bike operates in multiple areas and has been active for several years. We focused our analysis on the primary NYC boroughs Citi Bike operates in (Manhattan, Brooklyn, and Queens). Unless otherwise specified, statistics and graphics in this article are for the year 2019 and these boroughs ![](https://hackmd.io/_uploads/B1hC_xnX9.png)

## Analyzing the data
We focused our exploratory data analysis on:

* Rider demographics
* Citi Bike's growth and resilience in the face of COVID-19
* Time (Seasonality)
* Weather

And determined that time and some weather conditions would make for good predictors for a time-series model. Here is that analysis.

### Demographics
By age, most riders are between 20 and 40 years and are mostly male. Citi Bike has two classes of riders--annual subscribers and single-ride or day pass purchasers. We see a default age of 50 years for riders purchasing one-off trips or passes in the chart below.

![](https://hackmd.io/_uploads/HkmjbY2Q5.png)

![](https://hackmd.io/_uploads/BJlaRG0Xq.png)

We can see the gender and customer type distribution through the much-maligned pie chart

![](https://hackmd.io/_uploads/HJmSOKn75.png)
![](https://hackmd.io/_uploads/SJYruY27c.png)

### Growth and Resilience of Citi Bike
As the number of Citi Bike trips grows, operational efficiency is more important to company finances. In addition, there is need for accurately predicting demand and rebalancing stations effectively

![](https://hackmd.io/_uploads/rkAj_YnXq.png)

Bike stations appear to expand along subway lines…potential for further expansion into Brooklyn, the Bronx, or Queens?

![](https://hackmd.io/_uploads/SJXDFY3mq.png)

While Citi Bike is not used as much as mass transit in NYC, it offered a way for city residents to move from A to B during the pandemic that wasn’t in an enclosed space…perhaps this is why it didn’t see as sharp a drop in demand compared to the NYC subway or buses

![](https://hackmd.io/_uploads/HkI3YYn7c.png)

### Temporal Analysis

We see increased usage in the summer as one might expect, but not all summer days prove to have high counts.

Labor Day 2019 shows reduced demand…and weather might also play an effect. We examine that later.

![](https://hackmd.io/_uploads/SkpMqK2m9.png){width=350px}![](https://hackmd.io/_uploads/B1Rs9tn7c.png){width=350px}

Surprisingly, weekends, especially Sunday, seem to have lower trip counts on average than weekday. Sunday truly is the day of rest. 

![](https://hackmd.io/_uploads/BJodoYnXc.png){width=350px}![](https://hackmd.io/_uploads/HJIV2jhQ5.png){width=350px}

Looks like commuters make up a bulk of trips given the high trip counts around 8am and 5pm. This also explains the reduced number of trips on weekends.

### Weather
We began by looking at daily average temperature and found that trip demand is highly correlated with it. We can use this as a model predictor!

![](https://hackmd.io/_uploads/rJcchi2Qc.png)

We wanted to investigate whether weather conditions might have an effect on number of trips…however, only saw decrease for days with precipitation (rain/snow)

Conditions like fog (all types), thunder, or haze were low in terms of number of days and did not have as strong an effect (although we were surprised by the positive effect)

![](https://hackmd.io/_uploads/ByVAno379.png)

Digging deeper into rain, we wanted to see if the amount of rain mattered…and it does! (as expected)

![](https://hackmd.io/_uploads/ByyeajnQc.png)

Based on this analysis, we decided to create a model incorporating time, average temperature, and amount of precipitation in order to predict the number of trips.

## Trip Demand Prediction Models

We attempted two models, the first of our models is the traditional SARIMA model, the second was a Long Short-Term Memory Recurrent Neural Network (LSTM-RNN). In this, we further distinguish the models by the time resolution, and whether or not the model was including weather data (i.e. had multidimensional inputs). Notably absent from our treatment is a vectorized SARIMA model or SVARIMA; however, this was due to time considerations and the fact that LSTM-RNN models on average demonstrate far better performance on multidimensional time series than traditional models and approaches. 

The population data is found in in the seconds resolution; however, at this resolution, observations are not continuously present and we have long gaps for many seconds of the year with zero ridership. We take a look at the hourly ridership data from 2019; whatever model we create for one year we can easily generalize to multiple years. 

As a preliminary assessment of the data, we look at the ridership over the year along different timescales--in doing so we can begin to understand trends and seasonality relationships within the day. Changing the scale of our data, as we will see, is akin to passing the "wave" of our time series through a low-pass filter, giving us lower frequency relationships. Therefore, if there is high seasonality at low timescale, this will be erased as we increase our timescale and aggregate over larger steps. 

![](https://hackmd.io/_uploads/Sk1BWhhm9.png)

![](https://hackmd.io/_uploads/B1Gsio3Qc.png)

![](https://hackmd.io/_uploads/HJsb3ohQ5.png)

We can see from the above graphs that weekly resolution is far too sparse to capture meaningful relationships. Therefore, we would like to build models that predict at the Hourly timescale if we can, and if not, then use the Daily timescale

At the sub hourly timescale, the data became too unwieldy and noisy for a years worth, let alone for the many years of data Citi Bike has available. However in future extensions of this project we would like to take a second level resolution for one week for one station and predict the ridership at that level. 

Our models were thus:

1. Daily SARIMA, which did not converge to parameters. 
2. Daily LSTM-RNN with weather, which had reduced RMS error.
3. Hourly LSTM-RNN without weather, which had great success. 

We compared these models by producing the Daily and Hourly RMSE and comparing them to find the RMSE minimizing model. 

Based on our results, we would believe that an hourly LSTM-RNN for a specific station, with weather, will be the best model, and this is the model we will investigate for further research. However, we found that including the weather data, while good at predicting trends in average ridership, is not as good at predicting the noise, which is only captured at the hourly level.

### Seasonal-differencing autoregressive integrated moving average (SARIMA)
```r 
# SARIMA(1 1 1 0 1 1 7) AIC= 7761.436  SSE= 53381920877  p-VALUE= 0.821252 
```
![](https://hackmd.io/_uploads/HJZv-Rhmq.png)

Above is the two week prediction for the two week ridership given the entire years worth of data. The line is our median prediction and the light blue section is the 85% confidence interval. We see that our prediction model has a SSE of 53381920877, giving us a RMSE of 231045.27 rides/day, 9626.88 rides/hour. This is our number to beat. 

### Long short-term memory recurrent neural network (LSTM)
**Intro to RNN and LSTM-RNN**

RNN
: _Recurrent Neural Network_: This is a type of Artificial Neural Network that can also update all of its weights through time. RNN's are extremely powerful techniques that can allow for short-term memory to be introduced into the model.  

LSTM
: _Long Short Term Memory_: This is an extension of RNN's that allow those RNN's to have long term memory and short term memory. While powerful, RNNs suffer from the drawback that, after too many time steps, the earliest information is almost entirely forgotten. Relevant information is essential to 

Cross-validation
: This is a technique where the dataset is split into not only training and testing, but the whole training set is split into batches and then split into into testing and validation sets. This is almost always done randomly, which proves to be effective when dealing with most data, as sampling biases are attenuated. However, with time series, randomly sampling the data is going to ruin the relationship with time that we are trying to predict. 

Backtesting
: Backtesting is Crossvalidation for Time Series models. Instead of randomly sorting the data into different batches, we take each batch of the data without disrupting the time series. Below is our Daily backtesting strategy. 

**Daily LSTM-RNN (with Weather) Backtesting Strategy**

![](https://hackmd.io/_uploads/rJGjSP0m9.png)

Above we see the backtesting split and the train test split for each backtest slice. We can see the progression of each of the splits through the year. Zooming into each split, we have: 

![](https://hackmd.io/_uploads/BymbLDCQc.png)

**Daily LSTM-RNN (with Weather) Predictions**

First, let us take a look at how the Temperature and Average Precipitation data influences the ridership number. Below we see the Ridership, Average Temperature, and Average Rainfall over the entire year of 2019.

![](https://hackmd.io/_uploads/HyDgtw0mc.png)

Zooming into the January, we can see that there is clear monthly, and weekly seasonality. Furthermore, there seems to be a direct correlation between average temperature and ridership, and an inverse correlation between precipitation and ridership. 

![](https://hackmd.io/_uploads/HysYKvAXq.png)

We can see the predictions over each of the backtesting slices to get an idea of how our model looks like after training, and without early-stopping to correct for overfitting. Below, we see each slice with their prediction, and each RMSE value. 

![](https://hackmd.io/_uploads/r1kddDCXc.png)

While these predictions don't look particularly impressive against our data, keep in mind that a lot of the jaggedness of the data comes from the fact that total ridership can jump drastically between days for a number of factors. The mean RMSE was 15197 bikes/day  +/- 6097 bikes/day as the standard deviation. This gives us a hourly RMSE of 633.21 bikes/hour on average. Thus, moving to an LSTM model with weather drastically improved our daily RMSE, and this is a significant enough improvement to warrant exploring the LSTM approach further. 


**Hourly LSTM-RNN Backtesting Strategy (without Weather) and Predictions**

![](https://hackmd.io/_uploads/HJtZhDA75.png)

Going down to the hourly timescale, without including the weather data, we have already seen that the full years worth of data is extremely noisy. Looking at our backtesting strategy, we can see that reflected. Zooming into each month, we can see that there is a sub-daily seasonality to the data that we have been unable to capture. 

![](https://hackmd.io/_uploads/H1pJsuAQc.png)

Fitting an LSTM model to these, we reach the following backtesting predictions.

![](https://hackmd.io/_uploads/BkRmPVJVc.png)

We have an average RMSE of 1117.191 rides/hour, giving us a daily RMSE of 26812.58 rides/day. We can see that these models look much more predictive of the oscillations in the data; however, we also note that the calculated the daily and hourly errors naively and the resolution of the prediction we get is much stronger in the hourly timescale.  

### Conclusion
Our exploratory data analysis informed our model feature selection and we attempted two time-series models (SARIMA and LSTM) to predict trip demand on both an hourly and daily basis. We found that hourly models were better overall. Further improvements to the model would be in reducing the root mean square error in predictions and, perhaps, incorporating additional predictors. For future work, we would like to build an LSTM model for each station on the hourly timescale accounting for weather. From our study, we believe that this would give us the strongest predictive accuracy for our model. 

Separately, we analyzed bike rebalancing. You can read the full write-up here.
