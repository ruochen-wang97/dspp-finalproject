**Predicting Initial Unemployment Insurance Claims for last week of April using k-Nearest Neighbor and Random Forest Models.**
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

[^1]: _This calculation is based on the projections made by the Institute for Health Metrics and Evaluation (when it was last updated on April 29th). This [report](https://www.businessinsider.com/map-when-each-state-will-experience-coronavirus-peak-outbreak-2020-4) gives us the exact dates the peaks were predicted by the IHME model. And since we are assuming we are in the 3rd week of April, we make these calculations accordingly.However, because these are just projections and not actual values and with [new](https://news.utexas.edu/2020/04/17/new-model-forecasts-9-states-likely-to-see-peak-in-covid-19-deaths-by-end-of-april/) COVID-19 models coming up, such as the one developed by researchers at the [University of Texas-Austin](https://covid-19.tacc.utexas.edu/projections/) using geolocation data from cellphones. In turn, they try to see how the state-level social distancing measures can be used to project number of COVID-19 deaths per day, along with associated probabilities of whether the peak has reached or not._






