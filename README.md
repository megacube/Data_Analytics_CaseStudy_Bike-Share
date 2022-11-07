# Data_Analytics_CaseStudy_Bike-Share
Step 1 - Ask: 
Statement of the Business Task
Bike-Share’s management is facing the challenge of maximizing the number of annual memberships. Until now the Company’s marketing strategy relied on attracting their customers via the flexibility of its pricing plans: single-ride passess, full-day passes, and annual memberships. The former two types of customers are referred to as casual riders; and the latter one type is referred to as Cyclistic members. 

Questions to Answer: 
To achieve the goal of maximizing annual memberships, an opportunity exists in converting casual riders into annual members. In order to do that, the marketing team needs to understand below questions:
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclists use digital media to influence casual riders to become members?

This case study is focusing on solving the first question above: How do annual members and casual riders use Cyclistic bikes differently? This lays the foundation for question 2 and 3, which will provide key features that the marketing can focus on improving and provide user experiences that can help convert casual riders to Cyclistic members. 

Key Stakeholders:
The key stakeholders for this analysis are:
- Lily Moreno, Director of Marketing
- Cyclistic executive team

Step 2 - Prepare: 

# - Install all necessary libraries

install.packages("librarian")

librarian::shelf(tidyr, dplyr, here, lubridate, geosphere, ggplot2)

# - Data exploration - I used most recent data as my example to see what’s included:
> sep_2022_biketrips <- read.csv("202209-divvy-publictripdata.csv")
> head(sep_2022_biketrips)
> glimpse(sep_2022_biketrips)
I then used the summary function to get an overview of the data and noted that there are 13 columns and 701339 

> summary(sep_2022_biketrips)
Further noted that there were columns “started_at” and “ended_at” for datetime that were stored as characters. I used the * lubridate* library to convert and then we can calculate the length of each ride
Also noted that there were latitude and longitude of the startpoint and endpoint in the columns “start_lat”, “start_lng”, “end_lat” and “end_lng. Using these columns we can calculate the ride distance using the “geosphere” library
There are rows with NA’s which we need to consider when we clean-up our dataset
With all these consideration in mind, my next step is to combined all the files into one dataset and then clean-up my dataset for the past 12-months.

1. Where is your data located?
The data is located on a website hosted by AWS
2. How is the data organized?
- The data is organized by periods ranging from 2013 to current
- Each period (ether quarterly or monthly) a excel file is saved on the website
3. Are there issues with bias or credibility in this data?
- The data collected is from the City of Chicago. It may not be representative the whole US or the whole market
- Only certain Divvy system data are allowed by the City to disclose to the public. These data may be biased due to the restriction imposed by the City
- Only the data covered by the Divvy systems are collected and it left out those people who live in the area are not covered by the systems
4. How are you addressing licensing, privacy, security, and accessibility?
- The data has been made available by Motivate International Inc. under their license: https://ride.divvybikes.com/data-license-agreement
- I read through the license agreement and signed
- I make sure the use of the data is purely for learning purposes. No commercial use for any of these information
- I make sure the use of the data in in compliance with the agreement
5. How did you verify the data’s integrity?
- I did a search on the data provider and ensure that it is a legit business
- I checked the data links and noted these are hosted by AWS
- I clicked the data links and noted that a download popping up and other people cannot make edits / modification on the data
6. How does it help you answer your question?
- From steps 1- 5, I make sure the data I used is from a legit source by checking the website and the Company who published the data
Also, I understand the potential bias of the data. When we generalize our findings, we have to put a limit on our scope
- From the way data organized, I am able to make a plan to manipulate the data
7. Are there any problems with the data?
- Yes. There are some biases of the data: 
- Not all areas in US are covered in this data
- Not all areas in Chicago are cover in this data
- Only certain data are allowed to be disclosed by Chicago City and we don’t know what are the restrictions 

