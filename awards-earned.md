# Awards Earned

The awards tool in Brightspace by D2L includes first and last names of students who have earned awards, but it does not include usernames. Determining course completion based on the awards earned (set by release conditions) is an easier option when querying the data sets, rather than basing it on every content item visited/assignment submitted/quiz taken/etc. in a course. The following data flows will include usernames as well as all awards earned and the total number of awards earned per student for the first option, and just the total number of awards earned per student for the second option.

## Return All Awards Earned per Student with Total Number

This option will return multiple rows per student with the total number of awards earned repeated across those rows. It will specify which awards the students have earned. To create the ETL data flow in Domo Analytics: 

1. Start with Awards Issued, filter by the specific OrgUnitId
2. Group By the following: 1. UserId, 2. type a name for the New aggregated column (such as TotalAwards), 3. AwardId, 4. Count
3. Inner join the filtered data with the grouped data on UserId
4. Inner join with Users on UserId
5. Select desired columns, such as Username, FirstName, LastName, Criteria (which will specify which award it is), and TotalAwards (column created in step 2)
  
![ETL data flow for Return All Awards Earned as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-awards-all.png)


## Return Total Number of Awards Earned per Student

This option will return one row per student with the total number of awards earned, but it will not specify which awards each student has earned. To create the ETL data flow in Domo Analytics: 

1. Start with Awards Issued, filter by the specific OrgUnitId
2. Inner join with Users on UserId
3. Group By the following: 1. UserName, FirstName, and LastName, 2. type a name for the New aggregated column (such as TotalAwards), 3. AwardId. 4. Count
  
![ETL data flow for Return Total Number of Awards Earned as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-awards.png)
