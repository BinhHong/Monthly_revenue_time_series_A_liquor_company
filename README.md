[<img src="https://deepnote.com/buttons/launch-in-deepnote.svg">](https://deepnote.com/workspace/binh-hong-ngoc-a131-f8796abb-e60b-480f-8957-da759bc1ea9e/project/Time-series-fb855f0b-5d5f-43d2-a9d9-b4b5150d059e/%2FMonthly_revenue_time_series_A_liquor_company%2FMonthly_revenue.ipynb)

# Monthly revenue time series of a liquor company

## Introduction
In this project, I consider a time series of monthly revenue of Jim Beam Brands, a liquor company in Iowa State, USA. First of all, I explore the important characterizations of this time series, such as decompositions (both multiplicative and additive), stationary property, seasonal plot ... In the next step, I make some forecasts based on two models: SARIMA and Prophet. Trend changes detection, cross validation and more importantly, the hyperparameters tuning are also included.

## Dataset
The dataset used in this project is a part deduced from a huge dataset at [Iowa’s state-hosted open data portal](https://data.iowa.gov/Sales-Distribution/Iowa-Liquor-Sales/m3tr-qhgy), which has around 24 million rows and 24 columns.

For the purpose of this project, I filtered the vendor named `Jim Beam Brands`, resulting in `2180745` rows. The following columns are considered:
- `Date`: date that bottles were sold to retailers.
- `Item Description`: names of the sold bottles.
- `Bottle Volume`: volume of each bottle.
- `State Bottle Cost`: cost the Iowa State pays to Jim Beam Brands for sold bottles.
- `State Bottle Retail`: money the State receives from selling bottles to retailers.
- `Bottle Sold`: number of bottles sold to retailers.

## Methodologies

- After cleansing the data, I created a time series of monthly revenue of `Jim Beam Brands` and made some visualizations:

<img src="https://user-images.githubusercontent.com/95805829/166657971-30ac3b91-f191-4e35-82c4-27d71a1aadf8.png" width=80% height=60%>

A first glimpse of seasonality as well as the distribution of revenue are also explored.

- For better understanding, I check the stationary property, seasonality and make decompositions in both multiplicative and additive cases.
- Finally, the forecasts are carried out based on two models: `SARIMA` and `Prophet`. 

In SARIMA, I use `auto_arima` to search for best possible parameters `p`, `q`, `d` and test the result on a training set. Due to fact that this dataset has unexpected trend changes and outliers, two training sets are used to validate the accuracy.

With Prophet model, a cross-validation is implemented, and later used in hyperparameters tuning, including `changepoint_prior_scale` and `seasonality_prior_scale`. These parameters are then put into the forecast to effectively the `RMSE` error.

## Results

### 1. The seasonality can be somewhat suggested by seasonal plot

<img src="https://user-images.githubusercontent.com/95805829/166658125-c8d4b0b3-0552-46c3-ab80-24e3f47c1fed.png" width=80% height=60%>
and the box plots:
<img src="https://user-images.githubusercontent.com/95805829/166658018-83209525-df3c-4b67-a2ff-0216a1922ccc.png" width=80% height=60%>

### 2. Decomposition is done by `seasonal_decompose` from `statsmodels.tsa.seasonal`

<img src="https://user-images.githubusercontent.com/95805829/166658044-cfc6f541-12e6-43f5-a973-4d1b44e0d460.png" width=80% height=80%>

### 3. Forecasts with SARIMA model

Forecast on a training set containing 80% of our dataset:

<img src="https://user-images.githubusercontent.com/95805829/166658092-4d0ee429-1d58-43b4-ba0a-fd8b864e8f85.png" width=80% height=40%>

The errors in this case are:

<img src="https://user-images.githubusercontent.com/95805829/166663030-8a014723-d455-401b-be9e-e86f40301686.png" width=20% height=10%>

On another training set containing info of outliers, the result is better

<img src="https://user-images.githubusercontent.com/95805829/166658106-d80421b1-a887-444b-806a-3de339179b81.png" width=80% height=40%>

with better errors

<img src="https://user-images.githubusercontent.com/95805829/166663040-bef27914-65bd-4cbb-8536-5b95a640fd9b.png" width=20% height=10%>

### 4. Forecasts with Prophet model

The first forecast is done without hyperparameters indicated:

<img src="https://user-images.githubusercontent.com/95805829/166658065-54349f7e-93f8-4afe-864b-ee76f3fcf498.png" width=80% height=40%>

We get an improvement of reducing `RMSE` error after hyperparameters tuning:

<img src="https://user-images.githubusercontent.com/95805829/166658078-2e36b91c-c822-424a-8af9-5667811ad6ce.png" width=80% height=40%>

The `RMSE` error drops from 178302.105 to 150059.697.
