---
layout: post
title: Time Series (pandas)
description: >
  pandas
tags: [pandas]
use_math: true
categories:
  - study
  - computer science
---
### Time Series (pandas)
* Array elements' dtypes : Timestamp or Period
  * Basic structure : year-month-day (ex. 2024-01-02)
  * Timestamp : At time point
    * form : y-m-d or y-m-d time
  * Period : time interval
    * form : y or y-m or y-m-d or y-m-d time
* If we set time series (one column) as index of dataframe, it is convenient to deal with these
  * only extracting year series or month series or day series
  * accessing the specific records using time series elements

### Make Time Series
* Way 1
  * **Existing string column(Series)** representing time (y-m-d) → Time Series (dtypes : timestamp)
  * **pd.to_datetime(df[column's name] or any array like list)**
    * return Series whose data type is timestamp
  * If we wanna change from timestamp series to period series
    * **(timestamp_series).to_period(freq=)**
      * return Series whose data type is period
      * freq='D' : y-m-d
      * freq='M' : y-m
      * freq='A' : y
* Way 2
  * Create Time Series directly
  * **pd.date_range(start=, end=, periods=, freq=, tz=)**
    * ex. start='2024-01-01' : Time Series' first element is '2024-01-01'
    * end= : User can determine the end element directly (end=None : periods parameter will determine it)
    * periods=n : The number of elements in Time Series is n
    * freq=
      * ex. freq='3A' (year end) : 2024-12-31, 2027-12-31, 2030-12-31, ...
      * ex. freq='2AS' (year begin) : 2024-01-01, 2026-01-01, 2028-01-01, ...
      * ex. freq='2M' (month end) : 2024-01-31, 2024-03-31, 2024-05-31, ...
      * ex. freq='3MS' (month begin) : 2024-01-01, 2024-04-01, 2027-07-01, ...
      * ex. freq='2D' : 2024-01-01, 2024-01-03, 2024-01-05, ...
    * ex. tz='Asia/Seoul' : 00:00:00+09:00
  * **pd.period_range(start=, end=, periods=, freq=)**
    * ex. start='2024-01-01' : Time Series' first element is '2024-01-01' (Its form can be changed because it is period dtypes)
    * end= : User can determine the end element directly (end=None : periods parameter will determine it)
    * periods=n : The number of elements in Time Series is n
    * freq=
      * ex. freq='3A' : 2024, 2027, 2030, ...
      * ex. freq='2M' : 2024-01, 2024-03, 2024-05, ...
      * ex. freq='2D' : 2024-01-01, 2024-01-03, 2024-01-05, ...
      * ex. freq='2H' : 2024-01-01 00:00, 2024-01-01 02:00, 2024-01-01 04:00, ...

### Use Time Series
* We can only extract year series or month series or day series
  * **df['year'] = df[timeSeriesColumn's name].dt.year** : add new column ('year') whose data is year series
  * **df['month'] = df[timeSeriesColumn's name].dt.month** : add new column ('month') whose data is month series
  * **df['day'] = df[timeSeriesColumn's name].dt.day** : add new column ('day') whose data is day series
  * **df['year_month'] = df[timeSeriesColumn's name].dt.to_period(freq='M')** : add new column ('year_month') whose data is y-m series
* If we set Time Series column as index of dataframe (set_index)
  * Method for accessing element
    * ex. **df.loc['2024']** : return dataframe whose index contains year as 2024
    * ex. **df.loc['2024-01']** : return dataframe whose index's form is 2024-01
    * ex. **df.loc['2024-01' : '2024-05', 'c1' : 'c3']** : return dataframe (index : 2024-01 ~ 2024-05, column : c1 ~ c3)
  * Difference between two dates
    <br>

    ~~~
    standard_date = pd.to_datetime('y-m-d (example)')
    df['difference'] = standard_date - df.index(timeSeries)
    ~~~
