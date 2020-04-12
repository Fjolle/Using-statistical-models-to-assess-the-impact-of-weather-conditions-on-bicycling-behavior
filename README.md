#*Overview*

  Bike sharing systems have ploriferated in cities around the world as one of the most environmental friendly transport modes. These digital bike-sharing platforms powered by modern technologies have found use particularly among millenials who are increasingly more aware of the environment and health,  tech-savy, and appreciative of instant gratification offered. The city of London has one of the largest digital bike-sharing systems in the World known as Santander bikes with 783 docking stations as of today and 10 million annual bike trips. Transport for London (TfL) has made data on all public transport modes, including digital bike-sharing open through an API. In the following, I will use the bike-jouney data in conjuction with weather data to analyze, vizualize and provide insights on bike journeys in London throughout 2019 and analyze the impact of weather on the use of the London bikeshare system. 

I provide the codes and technical details for generating the insights below [here](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/tfl_project_copy2.ipynb)

##Data

  TfL makes available the historical bikesharing usage data (e.g. start and end time of the bike trip, bike id, and station name) in the form of csv files in the following [website](https://cycling.data.tfl.gov.uk). In addition, I use the [bike point API](https://api.tfl.gov.uk/swagger/ui/index.html?url=/swagger/docs/v1#!/BikePoint/BikePoint_GetAll) to add real time information on bike docks across London, including latitute, longitutde, capacity, and available bikes. The bike usage dataset includes 10,138,355 bike trips from Janaury 1, 2019 to December 31, 2019 with attributes for bike trip duration, start trip date and time, end trip date and time, start station id and end station id as in the table below. Hourly weather data from Janaury 1, 2019 to December 31, 2019 for temperature, precipitation, humidity, wind speed, and visibility were obtained from the [Dark Sky API](https://darksky.net/dev/account).

**Exploratory Data Analysis of Bike Journeys**

  I begin the analysis with the discovery of some general trends in the data which will inform the hypothesis creation and guide the analysis. 

  Here I illustrate the ten largest bike stations in London in terms of bike capacity which is obtained by sorting the dataset by top counts and truncating it.

![Biggest bike stations](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/Unknown-2.png)

  There are two stations that stand out as dominant sources of starting bike trips during the weekdays, that is King's Cross railway station and the London Waterloo railstation. In contrast, during weekends, most stations show little activitiy in terms of bike trips compared to weekdays, with the exception of Hyde Park and Kensington Gardens to a certain extent.Stations around Hyde Park and Kensignton Gardens are entertainment hubs for Londoners, explaining the large bike traffic on the weekend.To illustrate this, I aggregate the number of rentals per station, and segment bike rentals by weekday and weekend to obtain the graph below

![Bike journeys by weekday and weekend](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/bike%20trips%20by%20weekday%20and%20weekend.png)

  This pattern is not surprising, King's Cross and Waterloo being large and important stations in the London transporation are dominant sources of bikes in the morning, whereas Bank, Holborn, Liverpool, and Soho stations are the largest receiving bike stations in the morning. These stations are located in the city center where activity peaks in the early morning hours, including the finance center and the Westminister government buildings. This relationship inverses in the evenings, when the two large railway stations become the largest receiving bike stations and the city center stations the largest source stations.This is best illustrated by the two bar race charts below which depict the top source and receiving stations by time of the day. 

![Bar race chart 1](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/animation.gif)
![Bar race chart 2](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/animation2.gif)

  Distribution charts are a useful way to gain a comprehensive understaning of trends and outliers over time. The  graph below illustrates the distribution of bike rentals by day of the week and hour of the day. A distinct pattern can be seen between bike usage during the weekend and  weekdays. During  weekdays, bike usage is highest between 7 and 9 in the morning and between 17 and 20 in the afternoon. Weekend bike usage peaks between noon and 14 in the afternoon. This pattern of bike usage could be explained by the fact that weekday bike usage is mostly associated with commuting to and from work, whereas weekend usage is mostly associated with midday leisure trips.   

![Distribution](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/distribution_bike_trips.png)

  One issue that impacts biking as a mode of transporation that doesn't impair other modes of transporation to the same extent, is weather conditions such as rainfall, snow, humidity, wind, fog and extreme temperatures. The research below analyzes the impact of weather on the use of London Santander bikeshare system. 

  For the dependent variable analyzed, there was an average 2266 trips per hour with a standard deviation of 1304. The average trip duration was 19.6 minutes with a range of as short as 8 minutes to 514 minutes. Independent variables included both weather related variables and non-weather related control variables. London recorded a wide range of temperatures during 2019 spanning from -3.2 °C to 37 °C. The average humidity is London during 2019 was 65% and a standard deviation of 17%. The average wind speed was 8 MPH defined as a "gentle breeze". Rain in London was observed 16% of the year. 

**Preliminary analysis of the relationship between bike journeys and weather**

  The relationship between daily number of bike trips and average daily temperature is depicted in the figure below. Bike trips pick up as the temperatures increase. We can notice a few outlier days explained by adverse weather impacts that day. June 12, 2019, for exmaple, only saw xx trips, because ... A drop in bike ridership can be seen during the period of extreme heat between July xx, 2019 and August xx, 2019. Days with low ridership can also be explained for reasons other than weather, such as only xx trips on Christmas Day.  

![correlation](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/relationship%20between%20bike%20usage%20and%20temperature.png)

  While a mean of 28,398 and median of 28,872 trips per day are made for all the users, in the rain, the average number of trips per day drops to 23,262. The differences between bike trips and temperature between rainy and non-rainy days can be seen in the scatter plot below. Given the genereous amount of cold rainy days in London it does not surprise to observe a reduced numbers of bike trips during colder months. There is also a trend that we observe where during rainy days where daily bike trips show a larger oscilliation around the mean. It again suggests that, but does not prove, that on rainy days a fair share of London bikesharing customers who are more elastic to "perturbations" from cold and rain choose alternative modes of transporation. We cannot conclude this definitelvely relying solely on the scatter plot since we haven't controlled for a number of variables that could impact the decline in bike trips during rainy days. We discipline this investigation more below by using a statistical method to prove this hypothesis. 

![correlation](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/scatterplot.png)

  Hourly weather observations for temperature, rain, and humidity are merged with the hourly bike usage data. Bike trips were collapsed by hour, obtaining the sum of bike trips and average duration of trips per hour. I use a statistical model to analyze the bicycling behavior under different weather conditions in London while controlling for a range of variables. Average trip per hour was calculated by dividing hourly duration by hourly trips. Dummy variables were created for weather events (rain above the mean, humidity above the mean, wind above the mean). Temperature was also recorded into 5-degree-bins and converted to dummy variables. Dummy variables were also created for month between January-December 2019. We analyze the relationship between average trip duration (dependent variable) and weather (independent variable) using an ordinary least squares regression model. 
  
![OLS regression results](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/OLS%20regression%20results.png)
