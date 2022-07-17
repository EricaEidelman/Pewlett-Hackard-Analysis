# Employee Database with SQL

## Project Overview
The purpose of this project is to do a human resources analysis for Pewlett-Hackard as a large number of their employees will be retiring soon. Management needs to understand how many of the employees are retiring as well as well as their positions, in order to best prepare for a loss of staff and acclimating of new employees.

## Resources
Data Source: departments.csv, dept_emp.csv, dept_manager.csv, employees.csv, salaries.csv, titles.csv

Software: pgAdmin 4, PostgresSQL 11.4

## Results
- One of the main takeaways from the analysis is that 72,458 employees are due to retire soon. The list in the retirement_titles csv file has 133,774 rows, but that is due to the fact that some employees have switched positions in their time with the company.
- From the above, another point to consider is that employees do not stay in the same position all the time. This is important to note because there will be staff changes even without employees retiring.
- The majority of retirees are at a senior level, which means management should pay attention to those employees which have reached a senior position as they are most likely to retire and need replacement. (See image below for full numbers.)
- Finally, the majority of retirees hold staff and engineer positions, so management should likewise focus on those roles when considering new hiring strategies. (See image below for full numbers.)

![This is an image](https://github.com/EricaEidelman/Pewlett-Hackard-Analysis/blob/main/Retiring_Titles.png)

## Summary
From the analysis, management can see that they will need to fill 72,458 roles as employees reach retirement age and depart the company. On the other hand, only 1,549 employees are currently qualified to be mentors in the mentorship program, so at first glance it would appear that not enough mentors are available to coach the next generation. An additional query which might help to understand the situation is one which shows a breakdown of titles among the mentorship eligible employees, in order to see the proportion of titles of the mentors to the retirees and needed hires. The query can be obtained with the following code (results follow in image):

```
SELECT title, count(title)
FROM mentorship_eligibility
GROUP BY title
ORDER BY count(title) DESC;
```

![This is an image](https://github.com/EricaEidelman/Pewlett-Hackard-Analysis/blob/main/Mentorship_Eligibility_Titles.png)

The largest number of potential mentors are senior staff and engineers followed by regular staff and engineers, which is equivalent to the breakdown of titles among the retirees. However, if every single one of the retirees is replaced by a new hire in the equivalent position, senior staff and engineers will have to mentor over 60 people each, with regular staff and engineers having to mentor about 25 and 30 people each, respectively. This new information shows that the mentorship program will likely be unsustainable with only the non-retired employees participating and would need participation from some of the retirees.

Another useful query will be one that shows the average tenure of an employee in each position. Using the retirement_titles data and the code below, that information can be found.

```
SELECT title,
	avg(DATE_PART('years', AGE(to_date, from_date)) +
	DATE_PART('months', AGE(to_date, from_date))/12) as avg_years_tenure
FROM retirement_titles
WHERE DATE_PART('years', AGE(to_date, from_date)) +
	DATE_PART('months', AGE(to_date, from_date))/12 < 7000
GROUP BY title
ORDER BY avg_years_tenure DESC;
```

The image below shows the result of the query.

![This is an image](https://github.com/EricaEidelman/Pewlett-Hackard-Analysis/blob/main/Retiring_Title_Avg_Tenure.png)

The results of the query show that even though senior staff and engineers are the group most likely to retire, they have one of the lowest average tenures. Management must then either pay special attention to people promoted to the level of senior staff or senior engineer, as they are likely to retire soon, or to promote at an earlier age, which would create more years of tenure before the employee departs the company and needs to be replaced.
