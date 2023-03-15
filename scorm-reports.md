# SCORM Reports with Written Responses

If you use SCORM objects in D2L, such as Articulate Storyline or iSpring, student responses are visible in SCORM Reports in the Table of Contents. However, you must click each student's name and the Interactions tab in order to see their written responses. You can also only export one CSV file per student instead of all students together with all responses. To generate a CSV file of all students and responses for a SCORM object, join the SCORM data sets below. The Response column in the ScormInteractionAttempts data set includes the text that students entered during the interaction.

## Return All SCORM Responses

To create the ETL data flow in Domo Analytics: 

1. Start with ScormObjects and filter by OrgUnitId
3. Inner join with ScormVisits on ScormObjectId
4. Inner join with ScormInteractionAttempts on VisitId
5. Inner join with ScormInteractions on InteractionId and ActivityId
6. Inner join with Users on UserId
7. Select columns that are needed, such as Title in ScormObjects, Score and TimeSpent in ScormVisits, CorrectResponses and InternalId in ScormInteractions, and Response and TimeSpent in ScormInteractionAttempts.

![ETL data flow for Return All SCORM Responses as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-scorm-reports.png)

The InternalId column will return the identifier shown in the SCORM report in D2L, such as Scene1_Slide1_ShortAnswer_0_0. It may or may not include the text of the question, depending on which software created the SCORM object. Using SCORM 2004 when creating the object will usually include the question text.
