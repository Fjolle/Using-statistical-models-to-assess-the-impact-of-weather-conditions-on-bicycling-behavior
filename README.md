# **Overview**

Bike sharing systems have ploriferated in cities around the world as one of the most innovative transport modes. These digital bike-sharing platforms powered by modern technologies have found use particularly by the millenials who are increasingly more aware of the environment and health,  tech-savy, and in demand of instant gratification. The city of London has one of the largest digital bike-sharing systems in the World known as Santander bikes with 783 docking stations as of today and 10 million annual bike trips. Transport for London (TfL) has made data on all public transport modes, including digital bike-sharing open through an API. In the following, I will use the bike-jouney data in conjuction with weather data to analyze, vizualize and provide insights on bike journeys in London throughout 2019 and analyze the impact of weather on the use of the London bikeshare system. 

I provide the codes and technical details for generating the insights below [here](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/tfl_project_copy2.ipynb)

# **Data**

TfL makes available the historical bikesharing usage data (e.g. start and end time of the bike trip, bike id, and station name) in the form of csv files in the following [website](https://cycling.data.tfl.gov.uk). In addition, I use the [bike point API](https://api.tfl.gov.uk/swagger/ui/index.html?url=/swagger/docs/v1#!/BikePoint/BikePoint_GetAll) to add real time information on bike docks across London, including latitute, longitutde, capacity, and available bikes). The bike usage dataset includes 10,138,355 bike trips from Janaury 1, 2019 to December 31, 2019 with attributes for bike trip duration, start trip date and time, end trip date and time, start station id and end station id as in the table below.

![Journey Data](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/journey_data.png)

![Live bike dock information](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/table_2.png)

# **Bike stations**
The bar chart below shows the ten largest bike stations in London in terms of bike capacity.

![Biggest bike stations](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/Unknown-2.png)

# **Distribution of the total hourly bike rentals by day of the week**

The  chart below shows the distribution of hourly bike rentals by day of the week. A distinct pattern can be seen between bike usage during the weekend and the week. During the week days, bike usage is highest between 7am and 9am.Weekend bike usage peaks between noon and 2pm.



In order to analyze the imapct of unfavourable weather conditions such as rain on bike journeys, I obtain historical weather hourly observations for temperature, precipitation, humidity, wind speed, and visibility from the [Dark Sky API](https://darksky.net/dev/account).



![This is a test](https://github.com/albagjonbalajdc/Modeling-bike-journeys-and-weather-in-London/blob/master/Unknown.png)
