# INTRODUCTION
-- I used R for cleaning and for visualization.-- 

Google-Data-Analytics-Capstone (Case 2)
THIS IS THE CAPSTONE PROJECT FOR GOOGLE DATA ANALYTICS CERTIFICATE.

Bellabeat is a high-tech manufacturer of health-focused products for women. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits. Although Bellabeat is a successful small company, they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company.

# ASK

To identify potential opportunities for growth and provide recommendations for the Bellabeat marketing strategy improvement based on trends in smart device usage.

Questions to explore for the analysis:

1 What are some trends in smart device usage? 

2 How could these trends apply to Bellabeat customers?

3 How could these trends help influence Bellabeat marketing strategy?

# PREPARE

The data set contains 18 CSV files organized in long format. After running it thru ROCCC method, below is the result: 

##### Reliability - LOW: `only 30 fitbit users who consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring.`

##### Original - LOW: `Third party data collect using Amazon Mechanical Turk.`

##### Comprehensive - MED: `dataset contains multiple fields on daily activity intensity, calories used, daily steps taken, daily sleep time and weight record.`

##### Current - LOW: `data is collected from March 2016 through May 2016, which is not current and user habits may have changed. 

##### Cited - LOW: `data is collected by a third party organization`

### loading packages 

```
install.packages & library(tidyverse)
library(lubridate) 
library(dplyr)
library(ggplot2)
library(tidyr)
install.packages & library(janitor)
```

### Importing Datasets

```
activity <- read_csv("dailyActivity_merged.csv")
calories <- read_csv("dailyCalories_merged.csv")
intensities <- read_csv("dailyIntensities_merged.csv")
sleep <- read_csv("sleepDay_merged.csv")
weight <- read_csv("weightLogInfo_merged.csv")
```
#### Data Cleaning
```
head(activity)
colnames(weight)
```
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/52eef278-7000-4cbe-bb4b-576c4b7e06c9)

I started with viewing the data frames, checking the similarities in elements across different data frames. 

# Analyze

I first check to see how many participants are in each category. 

```
n_distinct(activity$Id)  
n_distinct(calories$Id)   
n_distinct(intensities$Id)
n_distinct(sleep$Id)
n_distinct(weight$Id)
```
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/211fae19-8211-4337-832b-b2e07aecf2b1)

There are 33 participants in the activity, calories, and intensities datasets, 24 in the sleep dataset, and only 8 in the weight dataset.

More data needed especially on the weight dataset since only 8 participants recorded. 

```
to check if there's any significant change in weight.

![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/e6ef1093-7864-45e2-8e3b-adef46d1a658)

There's no significant changes in weight, with low data count, it is advisible to drop this dataset.

```
Let's check on other datasets. 

```
activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes, Calories) %>%
  summary()

calories %>%
  select(Calories) %>%
  summary()

sleep %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()

weight %>%
  select(WeightKg, BMI) %>%
  summary()
```
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/9356d6b8-b6a9-4978-b4d0-29f9fbcd15ec)
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/9eff2d54-5fb3-40dc-ad48-3a80dc2aa30a)
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/43c6c43e-f924-46f7-80ae-7fe9d7c249fe)

#### Based on the data summary
1) The average number of steps per day is 7638.
2) The average calory burns are 97  per hour.
3) Participants sleep for 7 hours in average.
4) Majority of users spent most time in sedetary state, 16.5 hours.



