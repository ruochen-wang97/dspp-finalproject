**Predicting Initial Unemployment Insurance Claims for third week of April using k-Nearest Neighbor and Random Forest Models.**
=====

 _By Radhika Kaul, Odiche Nwabuikwu, and Ruochen Wang_

### Introduction
In the midst of the COVID-19 pandemic, various states have issued mass gathering restrictons and stay-at-home orders to prevent further spread of the disease. The unintended consequence of these strict measures have hit the labor market. Official federal estimates suggest that the largest surge in new unemployment insurance claims in U.S were experienced in late March, due to widespread social distancing measures. Understanding these surges at the state-level has value to the policymakers, especially in the trying times when increasing number of workers are being furloughed (this line is weird, better way to phrase it?). 

For our project, we constructed a datatset combining variables from various sources: data on unemployemnt insurance claims until the week of April 18 was gathered from the [Unemployment Insurance Weekly Claims data](https://oui.doleta.gov/unemploy/claims.asp), managed by the U.S. Department of Labor (DOL)'s Employment & Training Administration. COVID-19 related data was gather from two sources: [Institute for Health Metrics and Evaluation](http://www.healthdata.org/), an independent global health research centre at the University of Washington and the [COVID-19 Tracking data repository](https://github.com/CSSEGISandData/COVID-19) operated by Johns Hopkins University. We attempt to conduct a supervised machine learning exercise and predict the state-level unemployment insurance claims using k-Nearest Neighbour and Random Forest models. 

**Note**: _Official federal data on initial claims come out at a weekly interval and at a lag, so we assume to situate our models back in time  when we're still in the third week of April_


### Data Description

Following is the codebook we created for our dataset:

_Structural Variables_

- **State**: State
- **personal_inc2019**: Personal income in 2019 (state-level)
- **topind_gdp**: Top industry contributing to GDP
- **topindemp_1**: 
- **topindemp_2**: 
- **topindemp_3**:
- **region**: Region in the US
- **pctpop_hs**: Percent population with a HS degree
- **totpop_2018**: Total population in 2018
- **totpop_work**: Total working population
- **pctpop_work**: Percent working population
- **gov_republican**: Democratic or Republican governor
- **FIPS**: State FIPS code

_Real-time Variables_

- **initial_claims**: Number of first time claims filed an individual after job loss (weekly estimates)
- **continued_claims**: Number of insured unemployed workers filing for UI benefits (wwekly estimates)
- **sah_date**: Stay at home order start dates
- **massgath_res_date**: Mass gathering restriction start dates
- **noness_res_date**: Non-essential businesses closure date
- **days_since_peak_deaths[^1]**: Days since predicted peak by IHME model
- **reopen_date**: Stay at home order end dates
- **peak_death_yn**: Whether state has experienced a peak in COVID-19 deaths
- **sah_yn**: Whether the state issue a stay-at-home order date
- **Confirmed**: cummulative COVID-19 deaths by April 18, 2020
- **Deaths**: cummulative COVID-19 confirmed cases deaths by April 18, 2020
- **Incident_Rate**: Confirmed cases per 100,000 people
- **Mortality_Rate**: Number recorded deaths * 100/ Number confirmed cases

### The Modelling Process

We plan to conduct a model comparison for prediction of number of unemployment insurance claims filed in 4th week of April: Following are the models we shall apply on our training data:

- **Training-Testing Split**: 80:20 Split of the training and testing data. Since this is a time-series machine learning model, we shall keep all the variables except the initial_claims_2020_04_18 in the training data and compare our predictions to the actual values in the testing data which will have the initial_claims_2020_04_18 variable. 

- **Resampling Method for training data**: k-fold cross validation. Reading of the mechanism suggests that for smaller sample size, 10-fold CV repeated 5 or 10 times will improve the accuracy of your estimated performance. Bootstrapping (random sample of the data taken with replacement) seems to work better than k-fold CV due to less variability in the error measure, however, may increase the bias on the error estimate. Since ours is a really small sample, there is a higher likelihood of us running into that disadvantage.

- **Target Variable**: The target variable in our training set will be  _initial-claims-2020-04-11_, that is the number of initial claims filed in the week of April 11.

- **Predictors**: All the variables except FIPS, Lat, Long_

- **Model 1**: K-Nearest Neighbor - In this model, when a new sample is predicted, _k_ training set points are found that bears the almost same resemblance to the new sample being predicted. This algorithm is simple and easy to implement and is highly locally interpretable. Higher values of _k_ may result in lower RMSEs but may also run into the risk of overfitting the model.

    1) Selection 1: when k = 10
    2) Selection 2: when k = 25
    3) Selection 3: when k = 50
    
- **Model 2**: Random Forest - In this model, a large number of _de-correlated_ trees, with pre-specified targets and features are used to generate predictions. More trees usually allude to the model having greater accuracy. It also does a good job in reducing the bias and the variance of the error metric.

    1) Selection 1: when mtry = 5,  ntree = 50
    2) Selection 2: when mtry = 20, ntree = 100
    3) Selection 3: when mtry = 50, ntree = 400
    
- **Error Metric**: Root Mean Square Error - why are we using this.
    


[^1]: _This calculation is based on the projections made by the Institute for Health Metrics and Evaluation (when it was last updated on April 29th). IHME no longer reports projected peak date. This [report](https://www.businessinsider.com/map-when-each-state-will-experience-coronavirus-peak-outbreak-2020-4) gives us the exact dates the peaks were predicted by the IHME model. And since we are assuming we are in the 3rd week of April, we make these calculations accordingly.However, because these are just projections and not actual values and with [new](https://news.utexas.edu/2020/04/17/new-model-forecasts-9-states-likely-to-see-peak-in-covid-19-deaths-by-end-of-april/) COVID-19 models coming up, such as the one developed by researchers at the [University of Texas-Austin](https://covid-19.tacc.utexas.edu/projections/) using geolocation data from cellphones. In turn, they try to see how the state-level social distancing measures can be used to project number of COVID-19 deaths per day, along with associated probabilities of whether the peak has reached or not._






