# Graded Rubrics

Graded rubrics in Brightspace by D2L can only be downloaded individually by going to each student in Class Progress and printing as a PDF. If you'd like a spreadsheet of every student's scores for each criterion as well as any level descriptions or feedback given to students, you will need to join together several data sets.

## Return All Graded Rubric Criteria and Scores

To create the ETL Workflow in Domo Analytics: 

1. Start with Rubric Assessment, filter by the specific RubricId (hover over rubric name in the course and get the ID from the URL)
2. Inner join with Rubric Assessment Criteria on RubricId and UserId
3. Inner join with Rubric Criteria Levels on CriterionId
4. Inner join with Rubric Object Criteria on CriterionId
5. Inner join with Users on UserId (if you need students' names)
6. Filter rows so that Rubric Assessment Criteria Score = Rubric Assessment Criteria Value (1. Score , 2. equals, 3. value from a column, 4. Value) 
7. Select Columns that are needed (for example: Rubric Assessment Score for total score, Rubric Assessment Criteria Score for each criterion score, Rubric Object Criteria Name, Rubric Object Criteria SortOrder, Rubric Criteria Levels Description, and Rubric Assessment Criteria Feedback - if any was given to student)
8. Remove Duplicates
  
![ETL Workflow for Return All Graded Rubric Criteria and Scores as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-graded-rubrics.png)
