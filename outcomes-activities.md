# Learning Outcomes for Individual Activities

If you are using the Standards tool with Learning Outcomes in your D2L courses, the Mastery View of overall levels in the grade book does not show the levels achieved for each individual activity that students completed in the main table. You must click each overall level per student in order to see their achieved levels on each discussion, assignment, or quiz. To generate a CSV file of these levels that factor into the overall level, use the following data flow (which can also be added to the [Mastery View data flow](https://github.com/jenniferwagner18/brightspace-etl-dataflows/blob/main/mastery-view.md) as a second output). In addition to the outcomes and levels achieved for the last 5 assessed activities for each student, this file will also include if the level achieved was automatically generated or a manual override.

## Return Data for Outcomes Aligned to Discussions, Assignments, and Quizzes

To create the ETL data flow in Domo Analytics: 

1. Start with Outcome Registry Owners and filter by the specific OrgUnitId
2. Inner join with Outcomes Demonstrations on RegistryId
3. Full outer Join with Outcomes Scale Level Definition on ExplicitlyEnteredScaleLevelId/ScaleLevelId
4. Inner join with Outcome Details on OutcomeId
5. Inner Join with Users on AssessedUserId/UserId
6. Filter Rows - Include rows that meet **all** of the following rules: AlignedObjectType < 4, AssessedDate is NOT NULL, and IsPublished IS NULL
7. Alter Columns so that AlignedObjectId is cast as an integer
8. Left outer join with Discussion Topics on AlignedObjectId/TopicId
9. Left outer join with Assignment Summary on AlignedObjectId/DropboxId
10. Inner join Quiz Objects and Quiz Attempts on QuizId, then left outer join on AlignedObjectId/AttemptId and AssessedUserId/UserId
11. Value Mapper: 1) AlignedObjectType, 2) write to new column ActivityType as Text, 3) Write a default value of Unknown, 4) enter the numbers 1, 2, and 3 in three boxes, and 5) enter Discussion, Assignment, and Quiz in three boxes
12. Add Formula
    1. enter ActivityName as a new column name and the formula **``CASE WHEN `AlignedObjectType` = 1 THEN `Discussion Topics 7.Name` WHEN `AlignedObjectType` = 2 THEN `Assignment Summary 7.Name` WHEN `AlignedObjectType` = 3 THEN `QuizName` ELSE 'Other' END``** [make sure you have renamed columns from the discussion and assignment data sets since they both include a column named Name]
    2. enter ManualOverride as a new column name and the formula **``CASE when `ExplicitlyEnteredScaleLevelId`=`AutomaticallyGeneratedScaleLevelId` then 'FALSE' when `Name` IS NULL then 'FALSE' else 'TRUE' End``**
13. Filter Rows so that ActivityName is NOT NULL
14. Rank & Window: 1) Name a new column such as LastUpdate using the function Row Number, 2) AssessedDate, 3) Descending, 4) partition of AssessedUserId, Description, and OwnerId
15. Filter Rows so that LastUpdate < 6
16. Select desired columns

![ETL data flow for Return All Data for Outcomes with Individual Activities as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-outcomes-tools.png)

If you need to filter out drafts or deleted activities, there are IsGraded columns in the Discussion Topic User Scores, Assignment Submissions, and Quiz Attempts data sets. Set the filter rule to equals True. (The AssessedDate column in the Outcomes Demonstrations data set logs both drafts and published activities.) There are also IsDeleted columns in the Discussion Topics, Discussion Posts, Assignment Summary, Assignment Submission Details, and Quiz Attempts data sets. Set the filter rule to equals False.
