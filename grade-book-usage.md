# Gradebook Usage

This data flow will return the number of grade items per course and the number of grade items that have had any actions logged, as well as the grading system used and on which item the last action was logged, its type, the action, and the modified date. 

You can also omit steps 13 and 14 below if you'd like the full list of grade items per course and their types, rather than just one row per course with only the last action on one item. This can be useful to determine how often certain grade item types are used across your institution.

## Return Number of Grade Items, Number of Items with Actions, and Last Action Logged in Gradebook

1. Start with Organizational Units, select columns of OrgUnit, Type, Name, Code, and IsActive
2. Filter Rows: Include rows that meet **all** of the following rules - CourseCode starts with SS23- (this will depend on how your courses are labelled), Type = CourseOffering, and IsActive = True
3. For the second branch, start with Grade Objects and Filter Rows: Include rows that meet **all** of the following rules - IsDeleted = False and GradeObjectTypeId < 7 to filter out categories, final adjusted grade, and final calculated grade
4. Group By 1) OrgUnitId, 2) enter NumGradeItems as a new column name, 3) select GradeObjectId and Count
5. Inner self join with the previous filter tile on OrgUnitId
6. Inner join the two branches together on OrgUnitId
7. Left outer join with Grade Objects Log on GradeObjectId
8. Inner join with Gradebook Settings on OrgUnitId
9. Rank & Window: 1) name new column LastGraded and use the function Row Number, 2) Modified, 3) Descending, 4) OrgUnitId and GradeObjectId
10. Filter Rows on LastGraded = 1
11. Group By 1) OrgUnitId, 2) enter NumModified as new column name, 3) select Modified and Count
12. Inner self join with previous filter tile on OrgUnitId
13. Rank & Window: 1) name new column LastUpdated and use the function Row Number, 2) Modified, 3) Descending, 4) OrgUnitId
14. Filter Rows on LastUpdate = 1
15. Select needed columns

![ETL data flow for Gradebook Usage](https://jenniferlynnwagner.com/img/etl/domo-etl-gradebook-usage.png)

If there are no actions (NumModified = 0) or the action occurred before the start of the semester (dates are in the Modified column), then it is relatively safe to assume that the gradebook is not being used in the course. In this case, the grade items were most likely copied over from a previous semester instead of being created in the current semester.
