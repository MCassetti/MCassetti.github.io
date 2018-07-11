---
layout: post
title: Targetting Demographics with Open Source Data
---
### Introduction
In this exploratory data analysis, a company called Women Tech Women Yes is looking to distribute 
leafleters in NYC Subway stations in order to boost attendance and donations at their 2018 Gala. For the exploration, 
our team proposes optimal locations by using target demographic and open source data.

The target demographic in this example, can range from specific to broad. 
For the scope of exploration project, we chose to remain broad, focusing instead on the following

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
Four major tools were useful for this analysis
1. Pickle
2. Pandas
3. Yelp Api
5. GeoPandas

### Deliverable
Top 10 station recommendations and recommendated day time of day  

### Approach
The goal is to form a reasonable estimate of the number of canidate donors. We can infer this by working backwards from the number of people that are likely to be in a NYC Michelin restaraunt on a busy night. In order for a restaraunt to stay open and profitable, it is reasonable to say ~250 people will be at each restaraunt on a Friday night. Of those 250 people, on the conservative end, half will have commuted from a subway nearby.
Therefore, if we know the denisty of these high price restaraunts in proximity to the nearest train station, which can be done via querying the Yelp Api for $$$$ restaraunts, guess to the demographics of subway goers in that area. 
Given the engagement for sales rate is very low, we can give a smaller guess to a lower bound as being ~2% engagement, we can find the number of people at that time that way engage. Since we know the approximate demographic in that region, we can infer that a certain number of those people are going to a $$$$ restaraunt. From there we can make a reasonable first guess to the number of donations after engagement. For this study we chose 20% of engagements from target demographic as being our candidate donors.


### Data Processing
The MTA turnstile data along with Pandas was used to clean and repackage the 
data. Pandas is a power python tool for packacking and analzying data sets. GeoPandas is another

The Turnstile data is riddled with erroenous entries including negative values. Here is an example
of some code that can be used to filter out these data points
```python
turnstiles_df.loc[(turnstiles_df['totals'] < 0) | (
                  turnstiles_df['totals'] > 100000), 'totals'] = np.nan
turnstiles_df['totals'] = turnstiles_df['totals'].interpolate(method="linear")
```
Once this data was packaged the most populated stations can be evaluated. This can further be broken down by time of day and ay of week. 
A time series analysis showed that Friday night around 8pm has largest density of foot traffic compared to other days. 

![alt_text](https://raw.githubusercontent.com/MCassetti/MCassetti.github.io/master/public/timeseries_data.png)

Query the Yelp API was done in the standard method. However it is rate limited. To get arround it, we propose query with an offset of multiples of the rate limit and determining if the number of entries received is under the limit before attempt more queries. Each api key has a certain number of queries (10,000 as of the time of this writing, with a query limit of 50), therefore this is the only feasible work around.
```python
def iter_search(offset,limit,loc):
    # Example of yelp query 
    args = {
    'location': loc,
    'limit': limit,
    'offset': offset,
    'categories': 'coffee,restaurants',
    'open_at': 1530878400,  
    'radius_filter': '241.40160000000003',
    'price': '4'
    }
    search_results = yelp_api.search_query(**args)
    return search_results
```
Call this function in a loop and query with offsets that are multiples of the limit to save time.

The crime data is from NYC Open data set, which allows for downloading of csv files. The crimes are bin by type and locations (latitiude and longitude). We include this for completeness to show our results overlayed on the crime map of NYC. We chose one type of crime (robberies) for this analysis.
### Results
The following is a table shows the top 10 stations on Friday night between 6-10pm


| Subway Station        | Foot Traffic (people) | Number of High Priced Restaurants | Sales Reps Required | Donors |
| --------------------- | --------------------- | --------------------------------- | ------------------- | ------ |
| 3rd Avenue - 149 St   | 6436                  | 37                                | 3                   | 19     |
| Brighton Beachn       | 5762                  | 38                                | 2                   | 19     |
| 167 St                | 7851                  | 36                                | 3                   | 18     |
| 161 St/Yankee Stadium | 7522                  | 36                                | 3                   | 18     |
| 149 St/Grand Conc     | 4911                  | 37                                | 2                   | 18     |
| Flushing-Main         | 29208                 | 34                                | 12                  | 17     |
| Kings Highway         | 10528                 | 33                                | 4                   | 17     |
| 5th Avenue            | 9099                  | 34                                | 4                   | 17     |
| 103 St - Corona       | 8268                  | 35                                | 3                   | 17     |
| Fordham Rd            | 7623                  | 34                                | 3                   | 17     |

 
The other columns that were added include the sales reps required, which was added as a recommendation based on number of people likely to be engaged (2%) and the average time to talk to someone and persuade them to sign up (5 minutes). Given they will be deployed from 6pm-10pm time frame. This is just an estimate and recommedation, however it should probably be used if there is a shot at capturing maximum candidate donors

Here is a picture with the subway stations overlayed with Robberies in that NY metropolotian area at that time. This plot was generating using the GeoPandas module

![alt_text](https://raw.githubusercontent.com/MCassetti/MCassetti.github.io/master/public/pandas_plot.png)

### Conclusion
We can formulate a reasonable number of candiate donors from using restaraunt pricing to find subway locations to target our demographics. The data analysis can be done with open source data and very few underlying assumptions.

### Recommendaiton
The model can be updated to include other demographics more specific to the company (women in tech) or by creating an optimal customer profile such people that attend museums, or those that have a history of donation.
Additionally, this strategy should be tested and the probability of engagement and donation updated in the model for future studies. 
