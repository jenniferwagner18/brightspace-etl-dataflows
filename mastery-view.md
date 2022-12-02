# Mastery View of Learning Outcomes

If you are using the Standards tool with learning outcomes in your D2L courses, the Mastery View in the grade book does not currently have an export option. To generate a CSV file of the data in this view, use the following data flow with the Outcomes data sets. In addition to the outcomes and levels achieved for each student, this file will also include the enrollment status of the student (so that you can filter out students who have unenrolled, if needed), whether the level is hidden from the student in D2L, and if the level achieved was automatically generated or a manual override.

## Return Data for Mastery View

To create the ETL data flow in Domo Analytics: 

1. Start with Outcome Registry Owners
2. Alter Columns so that OwnerId is cast as an integer
3. Filter by the specific OrgUnitId
4. Inner join with Outcomes Demonstrations on RegistryId
5. Inner Join with Users on AssessedUserId/UserId
6. Full Outer Join with Outcomes Scale Level Definition on ExplicitlyEnteredScaleLevelId/ScaleLevelId
7. Inner join with Outcome Details on OutcomeId
8. Inner join with Enrollments and Withdrawals on UserId and OwnerId/OrgUnitId
9. Rank & Window - enter a new column name (such as LatestEnrollment) with a function using Rank on Enrollment Date in Descending order, partitioned by UserId
10. Add Formula with these formulas: 
    1. Type a name for a new column, such as ManualOverride, and add the formula **``CASE when `ExplicitlyEnteredScaleLevelId`=`AutomaticallyGeneratedScaleLevelId` then 'FALSE' when `Name` IS NULL then 'FALSE' else 'TRUE' End``** (this creates a new column where TRUE indicates the level was manually chosen in D2L, which is represented by an asterisk in Mastery View)
    2. choose the existing column Name and add the formula: **``IFNULL(`Name`, 'Not evaluated')``**
    3. choose the existing column IsPublished and add the formula: **``IFNULL(`IsPublished`, 'UNKNOWN')``**
    4. choose the existing column AssessedDate and add the formula: **``IFNULL(`AssessedDate`, '1970-01-01T00:00:01Z')``**
11. Rank & Window - enter a new column name (such as LatestAssessed) with a function using Row Number on IsPublished in Ascending and AssessedDate in Descending orders, partitioned by AssessedUserId, Description, and OwnerId
12. Filter Rows - add two filter rules where the columns created in steps 9 and 11 (LatestEnrollment and LatestAssessed) both equal 1
13. Select Columns - in addition to Username, FirstName, and LastName, you will probably want Description from OutcomeDetails for the text of the outcome (I rename this to Outcome), Name from Outcomes Scale Level Definition for the labels of the levels (I rename this to MasteryLevel), IsPublished, ManualOverride which was created in step 10, and Action from Enrollments and Withdrawals for the student's enrollment status

![ETL data flow for Return All Data for Mastery View as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-mastery-view-status.png)

You can skip steps 8 & 9 and the LatestEnrollment rule in step 12 if you do not need enrollment status for each student. You can also skip the first formula in step 10 if you do not need information on manual overrides.

### Pivot the Data
Use [this Python script](https://github.com/jenniferwagner18/brightspace-d2l-scripts/blob/main/d2l-outcomes-pivot.py) if you want to pivot the data so that the students are in rows, the outcomes are in columns, and the levels are the values. This will mostly re-create the Mastery View table in D2L. Or watch this [Outcomes data video](https://mediaspace.msu.edu/media/D2L+Outcomes+Data+PivotTable+to+re-create+Mastery+View/1_2f4z3wn3) to see how to create a pivot table with text-only data in Excel.
