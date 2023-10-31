# Case Study:How does a bike share navigate Speedy success?(RStudio and Tableau)
## Introduction

This project has been prepared as the capstone assignment for the Google Data Analytics Professional Certificate program. The goal of this program is to prepare participants for a career in the field of data analytics by teaching them key analytical skills such as data cleaning, analysis, and visualization as well as tools such as Excel, SQL, R Programming, and Tableau in order to prepare them for the competitive job market.This project will analyze publicly available datasets provided by the course.

## table of contant 
- [About company ](#about-company )
- [Ask](#ask)
- [Prepare](#prepare)
- [Process](#process)
- [Analyze and Share ](#analyze-and-share )
- [Act](#act)

  
## About company 

Cyclistic is a fictional bike sharing company in Chicago. In total, there are more than 5,800 bicycles and 600 docking stations throughout the city. bikes can be unlocked from one station and returned to any other station in the system at anytime. The cyclistic offers three different price plans: single-ride passes, full-day passes, and annual memberships. Customers who buy a single-ride pass or full-day pass are considered casual riders, while those who purchase annual memberships are considered cyclistic members. 

For future growth, Cyclistic prioritizes the conversion of casual riders into annual members based on a financial analysis that indicates annual members are more profitable than casual riders. As the director of the marketing team and manager, Lily Moreno, also believes that in order to grow in the future, it is important to maximize the number of annual members.In this Scenario my position is a Junior Data Analyst in marketing analyst team.



**Key Stakeholders of this analysis:**


* The Director of Marketing
* Marketing Analytics team and
* Executive team


This analysis follows Google's data analysis process for ensuring a structured and comprehensive analysis, which consists of six main steps: Ask, Prepare, Process, Analyze, Share, and Act. 

## Ask

The main business task in this project is to develop marketing strategies aimed at converting casual riders into annual members.

**Business Questions:**

* How do annual members and casual riders use Cyclistic bikes differently?

* Why would casual riders buy Cyclistic annual memberships?

* How can Cyclistic use digital media to influence casual riders to become members?


Since I’m tasked with answering the first question, my analysis will focus on comparing annual members and casual riders.

## Prepare 
Our primary data source is obtained from Cyclystic trip data, licensed under Motivate International Inc.This dataset is a quantitative measurement collected from bike trackers.This dataset does not contain any personal information.

**Data credibility (ROCC):**

* **Reliable:** For the sake of this analysis, we can ignore incomplete entries since they are less than 0.1% of all entries.

* **Original:** The company Cyclistic gathers this data directly.


* **Comprehensive:** There are more than 5 million complete data entries in the dataset.

* **Current:** As this data includes data from the past 12 months, it is up-to-date and accurate.


**Data Description**

In further analysis, it is observed that all files have the same 13 columns, and the data types are consistent. These columns are:


**Limitations-**
It is not possible to determine if casual riders live in the service area and purchased multiple single passes because past purchases to credit card numbers has been de-personalized to protect the privacy of users.

## Process
**Tools used for processing data**

Spreadsheets are not suitable for this project due to the large amount of raw data in this dataset. R Studio is the best used for this project as it has a wide range of tools and can easily handle large amounts of data.RStudio used to clean, analyze and aggregate the large amount of monthly data stored in the bike trip folder.

#### Cleaning and processing:

To begin, let's create a new script or query, which will act as a workspace for performing the necessary operations on cyclic data.Then we need to install the nesselary packages.In this we need tidyverse for data import and wrangling,libridate for date functions and ggplot for visualization. 

```r
install.packages("tidyverse")
install.packages("lubridate")
install.packages("ggplot2")
```
Then, We need to lord thepackages using library function.

```r
library(tidyverse)
library(lubridate)
library(ggplot2)
```
After that, first of all, the working directory should be set to the path of the folder created for it.  
```r
getwd()
setwd("C:/Users/DELL/Desktop/Cyclistic_bike_trip/CSV")
```
The CSV file in the folder must be uploaded to be used for the process, and here the read _CSV function is used for that.
```r
trip_202210 <- read_csv("202210-divvy-tripdata.csv")
trip_202211 <- read_csv("202211-divvy-tripdata.csv")
trip_202212 <- read_csv("202212-divvy-tripdata.csv")
trip_202301 <- read_csv("202301-divvy-tripdata.csv")
trip_202302 <- read_csv("202302-divvy-tripdata.csv")
trip_202303 <- read_csv("202303-divvy-tripdata.csv")
trip_202304 <- read_csv("202304-divvy-tripdata.csv")
trip_202305 <- read_csv("202305-divvy-tripdata.csv")
trip_202306 <- read_csv("202306-divvy-tripdata.csv")
trip_202307 <- read_csv("202307-divvy-tripdata.csv")
trip_202308 <- read_csv("202308-divvy-tripdata.csv")
trip_202309 <- read_csv("202309-divvy-tripdata.csv")
```


Before everything, it should be checked whether the columns in all the files here have the same names.  The reason for that is because it makes it easier to add that column to the same column when adding all these files.  Colname was used for that.
```r
colnames(trip_202210)
colnames(trip_202211)
colnames(trip_202212)
colnames(trip_202301)
colnames(trip_202302)
colnames(trip_202303)
colnames(trip_202304)
colnames(trip_202305)
colnames(trip_202306)
colnames(trip_202307)
colnames(trip_202308)
colnames(trip_202309)
```
In joining together, the relevant column should be in the same format. After that,we check the format of these columns to make sure that they are in the same format.  Str can be used for that.
```r
str(trip_202210)
str(trip_202211)
str(trip_202212)
str(trip_202301)
str(trip_202302)
str(trip_202303)
str(trip_202304)
str(trip_202305)
str(trip_202306)
str(trip_202307)
str(trip_202308)
str(trip_202309)
```


Afterwards, what needs to be done is stacking the individual monthly data frames into one single frame.
```r
all_trips <- bind_rows(trip_202210,trip_202211,trip_202212,trip_202301,trip_202302,trip_202303,trip_202304,trip_202305,trip_202306,trip_202307,trip_202308,trip_202309)
```
As the next step, a inspect should be performed on the newly created table to see if everything is in order.
```r
colnames(all_trips)  #List of column names
nrow(all_trips)  #How many rows are in data frame?
dim(all_trips)  #Dimensions of the data frame?
head(all_trips)  #See the first 6 rows of data frame.  Also tail(qs_raw)
str(all_trips)  #See list of columns and data types (numeric, character, etc)
summary(all_trips)  #Statistical summary of data. Mainly for numerics
```
Since no errors are visible in each column and with data types in general, we can now proceed to clean up the data set.Then a new data frame with clean data is created and the new data frame is named as cyclic_data.
```r
cyclistic_data <- all_trips %>%
select("ride_id","rideable_type","started_at","ended_at","start_station_name","start_station_id","end_station_name","end_station_id","start_lat","start_lng","end_lat","end_lng","member_casual") %>%
  na.omit()%>%
  mutate(trip_length =as.numeric(difftime(ended_at,started_at,units="mins")),
         month = format(as.Date(started_at),"%m"),
         day = format(as.Date(started_at),"%d"),
         year= format(as.Date(started_at),"%Y"),
         day_of_week= format(as.Date(started_at),"%A"))
```

- **cyclistic_data <- all_trips %>%** - According to the line of code, the all_trips output is being passed through to the subsequent operations, which will be applied to it step by step. After these operations have been carried out, the resulting data frame is assigned to the object cyclistic_data, creating a new dataset.

- **select("ride_id","rideable_type","started_at","ended_at","start_station_name","start_station_id","end_station_name","end_station_id","start_lat","start_lng","end_lat","end_lng","member_casual") %>%**  -This function is used to select specific columns from the output of the all_trip that is being passed as an input to the function.

- **na.omit()%>%** -By using the na.omit() function, all the rows with missing values (NA) will be removed from the columns of the aggregate_data, which will help clean and remove all the data that will affect the analysis.

- **mutate(trip_length =as.numeric(difftime(ended_at,started_at,units="mins")),
         month = format(as.Date(started_at),"%m")
         day = format(as.Date(started_at),"%d"),
         year= format(as.Date(started_at),"%Y"),
         day_of_week= format(as.Date(started_at),"%A"))** -It is possible to create columns based on trip length, month, day, year, and day of the week in which the trip was taken.These columns can be used for analysis to get key information.

In order to proceed with the analysis, we must first ensure that the trip_length column is prepared for analysis. In order to do this, we can use the min () and max () function, which are functions that are used when a ma
```r
min(cyclistic_data$trip_length)
max(cyclistic_data$trip_length)
```
![min max](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/84b98e1b-3ef8-4176-9faf-eacc25b205eb)

Basically, all of the numbers returned from functions are in minutes. However, there shouldn't be any negative values in the trip_length column, but for some reason, they tend to occur here. Therefore, we should clean out the trip_length column and filter it to contain only trip_length values that are larger than one minute.
```r
cyclistic_clean_data <-cyclistic_data %>%
  filter(trip_length>=1)
```

Now it is time to perform a descriptive analysis of this data frame using the summary() function.
```r
summary(cyclistic_clean_data$trip_length)
```
![sammary](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/20116885-f114-4728-94a7-20405cbfa384)

It is still important to recheck and make sure there are things that need to be cleaned out. First, it will be necessary for us to find any NA values in each column. To do this, we will be executing the following function:
```r
colSums(is.na(cyclistic_clean_data))
```
According to the data frame there are no NA values in any of the columns. 

![na](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/aa7e69ba-d8bc-45e6-9252-5323f23f53de)

Then , it should be checked for duplicate ride_id values so that they can be removed. When the returned value is FALSE, it means that there are no duplicate values in the data frame.
```r
any(duplicated(cyclistic_clean_data$ride_id))
```

![dupplicated](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/bf95ca18-30b6-430e-8f5e-854d3c77446d)

As a final step, inspect the structure of the new data frame to see if there are any data types that are not matching the type of data in the columns.
```r
str(cyclistic_clean_data)
```
![str](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/700d3531-ace9-4219-9e63-871183931b42)

After cleaning out the raw data set that had 5,674,399 rows, we now have a clean data set that has 4,203,068 rows and 18 columns . All of the data containing the correct data types. It is now time to analyze this clean data to see how it could be used.

## Analyze and Share 
Now that we have the cleaned data for further processing, the next step is to answer the given business question.  Below is the business question that needs to be answered.

**How do annual members and casual riders use Cyclistic bikes differently?**

The key data to help answer this question is obtained after cleaning.  Using available data it can be analyzed in various ways as follows.

**Comparing the number of rides per month**

Analyzing and comparing monthly ridership between casual riders and members is important to answering our business question.To do this, a data frame must be created in RStudio by extracting the columns from cycling_clean_data.  It requires the start_at column and theember_casual column. A row_count column should also be added to that data frame to count the bike rides for each month. This data frame is named month_count.  

```r
month_count=cyclistic_clean_data%>%
  group_by(month=month.name[month(started_at)],member_casual)%>%
  summarise(row_count=n())%>%
  arrange(match(month,month.name))
write.csv(month_count,"month count.csv",row.names = FALSE)
```
The first 6 row of the data frame can be previewed using the head() function:

![head](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/ac52aa39-651a-4d7b-9f0f-83245b4bac6d)

To get an easy understanding of the data, it was visualized using Tableau.
![Numbers of riders by month ](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/aba18002-b4c4-4988-ae07-6e03babbf9eb)

As can be seen from the visualization above, June, July and August have the highest number of rides for both casual riders and members.This pattern suggests that demand for bicycle use is higher during the summer season.  The generally low rainfall may have led to an increase in bicycle use. This visualization clearly shows that casual riders and members have used bikes more in these months.

**Comparing bike usage on days of the week**

This data can also be used to analyze how casual riders and members change their bike usage on different days of the week.Using the cyclic_clean_data, a new data frame will be created which contains the weekday and member columns and then create a new column to count the number of rides on a certain weekday.This data frame is named week_count.

```r
weekday_count=cyclistic_clean_data%>%
  group_by(day_of_week=day_of_week,member_casual=member_casual)%>%
  summarise(row_count=n())
write.csv(weekday_count,"weekday count.csv",row.names = FALSE)
```
The output of the code chunk above can be viewed using the View() function.

![weekdaycount](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/4ed235a8-ce90-49e0-926f-a8ee76a9d4e1)

In order to clearly understand what the trends are for casual riders and members, this data can be imported into Tableau for create a viz.

![bike usage on days](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/0c45c23f-2c17-4945-9d0d-8acb86f21981)

It can be seen clearly from this graph that there is a distinct difference in bicycle usage between casual riders and members.

- **Bike usage by casual riders peaks on weekends**- According to these findings, this could indicate casual riders are people who use their bikes for leisure and recreation purposes during their free time.This suggests they may be using the bikes for their own personal purposes and may find the bikes convenient for personal endeavors.
- **Bike usage by members peaks on weekdays** -This shows that many members use the bikes for their transportation and commuting purposes, and it may be that they are finding the bikes are convenient and reliable in terms of their transportation for their work and their weekday commitments.

**compare Top 5 starting and ending stations**

It is also possible to use this data in order to identify the top five locations for both casual riders and members, as well as to identify the most popular destinations for both groups to ride.This can be accomplished by creating the following two data frames.

```r
top_start_station<- cyclistic_clean_data%>%
  group_by(start_station_name,member_casual)%>%
  summarize(row_count=n())%>%
  arrange(desc(row_count))
write.csv(top_start_station,"top_start_station.csv",row.names = FALSE)

top_end_station<- cyclistic_clean_data%>%
  group_by(end_station_name,member_casual)%>%
  summarize(row_count=n())%>%
  arrange(desc(row_count))
write.csv(top_end_station,"top_end_station.csv",row.names = FALSE)
```
Currently, the data frame of the dataset has already been sorted in descending order, but here the top 5 positions of the data frame have not been filtered yet, and the top 5 positions can be filtered through Tableau.
![top 5 station casual](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/ef7f8fe1-ef6c-4a38-9598-8a1a83aef6dd)

![top 5 end station casual](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/9b305298-03c6-4ae1-b7d9-0d817cb1043c)

![top 5 station member](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/b2f92df1-ae86-4582-b7d7-3389d501c4a2)

![top 5 end station member](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/fe069438-91d2-4fa9-b658-45ea4d5ebb19)

**compare trip lenghts** 

It is possible to compare the trip lengths of casual riders and annual members in this analysis. This analysis can provide insight into the average ride length as well as whether there are significant differences in ride lengths between the two groups. To compare the trip lengths of the two groups, the following code is used:

```r
ride_lenght<-cyclistic_clean_data%>%
  group_by(member_casual,)%>%
  summarize(mean(trip_length))
write.csv(ride_lenght,"ride_lenght.csv",row.names = FALSE)
```

![avarage trip lenght](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/1a39beba-0903-4278-aac8-538717a62f5c)

Considering the data frame, casual riders have a longer trip length than members, with an average travel time of 23 minutes for casual riders and 12 minutes for members.

![mean by day](https://github.com/AhinsaSandarenu/Case-Study-How-does-a-bike-share-navigate-speedy-success-/assets/149358322/deffe654-3b37-4a85-a8b7-2cd7b2dacdeb)

When comparing bicycle use on different days of the week, the disparity in running distances between casual riders and members may provide some explanation for the former conclusion. The majority of casual bicycle riders use their bikes for leisure activities, exploration, and long excursions, which can result in a longer travel time.Additionally, members who primarily utilize the service for routine commuting or short travel have relatively short trip distances.

## Act

The data was analyzed in order to get an understanding of the different usage patterns of casual riders as well as members, and now the data is ready for the last step of the analysis process: act.Based on the findings of this analysis, the final step is to provide recommendations for the company's marketing strategies to convert casual riders into annual members. They are listed below.

**1. Campaigns during the summer:** 

- Casual riding is at its peak in June, July and August so it makes sense to capitalize on those months.In order to promote the benefits of annual membership, Allocate a significant portion of the marketing budget during these months . 
- Explain to casual riders that an annual membership is a more convenient and cost-effective solution during the summer season

**2. Weekend-targeted benefits:** 
- Offer benefits to members such as exclusive access to certain events and venues on the weekend, and coupons for restaurants as part of weekend offer.
- Make audience aware flexibility and convenience of cyclistic bikes for weekend adventures and leisure activities.


**3. Station-specific promotion:** 
- Focus on top spots frequented by casual riders, such as Streeter Dr & Grand Ave, DuSable Lake Shore Dr & Monroe St, Michigan Ave & Oak St, Millennium Park, DuSable Lake Shore Dr & North Blvd.
- distribute casual rider promotional materials in this stations. 
- Prominently display targeted advertisements at these locations.
- conduct on-site registrations and discounts at these locations to attract casual riders and encourage them to become annual members.



 