Step 3 - Process: 
1. What tool are you choosing and why?
- For this project, I choose R to analyze so that I can practice my skill sets in R.
- All the data are stored in the same format and it is very easy to load into R 
- The data name convention is also very consistent which make it easier to combined all the data. Noted that 202209 data is slightly different. I renamed the file and make it consistent with other file.
- R also has the visualization tool which can help visualize when doing analysis 
- The individual file has a lot of data and it makes sense to use R program language
2. Have you ensured your data integrity?
- I checked the data source in the preparation stage and make sure the source is reliable
- I took most recent period as a sample to check the data and ensure the format is consistent and meaningful
- For completeness, we are looking at 12 months. Noted that the data has been updated on either a monthly or quarterly basis since 2004. I choose the most recent past 12 months to ensure the timeframe is consistent and complete. 
3. What steps have you taken to ensure that your data is clean?
- I have concatenated the most recent 12 months CSV files into a single data frame
- Check the number of empty rows and columns and noted that it was not a significant part of the whole dataset. Therefore, it is appropriately to remove these empty rows without impact the results
- Check values in each column and ensure no duplicates. If there is any duplicates, remove them 
4. How can you verify that your data is clean and ready to analyze?
- Use filter() to check if there is any missing values
- Use count() to check if values for each variable is unique
- Use duplicated() to check for any duplicated values
5. Have you documented your cleaning process so you can review and share those results?
- Yes

Step 4 - Analyze: 
1. How should you organize your data to perform analysis on it?
- I downloaded the most recent 12 months data to perform analysis. However, they are in separate files. I need to combine them into one dataframe to perform my analysis.
2. Has your data been properly formatted?
- I took a sample of the most recent month: September data to glimpse through the format of data and noted that the start_at and end_at columns are stored as characters instead of datetimes. Therefore, I need to convert these columns
- I also noted that there are missing rows in the data. Given the size of the data I feel it is appropriate to drop these rows
- To better understand the relationship, I added two calculation:
   - Week_of_day: convert the time to the day in the week to understand when riders use the bikes
   - Ride_length: calculated the length of each ride using the started_at and ended_at columns
   - Distance: calculated the distance using the start_lng, start_lat, and end_lng, end_lat)
3. What surprises did you discover in the data?
- I thought the casual riders would have significantly less riding time or frequency. However, overall, casual riders was 40.3% of all trips while member riders was 59.7% 
- By displaying the data, I noted that on weekends (Saturday and Sunday),  Casual riders use more than member riders in distance on average. 
- Only casual riders use docked bike and they also used electric bike more than member riders
4. What trends or relationships did you find in the data?
- On average, casual riders ride bike spend more time than member riders
- On average, member riders ride more frequently than casual riders
- Member riders tend to use the classic bike while casual riders use all types of bikes
- Only casual riders use docked bikes
- On weekdays, member riders use bikes more than casual riders in distance
- Over the weekends, casual riders used bikes more than member riders in distance
5. How will these insights help answer your business questions?
- From the above trends, my observation is that casual riders have a need to use bikes which provides us the opportunity to convert the casual riders to member 
riders. 
- Only casual riders use docked bikes. This provides an opportunity for us to increase casual riders by placing more dock bikes in the system to attract them to an increasing user base. 
- We can convert the casual riders to member riders given the needs. However, we need more data such as age, professionals, ethic group, distance from work, experiencing in riding etc. to help us build up a portfolio for member riders. So that we can target the casual riders better to help us convert them to member riders
- For pricing, we can use different prices or charge by riding distance as both groups tend to use more over the weekends

Below are the main codes that I uesd in this project:


# - Install all necessary libraries

install.packages("librarian")
librarian::shelf(tidyr, dplyr, here, lubridate, geosphere, ggplot2)

# - Data exploration - I used most recent data as my example to see what’s included:
> sep_2022_biketrips <- read.csv("202209-divvy-publictripdata.csv")
> head(sep_2022_biketrips)
> glimpse(sep_2022_biketrips)

