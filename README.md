# SQLbaseball

----------------------
I wanted to practice my SQL queries so I found this SQLite baseball database. Here I tried to answer these questions.

1. From 2006 to 2016, which 5 teams had the highest payrolls?

2. Is there any general trend in payrolls over this time period?

3. How do the top 5 teams compare to other teams?

4. Which players had the highest number of homeruns?

5. How win percentage affects attendance?

6. Which teams have the most wins in games and in the World Series?

The overall goal was to practice my SQL queries while answering some interesting questions with this database. I also used plotnine which is a package that allows you to utilize ggplot to make visualizations.

------------------------------

Some Queries used:

Top 5 teams by Payroll. Had to sum(salary) and group by teamID, and yearID:
```SQL
WITH payrolls AS
(SELECT SUM(salary) AS Payroll, teamID, yearID 
FROM Salaries
WHERE yearID BETWEEN 2006 AND 2016
GROUP BY teamID, yearID
ORDER BY SUM(salary) DESC
LIMIT 20)

SELECT DISTINCT(teamID) FROM payrolls
LIMIT 5
```

Used case to set the other teams besides the top 5 as others

```SQL
WITH payrolls AS
(SELECT SUM(salary) AS Payroll, teamID, yearID 
FROM Salaries
WHERE yearID BETWEEN 2006 AND 2016
GROUP BY teamID, yearID
ORDER BY SUM(salary) DESC)

SELECT Payroll,yearID, AVG(Payroll),
    CASE WHEN teamID NOT IN (SELECT DISTINCT(teamID) FROM payrolls LIMIT 5) THEN 'Others'
    ELSE teamID
    END team
FROM payrolls
GROUP BY Team, yearID
```

To get avg salary by year to see trend over time.
```SQL
SELECT yearID, AVG(salary) AS Payroll
FROM Salaries
WHERE yearID BETWEEN 2006 AND 2016
GROUP BY yearID

Joined tables Salaries and People to get names of players with highest salaries.

SELECT l.*, r.nameGiven,r.nameLast FROM Salaries l
LEFT JOIN People r
ON l.playerID = r.playerID
ORDER BY salary DESC
LIMIT 20
```
Joined two subquery tables together to get salaries of the top 3 paid players over time.

SELECT l.*, r.salary FROM
    (SELECT playerID, HR, yearID FROM Batting
    WHERE playerID IN ('bondsba01' ,'rodrial01', 'pujolal01')) as l
LEFT JOIN
    (SELECT * FROM Salaries) as r
ON l.playerID = r.playerID AND l.yearID = r.yearID


------------------------------

For a better look at notebook here is the link for nbviewer.
[!nbviewer](https://nbviewer.org/github/EdgarFonseca94/SQLbaseball/blob/main/SQLbaseball.ipynb)
