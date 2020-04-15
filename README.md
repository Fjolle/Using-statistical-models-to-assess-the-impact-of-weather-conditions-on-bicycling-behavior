### Table of Contents  
1. [Overview](#overview) 
2. [Data](#data) 
3. [Exploratory Data Analysis of Bike Journeys](#exploratory)
4. [A View of the most Frequented Bike Stations](#standout)
5. [Bar Race for Source and Destination Stations](#race1)
6. [Bike Trips during Weekday and Weekend](#distribution)
7. [Preliminary Analysis of the Relationship between Bike Journeys and Weather](#Preliminary)
8. [Impact of Heavy Showers on Bicycling Behavior](#rainy)
9. [Approach to Analysis](#approach)
9. [Descriptive to Predictive](#Predictive)

<a name="overview"></a>
### Overview

Bike sharing systems have ploriferated in cities around the world as one of the most environmental friendly transport modes. These digital bike-sharing platforms powered by modern technologies have found use particularly among millenials who are increasingly more aware of the environment and health,  tech-savy, and appreciative of instant gratification offered. The city of London has one of the largest digital bike-sharing systems in the World known as Santander bikes with 783 docking stations as of today and 10 million annual bike trips. Transport for London (TfL) has made data on all public transport modes, including digital bike-sharing open through an API. In the following, I will use the bike-jouney data in conjuction with weather data to vizualize and provide insights on bicycling behavior in London and analyze the impact of weather on the use of London bikesharing system. 

<a name="data"></a>
### Data

TfL makes available the historical bikesharing usage data (with attributes for bike trip duration, start trip date and time, end trip date and time, start station id and end station id) in the form of csv files in the following [website](https://cycling.data.tfl.gov.uk). In addition, I use the [Bike Point API](https://api.tfl.gov.uk/swagger/ui/index.html?url=/swagger/docs/v1#!/BikePoint/BikePoint_GetAll) to add real time information on bike docks across London, including latitute, longitutde, capacity, and available bikes. The bike usage dataset includes over 10 million bike trips from Janaury 1, 2019 to December 31, 2019. Hourly weather data from Janaury 1, 2019 to December 31, 2019 for temperature, precipitation, humidity, wind speed, and visibility were obtained from the [Dark Sky API](https://darksky.net/dev/account).
  
_Codes and technical details for obtaining the data can be found here_ [Preparing the data](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Preparing%20the%20data.ipynb)

<a name="exploratory"></a>
### Exploratory Data Analysis of Bike Journeys

I begin the analysis with the discovery of some general trends in the data which will inform the hypothesis creation and guide the analysis. Codes and technical details for obtaining the figures below can be found here [Visualizations](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Visualization.ipynb)

<a name="stations"></a>
### A View of the most Frequented Bike Stations

There are two stations that stand out as dominant sources of bike trips during the weekdays, that is King's Cross railway station and the London Waterloo railstation. In contrast, during weekends, most stations show little activitiy in terms of bike trips compared to weekdays, with the exception of Hyde Park and Kensington Gardens to a certain extent.Stations around Hyde Park and Kensignton Gardens are entertainment hubs for Londoners, explaining the large bike traffic on the weekend.To illustrate this, I aggregate the number of rentals per station, and segment bike rentals by weekday and weekend to obtain the graph below

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/bike%20counts%20by%20station.png">
</p>

This pattern is not surprising, King's Cross and Waterloo being large and important stations in the London transporation are dominant sources of bikes in the morning, whereas Bank, Holborn, Liverpool, and Soho stations are the largest receiving bike stations in the morning. These stations are located in the city center where activity peaks in the early morning hours, including the finance center and the Westminister government buildings. This relationship inverses in the evenings, when the two large railway stations become the largest receiving bike stations and the city center stations the largest source stations.This is best illustrated by the two bar race charts below which depict the top source and receiving stations by time of the day. 

<a name="race1"></a>
### Bar Race for Source and Destination Stations

![Bar race chart 1](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/animation.gif)
![Bar race chart 2](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/animation2.gif)

<a name="distribution"></a>
### Bike Trips during Weekday and Weekend

Distribution charts are a useful way to gain a comprehensive understaning of trends and outliers over time. The  graph below illustrates the distribution of bike rentals by day of the week and hour of the day. A distinct pattern can be seen between bike usage during the weekend and weekdays. During  weekdays, bike usage is highest between 7 and 9 in the morning and between 17 and 20 in the afternoon. Weekend bike usage peaks between noon and 14 in the afternoon. This pattern of bike usage could be explained by the fact that weekday bike usage is mostly associated with commuting to and from work, whereas weekend usage is mostly associated with midday leisure trips.   

![Distribution](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/distribution%20of%20bike%20trips%20by%20day%20and%20hour.png)

<a name="Preliminary"></a>
### Preliminary Analysis of the Relationship between Bike Journeys and Weather 

One factor that impacts biking as a mode of transporation that doesn't impair other modes of transporation to the same extent, is weather conditions such as rainfall, snow, humidity, wind, and extreme temperatures. The research below analyzes the impact of weather on the use of London Santander bikesharing system. 

For the dependent variable analyzed, there was an average 2266 trips per hour with a standard deviation of 1304. The average trip duration was 19.6 minutes with a range of as short as 8 minutes to 514 minutes. Independent variables included both weather related variables and non-weather related control variables. London recorded a wide range of temperatures during 2019 spanning from -3.2°C to 37°C. The average humidity in London during 2019 was 65% with a standard deviation of 17%. The average wind speed was 8 MPH defined as a "gentle breeze".

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

The relationship between daily number of bike trips and average daily temperature is depicted in the figure below. Bike trips pick up as the temperatures increase, and reach the peak during the warmest months, and decelerate gradually as it cools off during Autumn and Winter period. We can notice a few outlier days explained by adverse weather impacts that day. June 10, 2019, for exmaple, only saw 13,223 trips, from 28,898 trips in the previous day due to heavy showers that day in London. Days with low ridership can also be explained for reasons other than weather, such as only 5,583 trips on December 26,2019, the day after Christmas Day.  Surprisingly, against my expectation, we notice a large increase in bike ridership during the period of extreme heat between July 2019 and August 2019, something that we investigate in the regression analysis below by controlloing for the impact of season on bike ridership.

![correlation](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/daily%20scatterplot%20between%20temperature%20and%20bike%20trips.png)

<a name="rainy"></a>
### Impact of Heavy Showers on Bicycling Behavior


| Avg bike trips/day     | Avg bike trips/rainy day| 
|:-------------          |:-------------           | 
| 28,389                 | 23,262                  | 


While a mean of 28,398 and median of 28,872 trips per day are made for all the users, in the rain, the average number of trips per day drops to 23,262. The differences between bike trips and temperature between rainy and non-rainy days can be seen in the scatter plot below. Given the genereous amount of cold rainy days in London it does not surprise to observe a reduced numbers of bike trips during colder months. There is also a trend that we observe where during rainy days daily bike trips show a larger oscilliation around the mean. It again suggests that, but does not prove, that on rainy days a fair share of London bikesharing users who are more elastic to "perturbations" from cold and rain choose alternative modes of transporation. We cannot conclude this definitelvely relying solely on the scatter plot since we haven't controlled for a number of variables that could impact the decline in bike trips during rainy days. We discipline this investigation more below by using a statistical method to prove this hypothesis. 
               
![rainy](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/scatterplot.png)

<a name="approach"></a>
### Approach to Analysis

_Codes and technical details for running the regressions below can be found here_ [Regression Analysis](https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Regression.ipynb)

Hourly weather observations for temperature, rain, humidity, and visibility are merged with the hourly bike usage data. Bike trips were collapsed by hour, obtaining the sum of bike trips and average duration of trips per hour, which leaves us with 8565 data rows of observation. I use a statistical model to analyze the bicycling behavior under different weather conditions while controlling for a range of variables. Average trip duration per hour was calculated by dividing hourly duration by hourly trips. Dummy variables were created for weather events (precipitation above the mean, humidity above the mean, wind speed above the mean, visibility above the mean, holidays/weekend, and peak travel hours). Temperature was also recorded into 5-degree-bins and converted to dummy variables. Dummy variables were also created for month between January-December 2019. January 2019 served as a reference group for the month dummy. Temperature 10-15°C served as a reference group for temperature bin dummy variables.
  
Two dependent variables are analyzed: (1) average trip duration and (2) number of bike trips. To analyze the relationship between average trip duration (dependent variable) and weather conditions (independent variable) I perform the ordinary least squares regression model (OLS). To analyze the impact of weather on number of bike trips, I perform a logistic regression model. Estimates of the parameteres in the average trip duration model are used to determine the change in trip duration in minutes associated with each parameter. In the logistic regression, each estimate is associated with a xx percent change in number of bike trips. The regression results from the OLS model are shown in Table 1. The Logistic regression model of number of trips is shown in Table 2. 

### <a name="Predictive"></a>
### Descriptive to Predictive

Temperature enters the regression as a dummy variable in 5°C ranges as we don't expect the relationship between temperature and cycling behavior to be linear. Coefficients show that temperatures between -3 and through the 10°C range are all highly significantly correlated (p<.01) with shorter average trip duration compared to when the temperature is in the 10-15°C range, ceteris paribus. When temperatures range between -3 and 10°C, average trip times are 12.5 minutes as opposed to 17.8 minutes, holding all the other variables constant. Temperatures in the range 20-35°C were positively correlated with increasing trip durations, and are highly significant (p<.01). Average trip times are 14 minutes longer per trip when the temperatures range between 20-35°C, and trip duration increases the most when the temperatures are above 30°C.

<p align="center"> 
<img src="https://github.com/albagjonbalajdc/A-model-of-bike-journeys-and-weather/blob/master/Plots/OLS%20regression%20results.png">
</p>

Controls for month of the year are included in both models, in order to control for the impact of the seasonal pattern on the temperature. From these results, it is clear that there is variation in both amount of usage and the length of trips dependent on temperature with both decreasing when it gets colder and increasing as it gets warmer. Other variables also show association with bicycle usage and trip duration. Parameter estimates of humidity, rainfall, and wind speed are statistically significant and negative in [both] models.The magnitude in trip duration for humidity, rainfall, and wind speed is -1.42 minutes, -1.37 minutes, and -0.60 minutes per trip, respectively. Reduction in trip duration during rainfall are less than in very cold temperatures. The impact of higher wind speed is considerably less than that of other weather conditions. Another control variable is a dummy for peak travel times which shows that bike trips are shorter than off-peak times by 1.3 minutes per trip. Bike trip duration during weekends and holidays is statistically different than on weekdays, and trips are longer by 2.9 and 5.9 minutes per trip, respectively. The later confirms the hypothesis that during weekend/holidays trips are more recreational in nature.
