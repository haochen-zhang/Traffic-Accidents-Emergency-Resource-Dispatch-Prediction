# Traffic Accidents Emergency Dispatch Prediction

A traffic accident casualty prediction model using Python

Introduction video: https://www.youtube.com/watch?v=uQ3aD4Z9xSw

### Table of Contents
- Background
- Data Resource
- EDA
- Model
- Results
- Contributors
- Further

### Background

In a modern era, it is undeniable that a city’s transportation system is an essential element of human civilization, improving human living standards and contributing to the economic growth of a city.  What comes with an ever-growing reliance on urban transportation is an increase in road accidents. As such, road accidents have been actively studied because of the growing demand for safety conditions and prevention strategies in the hopes of reducing the impact of accidents on road-users and vehicles. Our study aims to develop a model that predicts harmful accidents in New York City, the most congested city in the United States. According to the NYPD records, in 2019, the Big Apple saw over two-hundred thousand collisions, as shown in this animation. And despite this number declined by 46% during the year of pandemic, there were still an average of 303 car accidents on the city’s street every single day in 2020. A considerable amount of accidents would inevitably places greater demands on the police and medical resources. As such, efficient dispatch of police officers, ambulances, and medical workers relies heavily on predictions of accident harmfulness to save more lives and reduce the severity of  an accident. With this real-world application in mind, we propose the question that at what time, location, weather, and collision conditions that a harmful accident is likely to occur in new york city. 

### Data Resource
- <a href= "https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95">NYC Traffic Accidents</a> 
- <a href= "https://data.cityofnewyork.us/Transportation/VZV_Speed-Limits/7n5j-865y">Speed Limits of NYC Street</a> 
- <a href= "https://www.ncei.noaa.gov/maps/lcd/">Weather dataset</a> 
- <a href= "https://www.ncei.noaa.gov/maps/lcd/">National Centers for Environmental Information</a> 

### EDA

To answer our research question, we draw data from NYC Open Data, the NYC Police Department records on traffic accident, and use a substantial dataset of vehicle collisions from March, 2019, to March, 2021.  Predictors from this dataset include the number of cars involved in an accident, the 5 different boroughs of new york city, the time when an accident occured, which is included in the model as  a 3-hour interval, the speed limit of the street where an accident occured, 6 categories of contributing factors recorded by the police, and four categories of different car types involved in the accident. 

To gain information on weather conditions,  we merged the NYC Traffic Accidents data to the weather data by location (latitude and longitude) and the time when an accident occurred. Predictors from weather data include temperature, humidity, precipitation, visibility, and three additional categorical variables, which are whether or not the accident occurred during daytime, in a rainy day, or snow day. 

Finally, given the goal of our study is to predict whether an accident is harmful, we defined our outcome variable by combining the injury and deaths information from the NYC traffic accidents data. An accident that results in at least one injury, death, or both is counted as a harmful case, assigning 1 as True for the outcome variable casualty, whereas accidents without any injury are deemed as harmless.  assigning 0 as False for the outcome variable.

<br />

![categorical_vs](https://user-images.githubusercontent.com/70677169/148323979-c80038ea-fb02-4714-9e21-852ef3fad7a5.png)

<p align="center">Distribution of accidents across categorical variables</p>

<br />

![margin_bar](https://user-images.githubusercontent.com/70677169/148324008-ec2cf07c-f733-4953-a24b-a72db48c90a2.png)

<p align="center">Marginal association of each predictor vs. whether or not an accident is harmful</p>

<br />

![causuality_vs](https://user-images.githubusercontent.com/70677169/148324013-6c7530e7-bdbf-4bac-8800-d3b579b26b71.png)

<p align="center">NYC maps of non-injured accidents vs. injured or fatal accidents</p>

<br />

### Model

After training a bunch of classification models including basic logistic regression, lasso-like logistic regression with and without interactions, decision tree model, and random forest model, we detected that the random forest model with all predictors yields the lowest negative false rate as shown in the figure. Although we acknowledge the fact that this model doesn’t generate the highest accuracy score as shown in the bottom figure, We decided to use false negative rate as the metric for model selection because we believe in the context of car accidents and given the real-world application of this model is to dispatch police and medical resources efficiently to accidents that most needed, it is essential to have low  FNR—that is, a lower chance of predicting an accident as harmless when in reality that this certain accident is harmful. Also, as demonstrated by the plot precision-recall curves, we can see the area under the light green curve is relative high, indicating the selected model’s better performance than others. As such, we proceed our prediction analysis with the random forest model.

<p align="center">
  <img src="https://user-images.githubusercontent.com/70677169/148328666-aa655578-7aae-40a4-abad-93909676f7db.png" width="600">
  
  <p align="center">Accuracy scores and FNR for all models with default threshold and adjusted threshold</p>
  
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/70677169/148328591-b947d7be-2252-4866-b143-3f030e756733.png" width="600">
  
  <p align="center">Precision-recall curves for all models with default threshold and adjusted threshold</p>
  
</p>

### Results

To better understand the model predictions, we display the relative risk of harmful accidents by zip code of NYC (predictions generated from the test set). As we can see from the map, our model predicts that harmful accidents are mostly likely to occur in Staten Island and least likely to occur in Manhattan. This could be the case because although Manhattan tends to have heavy traffic flow, the speed limit is low. As we learned from the output of our model, speed limit is positively related to casualty. This means that it is likely in Staten Island the speed limit is high as the traffic flow is not that heavy as compared to that in Manhattan or Queens. It is also worth noting that some specific areas in Brooklyn and the Bronx are at high risk of harmful accidents as well.

<p align="center">
  <img src="https://user-images.githubusercontent.com/70677169/148328380-9f09230f-5759-4160-8ce1-13204120bfff.png" width="800">
  
  <p align="center">Relative risk of harmful accidents by zip code in NYC</p>
  
</p>

As discussed in the introduction, the ultimate goal of our model is for a scenario in which police offices and medical centers are receiving phone calls regarding car accidents occurring on the streets of NYC and need to dispatch resources to people who need them. To realize this goal, The pic demonstrates a potential interface for police officers and medical workers. By gathering the information of all our predictors such as what types of cars are involved in the accident, the contributing factors, the speed limit of the street, and so on6, this interface could automatically rank the reported accidents by the probability of a certain accident being harmful. Higher probability would be labeled in red and lower risk would be labeled in green. As such, relevant departments can dispatch resources efficiently, especially when the traffic is terrible and/or resources are limited, to the accident sites that need most help.

<p align="center">
  <img src="https://user-images.githubusercontent.com/70677169/148328460-ad76a8b2-987b-4df4-b8b6-6f502147a240.png" width="800">
  
  <p align="center">A demo of NYC real-time car accidents ranked by the probability of being harmful</p>
  
</p>

### Contributors
This project exists thanks to all the people who contribute.

<a href="https://github.com/mirahx24"><img src='https://avatars.githubusercontent.com/u/14883017?v=4' height="40" width="40"></a>
   <a href="https://github.com/sungwhan1990"><img src='https://avatars.githubusercontent.com/u/70632710?v=4' height="40" width="40"></a>
   <a href="https://github.com/tiffanyy2019"><img src='https://avatars.githubusercontent.com/u/54950062?v=4' height="40" width="40"></a>
   
### Further
If you are looking for detailed information, see final report in repository.
