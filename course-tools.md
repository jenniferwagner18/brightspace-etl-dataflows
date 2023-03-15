# Course Tools

If you'd like a CSV report of which courses are using each D2L tool (such as Content, Assignments, Quizzes, Discussion) and the counts of each use of the tool in the course, use the data flow below as a guide. You can add more tools to your own data flow, such as Rubrics, Surveys, Announcements, Intelligent Agents, etc. This will give you an idea of which tools are being used the most and for which tools you may want to offer instructors more training on.

## Return Counts of Tool Usage in Courses

To create the ETL data flow in Domo Analytics:

1. Start with Organizational Units and filter on Type equals Course Offering and IsActive equals True
2. On another branch, start with Content Objects and Group By 1) OrgUnitId, 2) enter a name for the new column, and 3) select ContentObjectId and Count distinct values
3. Left join the active courses and count of content objects on OrgUnitId
4. Continue adding new branches for each tool and left join on OrgUnitId. For example:
    1. Assignment Summary and Group By 1) OrgunitId, 2) enter a name for the new column, and 3) select DropboxId and Count distinct values
    2. Discussion Topics and Group by 1)  OrgunitId, 2) enter a name for the new column, and 3) select TopicId and Count distinct values
    3. Quiz Objects and Group by 1)  OrgunitId, 2) enter a name for the new column, and 3) select QuizId and Count distinct values
    4. Grade Objects and filter on GradeObjectTypeId is less than 7 and IsDeleted equals False, then Group By 1) OrgunitId, 2) enter a name for the new column, and 3) select GradeObjectId and Count distinct values
5. Select desired columns


![ETL data flow for Return Counts of Tool Usage as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-course-tool-counts.png)
