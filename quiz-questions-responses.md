# Quiz Questions and Student Responses

Although instructors can export quiz questions and student responses under Statistics for each quiz in D2L, each quiz must be exported separately. The following workflow can return data on all quizzes in a course or multiple courses when filtering by OrgUnitId(s). Filtering by individual QuizId is also possible, which can be found in the URL when hovering over the quiz name in D2L.

Note: This workflow does not return the possible answer choices to the questions. The responses to Written Response type questions will be truncated to 1,000 characters by default. If you need a longer character limit, contact D2L support.

## Return All Quiz Questions and Student Responses by Course or Quiz

To create the ETL Workflow in Domo Analytics: 

1. Start with Quiz Attempts and filter data by OrgUnitId or QuizId
2. Inner join with Quiz User Answers on AttemptId
3. Inner join with Quiz User Answer Responses on AttemptId, QuestionId and QuestionVersionId
4. Inner join with Question Library on QuestionId and QuestionVersionId
5. If you need students' names: Inner join with Users on UserId
6. Select Columns that are needed
  
![ETL Workflow for Return All Quiz Questions and Answers as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-quiz-responses.png)
