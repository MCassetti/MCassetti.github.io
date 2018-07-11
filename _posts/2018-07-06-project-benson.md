---
layout: post
title: Targetting Demographics with Open Source Data
---
### Introduction
In this exploratory data analysis, a company called Women Tech Women Yes is looking to distribute 
leafleters in order to boost attendance and donations at their 2018 Gala. For the exploration, 
our team proposes optimal locations by using target demographic and open source data.

The target demographic in this example, can range from spefic to broad. 
For the scope of exploration project, we chose to remaing broad, focusing instead on the following

1. Time of day with highest foot traffic
2. Subway locations with high density of upscale restaraunts
3. Locations located in low crime areas  
 
Additionally, we need the total number of people to be high enough
to account for low engagement rates with leafleters.  

### Data Sources
Three sources were leveraged to do this analysis were as follows
1. MTA Turnstile Data 
2. Yelp API
3. NY Crime Statistics 

### Tools
1. Pickle
2. Pandas
3. Yelp Api
5. GeoPandas

### Deliverable
Top 10 Stations with highest number of candidate donors 

### Approach
The goal is to form a reasonable estimate of the number of canidate donors. We can inference this by working backwards from the number of people that are likely to be in a NY Michelin restaraunt on a busy night. In order for a restaraunt to stay open and profitable, it is reasonable to say ~250 people will be at eachrestaraunt on a Friday night. Of those 250 people, on the conservative end, half will have commuted from a subway nearby.
Therefore, if we known the denisty of these high price restaraunts in proximity to the nearest train station, which can be done via querying the Yelp Api for $$$$ restaraunts, guess to the demographics of subway goers in that area. 
Given the engagement for sales rate is very low, we can give a smaller guess to a lower bound as being ~2%engagement, we can find the number of people at that time that way engage. Since we know the approximate demographic in that region, we can infer that a certain number of those people are going to a $$$$ restaraunt. From there we can make a reasonable first guess to the number of donations after engagement. For this study we chose 20% of engagements from target demographic as being our candidate donors.


### Data Processing
The MTA turnstile data along with Pandas was used to clean and repackage the 
data. Pandas is a power python tool for packacking and analzying data sets. GeoPandas is another

The Turnstile data is riddled with erroenous entries including negative values. Here is an example
of some code that can be used to filter out these data points
```python

```
Once this data was packaged the highest stations can be evaluated. This can further be broken down by time of day and ay of week. 
A time series shows that Friday night around 8pm has largest density of foot traffic compared to other days. 

Query the Yelp API was done in the standard method. However it is rate limited. To get arround it, we propose query with an offset of multiples of the rate limit and determining if the number of enteries recievedis greater or 
**Insert figure here**
![alt_text](http://zwmiller.com/projects/images/monte_carlo/part5/business_impact.png)

### Results
The following is a table shows the top 10 stations on Friday night between 6-10pm



  Subway Station       	Foot Traffic (people)	Number of High Priced Restaurants	Sales Reps Required	Donors
  3rd Avenue - 149 St  	6436                 	37                               	3                  	19    
  Brighton Beachn      	5762                 	38                               	2                  	19    
  167 St               	7851                 	36                               	3                  	18    
  161 St/Yankee Stadium	7522                 	36                               	3                  	18    
  149 St/Grand Conc    	4911                 	37                               	2                  	18    
  Flushing-Main        	29208                	34                               	12                 	17    
  Kings Highway        	10528                	33                               	4                  	17    
  5th Avenue           	9099                 	34                               	4                  	17    
  103 St - Corona      	8268                 	35                               	3                  	17    
  Fordham Rd           	7623                 	34                               	3                  	17    

 


Here is a picture with the subway stations overlayed with Robberies in that NY metropolotian area at that time. This plot was generating using the GeoPandas module


### Conclusion
We can formulate a reasonable number of candiate donors from using restaraunt pricing to find subway locations to target our demographics. The data analysis can be done with open source data. 

![alt_text](http://zwmiller.com/projects/images/monte_carlo/part5/business_impact.png)

### Recommendaiton
The model can be updated to include other demographics more specific to the company (women in tech) or by creating an optimal customer profile such people that attend museums or ballets, or those that have a history of donation. =

