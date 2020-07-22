# Analysis of Small Ski Resort Customer Data
![](https://raw.githubusercontent.com/twhipple/dsc-capstone-project-v2-online-ds-pt-090919/master/Final_Notebook_files/winter-1476870.jpg)
Image: Freshly groomed corduroy awaits the first skiers of the day. Source: ma xx, freeimages.com


For an interactive README, [click here.](https://twhipple.github.io/final_notebook)


## Contents
* Summary
* Libraries
* Business Understanding
* Data Understanding
* Modeling and Conclusions
* Recommendations
* Future Work


## Summary
Skiers further away tended to buy their tickets more in advance (having a higher ‘order mean’) but not exclusively - in fact the data showed there was a group of people who just tend to plan more in advance (Furthermore, some skiers may be using a non-local home address when they in fact live in a nearby city or town).

The two clusters with the most people were the skiers who lived the closest to the mountain - within approximately two hours. And obviously the group with the highest ticket revenue were those with the most adult ticket purchases (since adult tickets cost more than youth/senior). But it was the skiers in the cluster who returned more than once or bought multiple tickets that affected the revenue the most.

I was really only able to use data from the Magic website for my conclusions. Without address information (or unique identifiers) from the Liftopia.com website it was difficult to include these customers into the dataset since I needed zip code information to plot where skiers were from, how far they traveled, and the regions that show the most customers.


## Libraries
Below are the libraries I will use in my project.
* import pandas as pd
* import numpy as np
* import matplotlib.pyplot as plt
* %matplotlib inline

* import datetime as dt

* import geopy.distance

* from sklearn.preprocessing import StandardScaler

* from sklearn.cluster import KMeans

* import folium
* from folium import plugins
* from folium.plugins import MarkerCluster
* from folium.plugins import HeatMap
* from pywaffle import Waffle 
* import seaborn as sns



### Business Understanding
**How can I use data to improve my local ski mountain?***

For my data science capstone project, I was able to get four years of skier data from my local ski resort. Not only do I enjoy skiing, but thought I might be able to help out this small resort by providing some useful data analysis. Many of the big ski resort conglomerates have data science teams who examine a wide variety of customer data ranging from the most the popular ski lifts that people ride on a given day to the most sought after types of food ordered at the high mountain lodge. Nevertheless, with the data that was shared with me, I was able to create some fun visuals, explore different aspects of the data, and gain insight about the different customers visiting the resort.

A note on semantics - while the resort certainly is shared by both skiers and snowboarders, I will use the term ‘skiers’ to include all manner of customers who enjoy the mountain. From alpine skiers to telemarker (free-heel) skiers, mono and sit-skiers (Paraplegic and Tetraplegic skiers) to snowboarders (as well as ski bikes, which I have seen at the resort but are not allowed everywhere).


### Data Understanding
I wasn't sure what kind of data my small, local, ski resort would collect. Large resorts collect all sorts of information and have numerous data analysts on staff (looking at; the number of skiers per day, per lift, average number of runs taken, amount of snow, snow-making, number of patrollers, ski accidents/injuries, number of lift-ops working, food sold, rentals, etc.) 

The data I was able to get was a csv file for each of the past four years, 2017 - 2020 with private information deleted. It turns out the resort sells tickets through their website and stores the information with Liftopia.com (an online ticket resaler) which also sold lift tickets for the resort. About 75% of the data sales came directly from the website and about 25% from Liftopia.com. I did not get any data on day visitors who bought tickets directly at the mountain as this information is not collected.

There were 28 columns:
'order_id', 'ticket_id', 'order_status', 'product', 'ticket_type',
'store', 'order_date', 'trip_date', 'marketing_opt_in',
'purchaser_address', 'purchaser_city', 'purchaser_state',
'purchaser_zip', 'purchaser_country', 'net_rate_revenue', 'currency',
'barcode', 'guest_birthdate', 'guest_height', 'guest_weight',
'guest_gender', 'guest_ability_level', 'guest_shoe_size',
'guest_shoe_style', 'guest_shoe_type', 'guest_equipment_choice',
'custom_field_question', 'custom_field_response'


### Data Preparation
The majority of the features contained only about 10% of the data an needed to be deleted. I focused on 'trip_date', 'order_date', 'purchaser_address', 'purchaser_city', 'purchaser_state', 'purchaser_zip', 'net_rate_revenue'. I decided to use only the addresses from the USA since they had the most complete zip code information and could be used for graphs.
I created two new features: 'order_to_trip_date', 'miles_to_resort', and then used a groupby() method to look at the number of trips taken by each skier.


## Modeling
After much EDA and review of the data that was shared with me, I ended up using an Unsupervised KMeans Cluster Model. The majority of the features had insufficient data for me to work with and needed to be deleted. I did so some entry level statistics on types of skiers, skier age, and ability level but do not believe that it accurately represents the whole customer population. I had to do a lot of cleaning of the zip codes which I needed in order to create folium maps as well as determine skier distance from resort. I tried to group the customers according to some unique characteristics in order to look at those skiers who returned more than once, how far they traveled, how far in advance they ordered tickets, and what types of tickets they bought.

Here the the results from my 'examine_clusters' function where stats for each cluster are visible.
![](https://raw.githubusercontent.com/twhipple/dsc-capstone-project-v2-online-ds-pt-090919/master/Final_Notebook_files/cluster_examine_results.png)

## Conclusions
#### Cluster 1 - The Money Makers
Looks to me like this cluster contains the customers with the most number of trips and the highest total revenue. This cluster also includes at least one large group and possibly a number of medium sided groups which visited the resort. Nevertheless, the high number of adult tickets adds to the total revenue possibly as much as returning more often. These groups also contain some of the highest number of Youth/Senior tickets which could mean that these customers are coming as a family.

#### Cluster 3  - The Travelers
This cluster has the highest average distance to resort, which includes possibly all of the outliers who traveled the farthest. Oddly, this cluster does not contain the greatest order to trip dates, meaning that these customers bought their tickets and decided to go with little preparation (an median average of only a couple days). I suppose there is no way of knowing if some of these people have relocated to major cities near the resort such as Boston, Philly, or New York and are just using their original home addresses or if they actually traveled via plane/car to visit the resort in under a few days.

#### Cluster 2 - Families
Cluster 2 is the second largest group, with the second largest revenue, and the second largest number of Youth/Senior tickets. I am calling this the family group since it seems like the majority of the customers bought some combination of tickets. This group probably live within a few hour drive of the resort and the majority purchase their tickets only a couple days in advance.

#### Cluster 0 - One Time Visitors
Cluster 0 is the largest group. The majority of these people have only visited the mountain one time. How can we get these people to come back? How can we get these customers to go from cluster 0 to cluster 1 (the money-making group that returns with larger groups and families)?

#### Cluster 4 - The Planners
This final cluster has the highest mean order to trip date. Meaning that this group of customers are planning to visit the mountain much further in advance than the other groups - even further in advance than groups of people with addresses considerably farther away. For that reason I'm calling them the 'planners'. For whatever reason they have decided they are going skiing on average two months before their trip date - and about two months before most people decide to buy tickets. Perhaps they have very set vacation schedules or limited time off. Either way, they aren't necessarily bringing large groups to the mountain (a mean around 2 people) and their distance to the resort doesn't seem to be that much further than the majority (200 miles - probably close to 4 hours away)


### Evaluation
Results will be given to the ski resort in return for use of data.  I suppose the model could be applied to any number of small, local resorts around the country.


## Recommendations
Include data from Day-Ticket and Season Pass Holders if possible.

Plus, any information about summer and off-season visitors might have some valuable data to explore - I know lots of successful resorts are turning to year-round activities to offset the decline in ski season numbers.

I would also like to split up the Youth/Senior section and look more specifically at age. Unfortunately most of the data didn't include birth dates but it would be interesting to separate kids from seniors purely to look at the numbers.


## Future Work
Now that we have the skiers grouped into clusters, we can apply different marketing strategies based on their grouped characteristics using skier addresses - both for current skiers as well as future skiers.


### Google Distance API
For about 100 dollars I could accurately calculate the driving distance for each customer ($5 per 1000). While I'm not sure if this would change my number significantly, it would certainly be a more accurate statistic.
https://developers.google.com/maps/documentation/distance-matrix/usage-and-billing

### United States Zip Code Demographic
For about 200 dollars I could include much more specific demographic information for each zip code in my dataset - including income and education levels, home and rental costs, specific age and gender stats as well as all population numbers. 
https://www.unitedstateszipcodes.org/zip-code-database/
