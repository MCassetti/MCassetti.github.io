---
layout: post
title: Predicting the Price of Bitcoin Using Linear Regression 
---
### Introduction
The goal is to predict the price of bitcoin by chosing features that are not directly
related to market analysis of bitcoin. In particular, building a linear regression model
which takes data that quantifies the social sentiment, interest in the technology, and
adoptability and tries to predict the price of bitcoin. This was primarly done as the
BTC price bubble from 2017 highly correlates with the behavior of other features such
as number of tweets and google's interest metrics.

![alt_text](https://raw.githubusercontent.com/MCassetti/MCassetti.github.io/master/public/Price_bubble.png)


![alt_text](https://raw.githubusercontent.com/MCassetti/MCassetti.github.io/master/public/price_correlations.png)

### Deliverable
1. Linear Regression model which predicts the price of bitcoin 

### Data Sources
Data was scrapped off the internet using a combination of Selenium and BeautifulSoup.
Here is a summary of data sources
1. Daily BTC price using Coinmarketcap historical data 
2. Google Trends (weekly data that is interpolated to daily)
3. Bitinfocharts 
4. Nasdaq


### Features
In order to capture a model built off social sentiment, technological interest, and adoptability the
following features were chosen
1. Number of tweets
2. Google's global trend
3. Number of bitcoin commits
4. Price of Nvidia Stock 
5. Number of transactions
6. Number of bitcoin wallets
7. Transaction fees

### Exploratory Data Analysis
The first step was to run OLS and classify how well the model does in the absence 
of transformation or regularization.
Upon first glance, the model produced a fairly good r_squared, however it is clear that the features are highly non-linear
relationship to the price. Additionally, at this stage, despite the r_squared, the model is not given prediction in a reasonable
price range. 

Regularization was done to determine which, if any of the features could be excluded. Using L1 and L2 regularization, it was shown that the number of commits to the btc github repo did not correlate with price. This seems to indicate the interest to maintaining the bitcoin code base is not financially motivated!

Additionally, some of the data appears to be highly non-linear. A first attempt at linearization was done by transforming the following features:
1. Log-btc price 
2. box cox transformation 
    - Number of Tweets
    - Number of Transactions
    - Number of Wallets
3. Log of other feature data
    - Google Trend Data
    - Transaction Fees
    - Nvidia Stock Prices

### Models

After transformation of the feature data, multiple models were cross validated with the training set to determine which model will be our final model.
The models that were explored for the linear regression were as follows:
1. OLS
2. LassoCV
3. RidgeCV
4. Polynominal(n=2)

Additionally two non-linear regressors were explored
1. Random Forest
2. Decision Trees


### Results
Ultimately, the reguralization did little to boost the performance of this model, indicating the surviving features were all significant.
Additionally, the polynominal models performed the best, indicating non-linearity after transformation had been applied. 

![alt_text](https://raw.githubusercontent.com/MCassetti/MCassetti.github.io/master/public/polynominal.png)


Taking a closer look at the non-linear regressors, we get even better validation and test scores, in some cases close to .95 r_squared value. This is incredible, but
the real question comes down to price prediction, which our model would indicate we have found a btc price crystal ball.
Unfortunately after trying to predict the daily price of btc, given known feature values, the predictions did not seem to go as planned.

![alt_text](https://raw.githubusercontent.com/MCassetti/MCassetti.github.io/master/public/random_forest.png)

### Conclusion
Ultimately, there are several major issues with this linear regression model. Namely, doing linear regression on a time series. In particular, time series data exhibits autocorrelation, which means the residuals versus your fitted data shows a clear pattern with respect to the independent variables, i.e. heteroskedastic. 

![alt_text](https://raw.githubusercontent.com/MCassetti/MCassetti.github.io/master/public/residuals.png)

Additionally, the reason the estimate was so off has to do with the overall price volitility of bitcoin, especially over the last year. Since the variance is close to 3700USD over the course of a year then even with r_squared accounting for 95% of the variation, it is reasonable to get estimates off from the actual price on the magnitutde as seen from our predictor. 

The other major issue with this model is that it has a casuality issue. Once the the features are known (number of tweets, transactions, etc), then so too is the price. This means in order to actually use this model in a trading stradegy, it would have to predict the future price.  

### Future Analysis
I would like to explore autoregressors to try to predict the future price in the presence of autocorrelation, which would also give a better prediction of the future price. 
Additionally, the model could also include many other features, such as other economincal predictors like price of gold, or inflation of the us dollar.
Although this model was worth exploring, it is probably not advisable to formulate a trading stradegy from a linear regression model.
