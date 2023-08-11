# INTRODUCTION
-- I used SQL & R for cleaning and for visualization.-- 

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

![156193153-24d0a864-da4b-44f1-9d4e-02b389650d3d](https://github.com/yylai93/yyCaseStudy/assets/35448935/ecd947f0-eb24-4dbc-9360-9be5d94a11b8)

### Merging Data for better understanding
```
merged_data <- merge(sleep, activity, by = c('Id', 'date'))
head(merged_data)
```
<img width="684" alt="196826489-0dc398f1-a3b5-4d35-9821-d98e10cfc279" src="https://github.com/yylai93/yyCaseStudy/assets/35448935/8f26e0fc-ce63-4129-a73d-48536bf3afc0">
<img width="582" alt="196826615-bcfe1e97-c012-4925-be30-511315abeaae" src="https://github.com/yylai93/yyCaseStudy/assets/35448935/d3a36ddd-7c6a-4399-8308-8b7bd174af42">
<img width="736" alt="196826760-53a97d4c-f1f2-444a-94a7-e60c66415120" src="https://github.com/yylai93/yyCaseStudy/assets/35448935/8dc620d3-d6e8-4548-b07b-eeb30644121a">
<img width="322" alt="196826888-ae1195f0-a406-48b6-8237-6890cdc1ea67" src="https://github.com/yylai93/yyCaseStudy/assets/35448935/1cb9d2c3-9ecd-49b1-86ff-0790eb7b6716">

# SHARE
```
--checking the relationship between totalSteps vs Calories
ggplot(data = activity, aes(x = TotalSteps, y = Calories)) + geom_point() + geom_smooth() + labs(title = "Total Steps vs. Calories")
```
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/f108da8d-b48b-4281-b1ec-aa96e30419d5)
From the scatter chart above, we can see that more steps each participant takes, the more calories they burn. which indicates a positive relationship. 
```
--checking the relationship between Activity duration and Calories burned
Select Id,
SUM(TotalSteps) as total_steps,
SUM(VeryActiveMinutes) as total_very_active_mins,
Sum(FairlyActiveMinutes) as total_fairly_active_mins,
SUM(LightlyActiveMinutes) as total_lightly_active_mins,
SUM(Calories) as total_calories
From daily_activity
Group By Id

ggplot(combined_data, aes(x = SedentaryMinutes, y = Calories.x)) + 
  geom_point(aes(color = "Sedentary")) +
  geom_point(aes(x = LightlyActiveMinutes, color = "Lightly Active")) + 
  geom_point(aes(x = FairlyActiveMinutes, color = "Fairly Active")) + 
  geom_point(aes(x = VeryActiveMinutes, color = "Very Active")) + 
  labs(x = "Activity Minutes", y = "Calories Burned", 
       title = "Relationship between Activity Intensity and Calories Burned",
       color = "Activity Intensity")
```
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/61c215b9-f03e-4fa6-b118-5ea258b3cea5)

![Fitbit dashboard](https://github.com/yylai93/yyCaseStudy/assets/35448935/895a6bef-ddc9-483c-a75f-277926cca638)
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/f159e849-9422-4a73-9db5-2cf741ea0525)


There is a strong correlation between Very Active minutes and the amount of calories burned. We can conclude that the higher the intensity and the duration of the activity, the more calories is burned.

### Sleep, sedentary time and Calories comparison
```
ggplot(data = sleep, aes(x = TotalMinutesAsleep, y = TotalTimeInBed)) + geom_point() + labs(title = "Total time asleep vs Total time in bed")
ggplot(data = merged_data, mapping = aes(x = SedentaryMinutes, y = TotalMinutesAsleep)) + 
  geom_point() + labs(title= "Sleep Duration and Sedentary Time")
cor(merged_data$TotalMinutesAsleep,merged_data$SedentaryMinutes)
```
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/6217be62-3b86-4365-a411-8e1f8daeecef)
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/216ee857-2b5f-4909-a68b-5d14b0f44cf1)
![image](https://github.com/yylai93/yyCaseStudy/assets/35448935/d5328e6c-8411-4e60-9fed-0b92b4821b7d)

-0.59939400560339
There is a negative correlation between SedentaryMinutes and TotalMinutesAsleep. The less active a participant is, the less sleep they tend to get.

# ACT/Suggestions
 1) The average number of steps per day is 7,638. This is lower than what the CDC recommends. According to CDC's official website: 8,000 steps per day was associated with a 51% lower risk for all-cause mortality (or death from all causes). Taking 12,000 steps per day was associated with a 65% lower risk compared with taking 4,000 steps. Bellabeat can suggest that users take at least 8,000 steps per day and explain the benefits that come with it. 
 2) Develop an alarm feature that alerts user when they should start their excercise based on a specific time.
 3) Provide app notification for users to remind them to get sufficient sleep every day and implement new sleep measurement features or products such as tracking Rapid Eye Movement (REM) sleep.
 4) Consider setting daily/weekly calorie challenges and award points to users based on the top performers.





