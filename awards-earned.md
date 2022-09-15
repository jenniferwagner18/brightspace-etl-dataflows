# Awards Earned

The awards tool in Brightspace by D2L includes first and last names of students who have earned awards, but it does not include usernames. The following data flow will include usernames as well as the total number of awards each student has earned in a course. Determining course completion based on the awards earned (set by release conditions) is an easier option when querying the data sets, rather than basing it on every content item visited/assignment submitted/quiz taken/etc. in a course.

## Return Number of Awards Earned

To create the ETL data flow in Domo Analytics: 

1. Start with Awards Issued, filter by the specific OrgUnitId
2. Inner join with Users on UserId
3. Group By the following: 1. select the columns UserName, FirstName, and LastName, 2. type a name for the New aggregated column (such as BadgesEarned), 3. select AwardId and Count
  
![ETL data flow for Return Number of Awards Earned as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-awards.png)
