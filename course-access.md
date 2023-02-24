# Course Access

If you need a list of the dates when students accessed a course, and whether they accessed it from a browser or from the Pulse app, you can append the Course Access and Course Access Log data sets. Course Access only logs once per day for access through a browser, while Course Access Log logs the initial access and every 30 minutes that the students remain active in the course through the Pulse app. This data flow will remove the extra 30 minute logs so that there can only be one row per day per source (Browser or Pulse).

## Return Days that Students Accessed Course

To create the ETL data flow in Domo Analytics:

1. Start with Course Access
2. Alter Column: Select DayAccessed under column and cast data type as Date
3. Add Constants: 1) enter Source as a new column, 2) Text, 3) enter Browser as the constant
4. On the second branch, start with Course Access Log
5. Alter Column: Select Timestamp under column, rename to DayAccessed, and cast data type as Date
6. Remove Duplicates for OrgUnitId, UserId, and DayAccessed
7. Append Rows: 1) Include all columns, 2) Use best compatible data type
8. Inner join with Users on UserId
9. Filter Rows on OrgUnitId
10. Select needed columns (such as OrgUnitId, Username, FirstName, LastName, DayAccessed, and Source)

![ETL data flow for Return course access log per student per course](https://jenniferlynnwagner.com/img/etl/domo-etl-course-access.png)
