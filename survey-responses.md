# Individual Attempts Survey Responses

The individual attempts survey report that can be generated in Brightspace by D2L does not include attempt information. Multiple attempts are aggregated in the report. If using surveys for pre-tests and post-tests, particularly with Likert questions on a 1 to 5 scale, the responses appear in numerical order instead of attempt order. This makes it impossible to know which response is attached to which attempt. Fortunately, the Brightspace data set of Survey Attempts includes the Attempt Number.

## Return All Survey Responses

To create the ETL Workflow in Domo Analytics: 

1. Start with Survey Attempts, filter by the specific SurveyId (hover over survey name in the course and get the ID from the URL)
2. Inner join with Users on UserId to get students' names
3. Inner join with Survey User Answer Responses on AttemptId to get students' responses
4. Inner join with Survey Question Answers on AnswerId to get text of options/statements within the Likert question
5. Inner join with Question Library on QuestionId and QuestionVersionId to get text of question (not needed if using one Likert question)
6. Filter rows so that IsDeleted equals False
7. Select Columns that are needed
8. Remove Duplicates
  
![ETL Workflow for Return All Survey Responses as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-survey.png)
