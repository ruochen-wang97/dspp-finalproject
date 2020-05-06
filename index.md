**Predicting the Number of New Unemployment Insurance Claims Filed for the Week Ending April 18**
=====

_By Radhika Kaul, Odiche Nwabuikwu, & Ruochen Wang_

## Introduction

In face of the COVID-19 pandemic, many states in the United States have issued stay-at-home orders or mass gathering restrictions to prevent further spread of the disease. The unintended consequences of these restrictions have hit the labor market. Data from the federal government shows that following the enforcement of social distancing measures, late March has seen the largest surge in the filing of new unemployment insurance claims in the modern US history.

Understanding these surges at the state level has important policy implications as lay-offs and furloughs prevail in the labor market. This project aims to create a supervised machine learning model that predicts the number of new unemployment insurance claims filed per week to inform policymakers on the labor market trends in the immediate future.

## Data

We tested different models and decided to use data spanning from the week ending January 25, when the first case in the US was reported by the Centers for Disease Control and Prevention (CDC), to the week ending April 18, when the latest federal data are available.

The target and predictor variables in our dataset are from the following sources:

- [Unemployment Insurance Weekly Claims data](https://oui.doleta.gov/unemploy/claims.asp), administered by the U.S. Department of Labor (DOL)'s Employment & Training Administration
- COVID-19 tracking data from the [COVID-19 Data Repository](https://github.com/CSSEGISandData/COVID-19), administered by CSSE at Johns Hopkins University
- COVID-19 projection data from the [Institute for Health Metrics and Evaluation (IHME)](http://www.healthdata.org/) at the University of Washington
- Structural data from the [U.S. Census Bureau](https://www.census.gov)

### Codebook

_Real-time Variables_

* **initial_num**: target var. # of new claims filed for that week. 
* **continued_num**: # of insured unemployment for that week
* **days_since_sah**: # days between the last day of week and the day when state issued stay-at-home order
* **days_since_peak[^1]**: # of days between last day of week and predicted death peak according to IHME
* **Incident_Rate**: # of confirmed COVID cases per 100,000 residents
* **Mortality_Rate**: # of reported deaths * 100/ # confirmed cases

_Structural Variables_

* **region**: census region
* **topind_gdp**: top industry in terms of % GDP contribution
* **topindemp_1**: top 1 industry in terms of % employment
* **topindemp_2**: top 2 industry in terms of % employment
* **topindemp_3**: top 3 industry in terms of % employment
* **totpop_2018**: total population in 2018
* **pctpop_work**: % population age 15-64
* **pctpop_hs**: % population with at least a high school degree
* **personal_inc2019**: per capita personal income in 2019
* **gov_republican**: dummy set equal to 1 if governor republican

See [here](https://ruochen-wang97.github.io/dspp-finalproject/data-cleaning.html) for complete data cleaning process and descriptive analysis.

[^1]: _Calculated based on projections made by IHME. Last updated on April 29 and is no longer being updated. See [here](https://www.businessinsider.com/map-when-each-state-will-experience-coronavirus-peak-outbreak-2020-4) for predicted peaks._

## Modeling

We constructed a Random Forest (RF) model to predict the number of new unemployment insurance claims filed for the week ending April 18 using the variables listed above. We used three subsets of data - data from when the first COVID case was reported in the US, data from when the first COVID-related death was discovered in the US, and data from when the first stay-at-home order was issued in California - to determine the best model. Within each of the above scenarios, we tested a CART model together with three RF models with different hyperparameters, and for every data subset RF models always perform better than the CART model.

* **Training and Testing Sets**: After researching existing time-series models, we decided to assign the data from the week ending April 18 to the testing set and all the other data from previous week to the training set. This allows us to predict the outcome for the week ending April 18 based on data from previous weeks.

* **Resampling**: Bootstrap 

* **Outcome Variable**: _initial_num_

* **[Candidate Model 1](https://ruochen-wang97.github.io/dspp-finalproject/model_1stcase.html)**: Random Forest

  +  **Data From:** Week ending Janurary 25, when first COVID case was reported in the US
  +  **# of Features at Each Split:** 5
  +  **# of Trees:** 500
  
* **[Candidate Model 2](https://ruochen-wang97.github.io/dspp-finalproject/model_1stdeath.html)**: Random Forest

  +  **Data From:** Week ending March 7, when the first COVID-related death was discovered in the US (see [here](https://www.npr.org/sections/coronavirus-live-updates/2020/04/22/840836618/1st-known-u-s-covid-19-death-was-on-feb-6-a-post-mortem-test-reveals))
  +  **# of Features at Each Split:** 5
  +  **# of Trees:** 100

* **[Candidate Model 3](https://ruochen-wang97.github.io/dspp-finalproject/model_1stsah.html)**: Random Forest
  
  +  **Data From:** Week ending March 21, when the first stay-at-home order was issued in California
  +  **# of Features at Each Split:** 5
  +  **# of Trees:** 100

* **Error Metric**: Root Mean Square Error (RMSE). We decided to use RMSE because the outcome we are predicting is a regression problem, and it penalizes outlier observations.

We decided that Candidate Model 1 is the best model given that it has the lowest error rate.

## Discussion and Limitations

For this project, we constructed a Random Forest model in a supervised machine learning environment to predict the number of new unemployment insurance claims filed per week in the time of COVID-19. We used real-time variables such as the number of insured unemployment up until the target week and the number of days since the state issued a stay-at-home order as proxies for the interaction between the labor market and the ongoing pandemic, and structural variables to account for characteristics unique to each state. We end up with a model with 500 trees and five features at each split, with an R-sqaured of .683.

We think the primary reason that the first candidate model outperforms the other two is that it is trained on a larger dataset. This means it can utilize more information when making predictions. Within each subset of data (data from week ending January 25, March 7, and March 21), RF models always outperform the CART model because they allow for different combinations of features in each iteration and hence capture more variation in the data. Potential limitations of our model include overfitting, as suggested by the much lower RMSEs and residules for the training data than for the testing data (see [residual plot](https://ruochen-wang97.github.io/dspp-finalproject/model_1stcase.html)), as well as inevitable biases associated with the complexity in the factors affecting the labor market. Additionally, as with all Random Forest models, our model may not be very robust when changing the input data and the outcome it predicts for the following weeks might not be hyper consistent/reliable. It also cannot factor in the interdependence between observations in time series data when making splits.