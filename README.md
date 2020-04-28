### Article Outline  
1. [Introduction](#introduction) 
2. [Data](#data) 
3. [Exploratory Data Analysis of Bike Journeys](#exploratory)
4. [A View of the most Frequented Bike Stations](#standout)
5. [Bar Race for Source and Destination Stations](#race)
6. [Bike Trips during Weekday and Weekend](#distribution)
7. [Preliminary Analysis of the Relationship between Bike Journeys and Weather](#Preliminary)
8. [Impact of Heavy Showers on Bicycling Behavior](#rainy)
9. [Approach to Analysis](#approach)
9. [Descriptive to Predictive](#predictive)
10. [Interpretation of OLS model results](#ols)
11. [Evaluation of the Poisson model](#poisson)
12. [Evaluation and interpretation of the Negative Binomial model results](#binomial)
13. [Model Evaluation on Test Data Set](#evaluation)

<a name="introduction"></a>
### Introduction

Bike sharing systems have proliferated in cities around the world as one of the most environmental friendly transport modes. These digital bike-sharing platforms powered by modern technologies have found use particularly among millennials who are increasingly more aware of their environment and health, are tech-savvy, and appreciative of instant gratification offered by the ultra-convenience of bikes. The city of London has one of the largest digital bike-sharing systems in the world operated by Santander with 783 bike docking stations as of today and 10 million annual bike trips. Transport for London (TfL) has made data on all public transport modes, including digital bike-sharing available through an API. In the following blog, I will use the bike-journey data in conjunction with weather data to generate insights on bicycling behavior in London  and fit an OLS, Poisson, and Negative Binomial machine learning model that accurately predicts the impact of weather on the use of the London bike-sharing system. I analyze the influence of significant factors that affect bicycling. After training the model, I also test the model's strength on generalizing to an unseen dataset. 

#### Dataset and Code
[Click here for bike, weather data and code](https://github.com/albagjonbalajdc/Using-statistical-models-to-assess-the-impact-of-weather-conditions-on-bicycling-behavior)

<a name="data"></a>
### Data

Transport for London (TfL) makes available the historical bike-sharing usage data (with attributes for bike trip duration, start trip date and time, end trip date and time, start station id and end station id) in the form of CSV files in the TfL website. In addition, I use the Bike Point API to add real time information on bike docks across London, including latitude, longitude, capacity, and available bikes. The bike usage dataset includes over 10 million bike trips from Janaury 1, 2019 to December 31, 2019. Hourly weather data from Janaury 1, 2019 to December 31, 2019 for temperature, precipitation, humidity, wind speed, and visibility were obtained from the Dark Sky API.
  
_Codes and technical details for obtaining the data can be found here_ [Preparing the data](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Preparing%20the%20data.ipynb)

<a name="exploratory"></a>
### Exploratory Data Analysis of Bike Journeys

I begin the analysis with the discovery of some general trends in the data which will inform the hypothesis creation and guide the analysis. Codes and technical details for obtaining the figures below can be found here [Visualizations](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Visualization.ipynb)

<a name="stations"></a>
### A View of the most Frequented Bike Stations

There are two stations that stand out as dominant sources of bike trips during weekdays, that is King's Cross railway station and the London Waterloo rail-station. In contrast, during weekends, most stations show little activity in terms of bike trips compared to weekdays, with the exception of Hyde Park and Kensington Gardens to a certain extent. Stations around Hyde Park and Kensignton Gardens are entertainment hubs for Londoners, explaining the large bike traffic on the weekend. To illustrate this, I aggregate the number of bike rentals per station, and segment rentals by weekday and weekend to obtain the graph below.

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/bike%20counts%20by%20station.png">
</p>

This pattern is not surprising, King's Cross and Waterloo being large and important stations in the London transportation network are dominant sources of bikes in the morning, whereas Bank, Holborn, Liverpool, and Soho stations are the largest receiving bike stations in the morning. These stations are located in the city center where activity peaks in the early hours, including the finance center and the West-minister government buildings. This relationship inverses in the evenings, when the two large railway stations become the largest receiving bike stations and the city center stations the largest source stations. This is best illustrated by the two bar race charts below which depict the top source and receiving stations by time of the day.

<a name="race"></a>
### Bar Race for Source and Destination Stations

![Bar race chart 1](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/animation.gif)
![Bar race chart 2](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/animation2.gif)

<a name="distribution"></a>
### Bike Trips during Weekday and Weekend

Distribution charts are a useful way to gain a comprehensive understanding of trends and outliers over time. The graph below illustrates the distribution of bike rentals by weekday and hour. A distinct pattern can be seen between bike usage during the weekend and weekdays. During weekdays, bike usage is highest between 7am and 9am and between 5pm and 8pm. Weekend bike usage peaks between 12pm and 2pm. This pattern of bike usage could be explained by the fact that weekday bike usage is mostly associated with commuting to and from work, whereas weekend usage is mostly associated with midday leisure and recreational trips.   

![Distribution](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/distribution%20of%20bike%20trips%20by%20day%20and%20hour.png)

<a name="Preliminary"></a>
### Preliminary Analysis of the Relationship between Bike Journeys and Weather 

One factor that impacts biking as a mode of transportation that doesn't impair other modes of transportation to the same extent, is weather conditions such as: rainfall, snow, humidity, wind, and extreme temperatures. The research below analyzes the impact of weather conditions  on the use of London Santander bike-sharing system. For the dependent variable analyzed, there was an average of 2266 trips per hour with a standard deviation of 1304. The average trip duration was 19.6 minutes with a range as short as 8 minutes to 514 minutes. Independent variables included both weather related variables and non-weather related control variables. London recorded a wide range of temperatures during 2019 spanning from -3.2°C to 37°C. The average humidity in London during 2019 was 65% with a standard deviation of 17%. The average wind speed was 8 MPH defined as a "gentle breeze”.

| Dependent Vairable            | Mean                    | Std.Dev                  |Min                 |Max            |     
|:-------------                 |:------------:           | :-------------:          |:-------------:     |:-------------:| 
| Trips per hour                | 1179                    | 1129                     |6                   | 5377          |     
| Avg trip duration per hour    | 16.8                    | 4                        |8.11                | 70            | 
| Average temperature per hour  | 12.2                    | 6                        |-2.95               | 36.9          | 
| Relative Humidity             | 0.72                    | 0.45                     |0                   | 1             | 
| Relative Wind Speed           | 0.44                    | 0.5                      |0                   | 1             |
| Visibility                    | 0.92                    | 0.28                     |0                   | 1             |
| Peak travel hours             | 0.29                    | 0.45                     |0                   | 1             |
| Weekend                       | 0.29                    | 0.45                     |0                   | 1             |
| Holidays                      | 0.02                    | 0.14                     |0                   | 1             |

The relationship between daily number of bike trips and average daily temperature is depicted in the figure below. Bike trips pick up as the temperatures increase, and reach the peak during the warmest months, and decelerate gradually as it cools off during Autumn and Winter period. We can notice a few outlier days explained by adverse weather impacts that day. June 10, 2019, for example, only saw 13,223 trips, from 28,898 trips in the previous day due to heavy showers that day in London. Days with low ridership can also be explained for reasons other than weather, such as only 5,583 trips on December 26,2019, the day after Christmas Day. Surprisingly, against my expectation, we notice a large increase in bike ridership during the period of extreme heat between July 2019 and August 2019, something that we investigate in the regression analysis below by controlling for the impact of season on bike ridership.

![correlation](https://github.com/albagjonbalajdc/Using-statistical-models-to-assess-the-impact-of-weather-conditions-on-bicycling-behavior/blob/master/Plots/regression_correlation.png)

<a name="rainy"></a>
### Impact of Heavy Showers on Bicycling Behavior


| Avg bike trips/day     | Avg bike trips/rainy day| 
|:-------------          |:-------------           | 
| 28,389                 | 23,262                  | 

A mean of 28,398 and median of 28,872 trips per day are taken for all the users, while in the rain, the average number of trips per day drops to 23,262. The differences between bike trips and temperature between rainy and non-rainy days can be seen in the scatter plot below. Given the generous amount of cold rainy days in London it does not surprise to observe a reduced numbers of bike trips during colder months. There is also a trend that we observe where during rainy days daily bike trips show a larger oscillation around the mean. It again suggests that, but does not prove, that on rainy days a fair share of London bike-sharing users who are more elastic to "perturbations" from cold and rain choose alternative modes of transportation. We cannot conclude this definitively relying solely on the scatter plot since we haven't controlled for a number of variables that could impact the decline in bike trips during rainy days. We discipline this investigation more below by using a range of statistical models to prove this hypothesis.
               
![rainy](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/scatterplot_temp_bike_trips.png)

<a name="approach"></a>
### Approach to Analysis

_Codes and technical details for running the regressions below can be found here_ [Regression Analysis](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Regression.ipynb)

Hourly weather observations for temperature, rain, humidity, and visibility are merged with the hourly bike usage data. Bike trips were collapsed by hour, obtaining the sum of bike trips and average duration of trips per hour, which leaves us with 8565 data rows of observation. I use statistical models to analyze the bicycling behavior under different weather conditions while controlling for a range of variables. Average trip duration per hour was calculated by dividing hourly duration by hourly trips. Dummy variables were created for weather events (precipitation above the mean, humidity above the mean, wind speed above the mean, visibility above the mean, holidays/weekend, and peak travel hours). Temperature was also recorded into 5-degree-bins and converted to dummy variables. Dummy variables were also created for month between January-December 2019. January 2019 served as a reference group for the month dummy. Temperature 10-15°C served as a reference group for temperature bin dummy variables.

Two dependent variables are analyzed: (1) average trip duration and (2) number of bike trips. To analyze the relationship between average trip duration (dependent variable) and weather conditions (independent variable) I perform the ordinary least squares regression model (OLS). To analyze the impact of weather on number of bike trips, I perform a Poisson and Negative Binomial regression model. Estimates of the parameters in the average trip duration model are used to determine the change in trip duration in minutes associated with each parameter. In the negative binomial regression, the parameters represent the incremental change in bike trips frequency associated with each unit change in the parameters. The regression results from the OLS model are shown in Table 1. The Poisson and Negative Binomial regression model of number of trips is shown in Table 2 and Table 3. 

### <a name="predictive"></a>
### Descriptive to Predictive
### <a name="ols"></a>
#### Interpretation of OLS model results

Temperature enters the OLS and the Negative Binomial regression as a dummy variable in 5°C ranges as we don't expect the relationship between temperature and cycling behavior to be linear. Coefficients show that temperatures between -3°C and through the 10°C range are all highly significantly correlated (p<.01) with shorter average trip duration compared to when the temperature is in the 10°C-15°C range, ceteris paribus. When temperatures range between -3°C and 10°C, average trip times are 12.5 minutes as opposed to 17.8 minutes, holding all the other variables constant. Temperatures in the range 20°C-35°C were positively correlated with increasing trip durations, and are highly significant (p<.01). Average trip times are 14 minutes longer per trip when the temperatures range between 20°C-35°C, and trip duration increases the most when the temperatures are above 30°C.

#### Table 1. OLS regression summary
<p align="center"> 
<img src="https://github.com/albagjonbalajdc/Using-statistical-models-to-assess-the-impact-of-weather-conditions-on-bicycling-behavior/blob/master/Plots/OLS%20regression%20results.png">
</p>

Controls for month of the year are included in both models, in order to control for the impact of the seasonal pattern on the temperature. From these results, it is clear that there is variation in length of trips dependent on temperature with both decreasing when it gets colder and increasing as it gets warmer. Other variables also show association with bicycle usage and trip duration. Parameter estimates of humidity, rainfall, and wind speed are statistically significant and negative.The magnitude in trip duration for humidity, rainfall, and wind speed is -1.42 minutes, -1.37 minutes, and -0.60 minutes per trip, respectively. Reduction in trip duration during rainfall are less than in very cold temperatures. The impact of higher wind speed is considerably less than that of other weather conditions. Another control variable is a dummy for peak travel times which shows that bike trips are shorter than off-peak times by 1.3 minutes per trip. Bike trip duration during weekends and holidays is statistically different than on weekdays, and trips are longer by 2.9 and 5.9 minutes per trip, respectively. The later confirms the hypothesis that during weekend/holidays trips are more recreational in nature.

### <a name="poisson"></a>
#### Evaluation of the Poisson model

Now we turn to the analysis of the dependent variable on bike trip counts. For our bike trip counts data an appropriate model is a discrete probability distribution, of which Poisson distribution is the simplest. We consider the count of bike trips per hour to be a random variable from a Poisson distribution whose mean rate is influenced by a range of variables same as in the OLS model above (Table 1). When modeling count data, an OLS regression is not appropriate since it requires the dependent variable to be a continuous quantity without limits on it range. However, count data cannot be negative and thus discrete probability distribution is a better fit. We begin by applying the Poisson regression. One important assumption of the Poisson distribution is that the variance of a random variable is equal to the mean. This should also be true for the data on count of bike trips if the data follows a Poisson distribution. In cases when the variance exceeds the mean indicating over-dispersion, the data violates the Poisson distribution assumptions, leading to underestimated standard errors. This can arise when variables with important explanatory power are excluded from the model for whatever reason. The regression could also suffer from under-dispersion, that is when the variance is smaller than the mean, but this is observed less commonly. The output from the Poisson regression is shown in the table below. 

The output from the Poisson regression is shown in the table below.

#### Table 2. Poisson regression summary

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/Using-statistical-models-to-assess-the-impact-of-weather-conditions-on-bicycling-behavior/blob/master/Plots/Poisson%20regression%20results.png">
</p>

We determine the adequacy of the Poisson regression using the indicators in the upper part of Table 2. Specifically, we are interested in the residuals, which is an indication of the amount by which independent fitted observations differ from their real means. The sum of the squares of the residuals is used to assess the fit of the model, specifically the Pearson chi2 in the lower right side of the upper table. To better understand how well the Poisson model has fitted the bike data, we plot the Pearson residuals against their fitted means in the figure below. When the data follows a Poisson distribution the ratio Pearson chi2/Df residuals is approximately 1. A ratio below 1 indicates under-dispersion and more than 1 implies over-dispersion. This figure shows that the Poisson model is a poor fitting model for our bike data since the majority of the data points is largely dispersed between -50 and 90. The Pearson chi2/Df residuals ratio in our model is 467, suggesting over-dispersion. 

#### Poisson residuals and dispersion

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/Residual%20vs%20Mean%20(Poisson).png">
</p>

The low performance of the Poisson model proved inadequate since our data violates the mean equals variance assumption. Therefore, we need a model that does not make the equi-dispersion assumption. One such model in the family of the Poisson regression is the Negative Binomial regression to which we turn now.

### <a name="binomial"></a>
#### Evaluation and interpretation of the Negative Binomial model results

The Negative Binomial regression uses a continuous positive dispersion parameter, α which specifies the extent by which the distributions variance exceeds the mean. We can express the variance from the model with the equation σ2=μ+αμ^2. We need to specify the parameter α in the model. To do so, we follow the Cameron and Trivedi test for under- or over-dispersion of a Poisson model. To perform this test we take the fitted means μ from the Poisson regression and perform an OLS regression without an intercept using the equation ((yi−μi)2−yi)/μi=αμi+ϵi, where the left-hand side is the independent variable, α is the unknown coefficient and ϵ is the error term. We will prove below that the estimated α term is a more reliable indicator than the Pearson statistic in the Poisson regression.
  
#### Table 3. Negative Binomial regression summary

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/Using-statistical-models-to-assess-the-impact-of-weather-conditions-on-bicycling-behavior/blob/master/Plots/Negative%20Binomial%20regression%20results.png">
</p>

The information in the upper section of Table 3 indicates a substantially better fit using the Negative Binomial regression compared to Poisson regression implied by a larger value for the log-likelihood and a substantially lower Pearson chi2 / Df Residuals ratio, 2.25.

Similarly, the scatterplot between fitted mean and Pearson residuals shows substantially lower dispersion, where the maximum value for the standardized deviance residual is 17.5 as compared to 200 for the Poisson regression. The model still has room for improvement and that would require better predictors. 

#### Negative Binomial residuals and dispersion

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/Residual%20vs%20Mean%20(Negative%20Binomial).png">
</p>

### <a name="evaluation"></a>
#### Model Evaluation on Test Data Set

We use the model trained on the 80% of the dataset to make predictions and determine how well it fits the real data on the 20% of the dataset and generate the graph below between predicted mean bike trips and the actual mean bike trips. Here we can notice that the predicted means do not follow the actual means and therefore the model was not able to generalize very well.

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/Predicted%20vs%20Actual.png">
</p>

Parameters estimates for temperature in the Negative Binomial regression model are highly statistically significant (p<.01)  and show that bike trips decrease as temperatures decrease, but also decrease above 30°C, relative to the reference category (10°C-15°C range). We calculate the Incident Rate Ratios (IRR) by exponentiating the parameters of the model, which represent the incremental change in trip frequency associated with each unit change in the parameters. Parameter estimates for Humidity and Rain are also negative in this model and statistically significant. Bike trip frequency is reduced by 0.9 when it is raining and by and 0.5 when it is very humid. The dummy control for peak travel times shows that bike usage is higher during peak hours by 3.4 bike trips. Usage on weekends and holidays statistically different from weekdays is associated with a reduction in bike trips by 0.86 and 0.82.
