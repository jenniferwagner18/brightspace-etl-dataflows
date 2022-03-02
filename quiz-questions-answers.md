# Quiz Questions and Possible Answer Choices

Although instructors can export quiz questions and answer choices under Statistics for each quiz in D2L, this requires that one attempt has already been made on the quiz. Additionally, each quiz must be exported separately. The following workflow can return data on all quizzes in a course or multiple courses when filtering by OrgUnitId(s). Filtering by individual QuizId is also possible, which can be found in the URL when hovering over the quiz name in D2L.

It is not possible to download all of the questions in a course's question library. Questions must be added to a quiz in order to be connected to an OrgUnitId.

Note: This workflow does not return students' responses to the questions. (See [Quiz Questions and Student Responses](https://github.com/jenniferwagner18/brightspace-etl-workflows/blob/main/quiz-questions-responses.md)).

## Return All Quiz Questions and Answer Choices by Course or Quiz

To create the ETL Workflow in Domo Analytics: 

1. Start with Quiz Objects and filter data by OrgUnitId or QuizId
2. If there are sections/pools: Outer left join with QuizSurveySections on Quiz Id/CollectionId
3. Inner join with Question Relationships on QuizId/CollectionId
4. Inner join with Question Library on QuestionId and QuestionVersionId
5. Inner join with Quiz Question Answers on QuestionId and QuestionVersionId
6. If there are short answer or fill in the blank questions: Outer left join with Quiz Question Answer Options on AnswerId, QuestionId, and QuestionVersionId
7. Select Columns that are needed
8. Remove Duplicates
  
![ETL Workflow for Return All Quiz Questions and Answers as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-quiz-answer-choices.png)
