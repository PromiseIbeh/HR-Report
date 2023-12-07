## HR Absenteeism Report
### Table of Content
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Result](#result)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [Reference](#reference)

### Project Overview
The aim of this project is to analyse the covid dataset to get information on the impact 0f covid in Nigeria. We want to know the death rate, infection rate, chances of getting infected with covid in Nigeria, percentage of people who has taken the vacine.

### Data sources
The covid dataset used for this project is an open dataset, made available on kaggle for for public use.

### Tools
- SQL server - used to analyse data
- PowerBi - used to create report

### Data Cleaning
1. remove irrelevant columns
2. change data types

### Exploratory Data Analysis
- what is the rate of death in 2020
- what is the infection rate
- what are the chances you'll be infected with covid
- what

### Data Analysis
```sql
--join all the table to get one usable table

SELECT *
FROM Absenteeism A
LEFT JOIN compensation C
ON A.ID = C.ID
LEFT JOIN Reasons R
ON A.Reason_for_absence = R.Number

--Finding the healthiest employee with low absenteeism for bonus

SELECT * 
FROM Absenteeism
WHERE Social_drinker =0 AND Social_smoker =0 and Body_mass_index <25
AND Absenteeism_time_in_hours < (SELECT AVG(Absenteeism_time_in_hours) FROM absenteeism)

-- compensation rate for non smokers/ Budget $983,221. 
-- total hours worked by non-smokers employees=8hrs * 52weeks * 686 employetes =1,426,880.
--Percentage of increase = 983,221/1,426,880 = 0.68

--number of non-smokers employees
SELECT COUNT(*)
FROM Absenteeism
WHERE Social_smoker =0		


-- Optimizing the joined table
SELECT 
A.ID,
R.Reason,
A.Month_of_absence,
DATENAME(MONTH, DATEADD(MONTH,[Month_of_absence] , -1)) AS Month_name,
CASE 
	WHEN month_of_absence in (12,1,2) THEN 'Winter'
	WHEN month_of_absence in (3,4,5) THEN 'Summer'
	WHEN month_of_absence in (6,7,8) THEN 'Spring'
	WHEN month_of_absence in (9,10,11) THEN 'Fall'
	ELSE 'Unknown'
	END AS 'Season',
A.Day_of_the_week,
DATENAME(WEEKDAY,[Day_of_the_week]) Week_day,
A.transportation_expense,
A.service_time,
A.age,
A.Work_load_Average_day,
A.Hit_target,
A.disciplinary_failure,
A.Son,
A.Social_drinker,
A.Social_smoker,
A.Pet,
A.Weight,
A.Absenteeism_time_in_hours,
A.Body_mass_index,
CASE
	WHEN a.Body_mass_index < 18 THEN 'Under weight'
	WHEN a.Body_mass_index BETWEEN 19 AND 25 THEN 'Healthy weight'
	WHEN a.Body_mass_index BETWEEN 25 AND 30 THEN 'Over weight'
	WHEN a.Body_mass_index >=31 THEN 'Obese'
	ELSE 'Unknown'
	END AS 'BMI'
FROM Absenteeism A
LEFT JOIN compensation C
ON A.ID = C.ID
LEFT JOIN Reasons R
ON A.Reason_for_absence = R.Number
ORDER BY 3,5

--Average hour of absenteeism
SELECT AVG(absenteeism_time_in_hours)
FROM Absenteeism

--Percentage of social drinkers
SELECT [Social_drinker], 
COUNT([Social_drinker])*100/SUM(COUNT(*)) over() Percentage_of_drinkers
FROM Absenteeism
GROUP BY [Social_drinker]

-- percentage of social smokers
SELECT [Social_smoker],
COUNT(*)*100/(SELECT COUNT([Social_smoker]) FROM Absenteeism) Percentage_of_smokers
FROM Absenteeism
GROUP BY [Social_smoker]

--percentage of disciplinary failures
SELECT [Disciplinary_failure],
COUNT(*)*100/(SELECT COUNT([Disciplinary_failure]) FROM Absenteeism)
FROM Absenteeism
GROUP BY [Disciplinary_failure];
```

### Result
the summary of the analysis are as follows;
this is where you add the summary of your analysis, what are your findings

### Recommendations
recommendations based on your analysis based on your findings

### limitations

mention the limitations, the buts, what might affect your conclusions

### Reference

where you got any form of help, be it a book, a site or a person


how to create a table here
|heading|heading|
|-------|-------|
|sql| sql|


**Bold**
*italic*







