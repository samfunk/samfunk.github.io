---
layout: post
title: 'Linear Regression with Web-Scraped Data'
date: 2017-10-09 12:00:00 -0600
categories: jekyll update
---
# Investigating the Federal Reserve's Influence on the S&P 500  

<p align="center"><img src="http://www.frontpagemag.com/sites/default/files/uploads/2014/10/janet-yellen.jpg" alt="Yellen" width="500"/></p>  

## Data  

My initial plans for this project were quite ambitious. I wanted to scrape years of articles from multiple financial news sources, such as MarketWatch, Bloomberg, CNBC, Reuters, and the Financial Times. However, I soon realized that, although this could and can still be done, time was not on my side. Therefore, I decided to limit my data aspirations to one site over a four month period.

I scraped every article from MarketWatch from June through September 2017. This amounted to around 13,000 total articles. I took stock of each article's subject and saved all the economic ones. Out of these, I distinguished whether the article was primarily focused on the Fed (strong cases) or if it simply mentioned a word such as 'Yellen' or 'FOMC' (weak cases). At this point, I had my data on the distribution of Fed articles against the rest of MarketWatch's content.

My next source of data came from the Federal Reserve directly. I scraped the Fed's calendar and took count of when specific events occurred. Some of these events included speeches, testimonies, FOMC meetings, and statistical releases. I also decided to include a variable that would try and measure the general public's interest in the activity of the Fed. Using Google Trends, I pulled metrics on search terms like 'unemployment', 'inflation', 'federal reserve', and 'yellen.' I devised a weighted score that aimed to capture the daily levels and changes of interest in the Federal Reserve.

Finally, with the S&P 500 as my independent variable, I pulled historical, daily data for the GSPC (equivalent to the SPX index) from Yahoo Finance. The sources of all these data sets are listed below.

* [MarketWatch.com](http://www.marketwatch.com/search?q=&m=Keyword&rpp=100&mp=806&bd=true&bd=false&bdv=09%2F30%2F2017&rs=true)  
* [Fed Calendar](https://www.federalreserve.gov/newsevents/2017-september.htm)
* [Google Trends](https://trends.google.com/trends/explore?date=today%203-m&geo=US&q=federal%20reserve)  
* [GSPC Data](https://finance.yahoo.com/quote/%5EGSPC/history?p=%5EGSPC)  

## Exploratory Data Analysis  

Before I jumped into any regressions, I needed to take a good look at my data. One issue I ran into immediately was the fact I was using stock market data, which is a sequence of discrete-time data. When it comes to linear regressions, time series data can prove to be problematic since individual data points can potentially be influenced by past or future data points. To solve this issue of autocorrelation, I took the daily difference of the SPX and used those values as my target variable. This technique of differencing greatly reduces autocorrelation and the underlying trend present in the closing SPX values as well as making the data stationary. At this point, I was able to make the assumption that my data would be able to be used for linear regression. It is important to note that my results could still potentially be affected by time, which could lead to spurious results.

As for my features, I narrowed the relevant variables down to 'Fed' articles per, the proportion of 'Fed' articles to economic articles, daily statistical releases, and the weighted Google Trends score. These were the only variables with any semblance of correlation with the SPX. It should be noted that the first two features related to 'Fed' articles had a high positive correlation. The next section on my regression model will address this issue. Finally, I transformed the dependent variable by taking the absolute value of the daily difference in index level. This decision was made with the naive nature of my features in mind. None of the articles or Fed events were measured on the connotation of their content, therefore, making the index's change in magnitude more important than the direction.
