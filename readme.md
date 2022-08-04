<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 5px; height: 30px">

# Capstone Project: Forecasting HDB Resale Prices

## Introduction

With more than 1 million flats spread across 24 towns and 3 estates, the Singapore brand of public housing is uniquely different. These flats spell home for over 80% of Singapore's resident population. Singaporeans have two main options when it comes to purchasing a HDB flat; they can either choose to buy a new flat from HDB (BTO) or a resale flat from the open market. Naturally, both types of flats come with their own pros and cons. 

There are reports stating that demand for resale flats has spiked in recent years, resulting in a reactionary increase in resale flat prices. According to the PropertyGuru Singapore Property Market Report Q2 2022, young families continue to gravitate towards the HDB resale market as a result of the latest round of BTO building delays and their unwillingness to wait through lengthy BTO completion periods, even though their prices are generally higher. Hence, we will be focusing on resale flats in this project.

In a [typical resale flat transaction](https://www.hdb.gov.sg/residential/buying-a-flat/buying-procedure-for-resale-flats/overview), an accurate valuation can only be requested after registering an **Intent to Buy** and obtaining an **Option to Purchase (OTP)**. This is not intuitive as this means that HDB will only provide the valuation after the price is agreed on. If the **Cash Over Valuation (COV)**, the difference between the sale price of the flat and its actual valuation by HDB, exceeds what buyers expect or budget for, they may find it hard to 'cough up' the **COV** in cash. The only way to get an estimate of the **COV** before this stage is only through personal due diligence, even for property agents.

## Problem Statement

The aim of this project is to create a model that will give an accurate prediction of the actual valuation that can be utilized by everyone involved in a resale flat transaction, namely the buyers, sellers and property agents). This would inform the relevant parties whether a particular flat is undervalued or overvalued, and in turn give a good estimate of its potential COV.

---
## Data Sources and Descriptions

- **[2000-2012](https://data.gov.sg/dataset/resale-flat-prices?resource_id=8c00bf08-9124-479e-aeca-7cc411d884c4):** Resale Flat Prices from January 2000 to February 2012

- **[2012-2014](https://data.gov.sg/dataset/resale-flat-prices?resource_id=83b2fc37-ce8c-4df4-968b-370fd818138b):** Resale Flat Prices from March 2012 to December 2014

- **[2015-2016](https://data.gov.sg/dataset/resale-flat-prices?resource_id=1b702208-44bf-4829-b620-4615ee19b57c):** Resale Flat Prices from January 2015 to December 2016

- **[2017-](https://data.gov.sg/dataset/resale-flat-prices?resource_id=f1765b54-a209-4718-8d38-a39237f502b3):** Resale Flat Prices from January 2017 to July 2022

- **[mrt](https://github.com/hxchua/datadoubleconfirm/blob/master/datasets/mrtsg.csv):** MRT Station Information (Updated TE Line details + removed unused stations)

- **[malls](https://en.wikipedia.org/wiki/List_of_shopping_malls_in_Singapore):** Data on Shopping Malls in Singapore. (Updated inaccurate mall names + removed malls that closed)

- **[supermarkets](https://data.gov.sg/dataset/listing-of-licensed-supermarkets):** Listing of Licensed Supermarkets as at May 2021

- **[hawker_centres](https://data.gov.sg/dataset/hawker-centres):** Hawker Centre and Market locations in Singapore. Converted from KML file with location details

- **[parks](https://data.gov.sg/dataset/parks):** Park locations in Singapore. Converted from KML file with location details

- **[schools](https://data.gov.sg/dataset/school-directory-and-information):** School Directory and Information



---
## Executive Summary

### 1. Data Cleaning and EDA
I started by merging all the dataframes relating to Resale Flat Prices as they have similar columns. Individual features were cleaned and proper treatment was given. Preliminary EDA was done for most columns. I also explored the relationships between the features and the target variable, Resale Price. The combined dataframe was subsequently saved and exported.

### 2. Feature Engineering
In my second notebook, my aim was to create additional features to enhance the predictive abilities of my model. I utilized a function to scrape coordinates data from [OneMap API](https://www.onemap.gov.sg/docs/#onemap-rest-apis). Subsequently, I calculated the distance between all the unique addresses and the nearest respective amenities, and the number of said amenities in their vicinity. Lastly, I used another function to calculate the distance between each address and City Hall MRT (used to represent the most central spot in Singapore). These new features were then saved into a new dataframe.

### 3. Preprocessing
Categorical features need to be encoded before the modeling process. Both ordinal and nominal features were encoded using ordinal and one-hot encoding respectively. The dataframes from Steps 1 and 2 were then merged and exported.

### 4. Modeling
I utilized PyCaret, an open-source, low-code machine learning library in Python that automates machine learning workflows to streamline and enhance the modeling process.
After comparing several models, I decided to go with LightGBM, an efficient and high-performance gradient boosting framework which was originally developed by Microsoft.
Root Mean Squared Error (RSME) was used as my main metric as it is particularly useful when large errors are undesirable. It is also measured in the same units (SGD) as our target variable. The model was then tuned and saved for potential deployment in the future.

---
## Conclusion

We managed to create a model that has a significantly lower RSME when compared to our baseline. The final RSME is 20,727, which is less than 4% of the current mean resale price of ~$550k. We can also conclude that the features we engineered have a statistically significant relationship with resale flat prices, and were useful in improving the accuracy of our models. Our model should be able to detect undervalued and overvalued flats, and should be able to give a good estimate of COVs.

Moving forward, I will aim to deploy the trained model in a web-based application like Streamlit. I also plan to test this model further on future data. HDB updates the dataset very frequently, making this plan very feasible.

## Limitations

There are other factors that will influence resale flat prices and COVs like the condition of flats and the directions they are facing. Flats with extensive renovations and furnishings or flats which are well maintained tend to fetch a higher price. Flats that are North/South facing have generally higher demand compared to those facing East/West because of heat and glare of the Sun. Our inability to take these factors into account might ultimately affect our model's accuracy.

A key consideration in this project was whether I should select the most accurate or efficient model. Processing power was a big concern; I only managed to train all the models with the help of AWS Sagemaker Studio. I only ended up choosing the most efficient model, LightGBM, because of its high potential in deployability. This decision might have led to a notable tradeoff in prediction accuracy.

Euclidean distance from location was also used instead of travel time. This may not be the most accurate unit because euclidean distance from a location is not perfectly correlated with the time taken to get there. This could have been improved by using a paid API (e.g. Google Maps API) to further understand the relationship between distance and travel times between two locations.


---

## References
1. https://www.teoalida.com/singapore/hdbflattypes/
2. https://www.moe.gov.sg/primary/p1-registration/distance
3. https://www.propertyguru.com.sg/property-guides/hdb-valuation-sales-12882
4. https://blog.moneysmart.sg/property/hdb-resale-flat-factors/
5. https://dollarsandsense.sg/bto-vs-resale-choose/
6. https://www.hdb.gov.sg/about-us/our-role/public-housing-a-singapore-icon
7. https://www.hdb.gov.sg/residential/buying-a-flat/buying-procedure-for-resale-flats/overview
8. https://sg.finance.yahoo.com/news/cash-over-valuation-hdb-resale-022900303.html

---












