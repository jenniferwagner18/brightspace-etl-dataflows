# Mastery View of Learning Outcomes

If you are using the Standards tool with learning outcomes in your D2L courses, the Mastery View in the grade book does not have an export option. To generate a CSV file of the data in this view, use the following data flow with the Outcomes data sets.

## Return Data for Mastery View

To create the ETL data flow in Domo Analytics: 

1. Start with OutCome Registry Owners, filter by the specific OrgUnitId
2. Inner join with Outcomes Demonstrations on RegistryId
3. Inner Join with Users on AssessedUserId/UserId
4. Right Join with Outcomes Scale Level Definition on ExplicitlyEnteredScaleLevelId/ScaleLevelId
5. Inner join with Outcome Details on OutcomeId
6. Add Formula with a name for the new column, such as ManualOveride, and the formula **``CASE when `ExplicitlyEnteredScaleLevelId`=`AutomaticallyGeneratedScaleLevelId` then 'FALSE' else 'TRUE' End``** (this creates a new column where TRUE indicates the level was manually chosen in D2L, which is represented by an asterisk in Mastery View)
7. Select desired columns (in addition to Username, FirstName, and LastName, you will probably want Description from OutcomeDetails for the text of the outcome, Name from Outcomes Scale Level Definition for the labels of the levels, IsPublished, and ManualOveride which was created in step 6)
8. Filter Rows on **IsPublished not null** (TRUE indicates that the level is not hidden from students in Mastery View - represented by the eye icon)
  
![ETL data flow for Return All Data for Mastery View as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-outcomes.png)

*Note that Not Evaluated in the Mastery View is missing from the data sets since there is no data to be saved.*

### Pivot the Data
Use [this Python script](https://github.com/jenniferwagner18/brightspace-d2l-scripts/blob/main/d2l-outcomes-pivot.py) if you want to pivot the data so that the students are in rows, the outcomes are in columns, and the levels are the values. This will mostly re-create the Mastery View table in D2L.