# I then used the summary function to get an overview of the data and noted that there are 13 columns and 701339 
> summary(sep_2022_biketrips)
I renamed Sep_2022 to 202209-divvy-tripdata to keep the consistency of the name conversion and combined all the 12 - months data into one dataset:
df1 <- read.csv("202110-divvy-tripdata.csv")
df2 <- read.csv("202111-divvy-tripdata.csv") 
df3 <- read.csv("202112-divvy-tripdata.csv") 
df4 <- read.csv("202201-divvy-tripdata.csv")
df5 <- read.csv("202202-divvy-tripdata.csv")
df6 <- read.csv("202203-divvy-tripdata.csv")
df7 <- read.csv("202204-divvy-tripdata.csv")
df8 <- read.csv("202205-divvy-tripdata.csv")
df9 <- read.csv("202206-divvy-tripdata.csv")
df10 <- read.csv("202207-divvy-tripdata.csv")
df11 <- read.csv("202208-divvy-tripdata.csv")
df12 <- read.csv("202209-divvy-tripdata.csv")
binded_df <- rbind(df1, df2, df3, df4, df5, df6, df7, df8, df9, df10, df11, df12)
# - To format the data, I then converted the started_at and ended_at columns to datetime and calculated ride_lenght, day_of week, and distance
all_trips <- binded_df %>%
  mutate(started_at = ymd_hms(started_at)) %>%
  mutate(ended_at = ymd_hms(ended_at)) %>%
  mutate(ride_length = ended_at - started_at) %>%
  mutate(day_of_week = weekdays(started_at)) %>%
  mutate(distance = distHaversine(cbind(start_lng, start_lat), cbind(end_lng, end_lat)))
# - I cleaned up the data by filter out all the blank rows
all_trips_2 <- all_trips %>%
   select(ride_id, 
              rideable_type, 
              started_at, 
              ended_at, 
              member_casual, 
              ride_length, 
              day_of_week, 
              distance) %>%
   filter(distance > 0) %>%
   filter(ride_length > 0) 
Below is a summary of all_trips cleaned up data

# - Due to the volume of data, my R cannot handle the platting. Therefore I took a random sample of the dataset to get a flavor of the information:
subset_trips <- sample(all_trips_2, 1000)
subset_trips <- all_trips_2[sample(nrow(all_trips_2), 1000, replace = FALSE, prob = NULL),]
summary of the subset

# - First plot the sample by rideable type
subset_trips %>%
ggplot(aes(x = rideable_type, y = ride_length, fill = member_casual)) +
  geom_col(position = "dodge")

# - Then plot the sample by week day of number of ride
subset_trips %>% 
  mutate(day_of_week = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, day_of_week) %>% 
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>% 
  arrange(member_casual, day_of_week)  %>% 
  ggplot(aes(x = day_of_week, y = number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge")

# - Then plot the sample by week day of distance

subset_trips %>% 
  mutate(day_of_week = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, day_of_week) %>% 
  summarise(distance = n()
            ,average_duration = mean(distance)) %>% 
  arrange(member_casual, day_of_week)  %>% 
  ggplot(aes(x = day_of_week, y = distance, fill = member_casual)) +
  geom_col(position = "dodge")
# To get more description information I did below:

aggregate(all_trips_2$ride_length ~ all_trips_2$member_casual, FUN = mean)
aggregate(all_trips_2$ride_length ~ all_trips_2$member_casual, FUN = median)
aggregate(all_trips_2$ride_length ~ all_trips_2$member_casual, FUN = max)
aggregate(all_trips_2$ride_length ~ all_trips_2$member_casual, FUN = min)

aggregate(all_trips_2$ride_length ~ all_trips_2$member_casual + all_trips_2$day_of_week, FUN = mean)
all_trips_2$day_of_week <- ordered(all_trips_2$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
