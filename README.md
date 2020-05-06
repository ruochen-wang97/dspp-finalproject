**Predicting the Number of New Unemployment Insurance Claims Filed for the Week Ending April 18**
=====

 _By Radhika Kaul, Odiche Nwabuikwu, & Ruochen Wang_

## Introduction
In face of the COVID-19 pandemic, many states in the United States have issued stay-at-home orders or mass gathering restrictions to prevent further spread of the disease. The unintended consequences of these restrictions have hit the labor market. Data from the federal government shows that following the enforcement of social distancing measures, late March has seen the largest surge in the filing of new unemployment insurance claims in the modern US history.

Understanding these surges at the state level has important policy implications as lay-offs and furloughs prevail in the labor market. This project aims to create a supervised machine learning model that predicts the number of new unemployment insurance claims filed per week to inform policymakers on the labor market trends in the immediate future.

## Data

We tested different models and decided to use data spanning from the week ending March 7, when the first death in the US was reported by the Centers for Disease Control and Prevention (CDC), to the week ending April 18, when the latest federal data are available.

The predicted and predictors variables in our dataset are from the following sources:

- [Unemployment Insurance Weekly Claims data](https://oui.doleta.gov/unemploy/claims.asp), administered by the U.S. Department of Labor (DOL)'s Employment & Training Administration
- COVID-19 tracking data from the [COVID-19 Data Repository](https://github.com/CSSEGISandData/COVID-19), administered by CSSE at Johns Hopkins University
- COVID-19 projection data from the [Institute for Health Metrics and Evaluation (IHME)](http://www.healthdata.org/) at the University of Washington
- State-level structural data from the [U.S. Census Bureau](https://www.census.gov)

### Codebook

_Real-time Variables_

- **initial_num**: outcome car. # of new claims filed for that week. 
- **continued_num**: # of insured unemployment for that week
- **days_since_sah**: # days between the last day of week and the day when state issued stay-at-home order
- **days_since_peak[^1]**: # of days between last day of week and  predicted death peak according to IHME
- **Incident_Rate**: # of confirmed COVID cases per 100,000 residents
- **Mortality_Rate**: # of reported deaths * 100/ # confirmed cases

_Structural Variables_

- **region**: census region
- **topind_gdp**: top industry in terms of % GDP contribution
- **topindemp_1**: top 1 industry in terms of % employment
- **topindemp_2**: top 2 industry in terms of % employment
- **topindemp_3**: top 3 industry in terms of % employment
- **totpop_2018**: total population in 2018
- **pctpop_work**: % population age 15-64
- **pctpop_hs**: % population with at least a high school degree
- **personal_inc2019**: per capita personal income in 2019
- **gov_republican**: dummy set equal to 1 if governor republican

[^1]: _Calculated based on projections made by IHME. Last updated on April 29 and is no longer being updated. See [here](https://www.businessinsider.com/map-when-each-state-will-experience-coronavirus-peak-outbreak-2020-4) for predicted peaks._

## Modeling

We constructed a Random Forest (RF) model to predict the number of new unemployment insurance claims filed for the week ending April 18 using the variables listed above.

- **Training and Testing Sets**: After researching existing time-series models, we decided to assign the data from the week ending April 18 to the testing set and all the other data from previous week to the training set. This allows us to predict the outcome for the week ending April 18 based on data from previous weeks.

- **Resampling**: bootstrap

- **Outcome Variable**: _initial_num_

- **Model 1**: 
    
- **Model 2**: Random Forest - In this model, a large number of _de-correlated_ trees, with pre-specified targets and features are used to generate predictions. More trees usually allude to the model having greater accuracy. It also does a good job in reducing the bias and the variance of the error metric.

    1) Selection 1: when mtry = 5,  ntree = 50
    2) Selection 2: when mtry = 20, ntree = 100
    3) Selection 3: when mtry = 50, ntree = 400
    
- **Error Metric**: Root Mean Square Error - why are we using this.