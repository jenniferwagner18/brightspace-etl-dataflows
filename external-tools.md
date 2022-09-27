# External Learning Tools

This data flow will generate the total number of LTI links for External Learning Tools per semester so that you can determine how often tools are being used and in which courses. The course codes and tools will differ depending on what is available at your institution so you will need to update the formulas accordingly. Look at the LTI Links data set and the URL column to see all of the tools being used. 

## Return total number of LTI links for External Learning Tools

The total number of links per tool per semester will be repeated across the rows. To create the ETL data flow in Domo Analytics:

1. Start with Organizational units
2. Filter that includes rows that meet all rules: Type equals Course Offering and IsActive equals True
3. Filter that includes rows that meet any rule: Code starts with FS22, Code starts with US22, etc. (this will depend on how semesters are included in your institution's course codes)
4. Add LTI Links and filter IsVisible equals True
5. Inner join on OrgUnitId
6. Add formulas
    1. Name output column Tool and use formula (find a word that is in the URL for each tool to identify it; add as many as you need):
    
		**``CASE 
		when `Url` like '%playposit%' then 'Playposit'
		when `Url` like '%padlet%' then 'Padlet'
		when `Url` like '%piazza%' then 'Piazza'
		when `Url` like '%h5p%' then 'H5P'
		else 'Other Tools'
		End``**
    
    2. Name output column Semester and use formula (if your Code column starts with semester designations; add as many as you need):

		**``CASE 
		when `Code` like 'SS22%' then 'SS22'
		when `Code` like 'FS22%' then 'FS22'
		else 'Other Semester'
		End``**
    
7. Group By 1. Semester and Tool, 2. name the new column TotalLinks, 3. Tool, 4. Count
8. Inner join on Semester and Tool
9. Select desired columns

![ETL data flow for Return total number of LTI Links as described in ordered list](https://jenniferlynnwagner.com/img/etl/domo-etl-lti-tools.png)

