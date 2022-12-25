# SQLConcepts

## Aggregates

SQL aggregation is the task of collecting a set of values to return a single value.

Following are some common aggregate functions along with group by:

- AVG: This calculates the average of all values in a group.
- MIN: It returns the lowest value in a group.
- MAX: MAX aggregate function in SQL returns the largest value in a group
- COUNT: It is used to count the number of rows in a set. the COUNT function includes rows with NULL values.
- SUM: This is used to calculate the sum of all non-NULL values in a group

Here is an example of aggregate function with GROUP BY

SELECT City_Name 
COUNT (Voter_ID) AS Voter_Count 
FROM Voter_List 
GROUP BY City_Name 
ORDER BY City_Name;

Here is an example of aggregate function without GROUP BY

SELECT COUNT (*) 
FROM Voter_List 
WHERE City_Name = 'X';


## Windowing

PostgreSQL's documentation does an excellent job of introducing the concept of Window Functions:

A window function performs a calculation across a set of table rows that are somehow related to the current row. This is comparable to the type of calculation that can be done with an aggregate function. But unlike regular aggregate functions, use of a window function does not cause rows to become grouped into a single output row — the rows retain their separate identities. Behind the scenes, the window function is able to access more than just the current row of the query result.

Syntax of Windowing Function

The first part of the below aggregation, AVG("MWh"), looks a lot like any other AVG aggregation. Adding OVER designates it as a window function. You could read the above aggregation as "take the average of milliWattPerHour over the entire result set, in order by Date."

If you'd like to narrow the window from the entire dataset to individual groups within the dataset, you can use PARTITION BY to do so:

SELECT "Plant", "Date",
    AVG("MWh") OVER (
        PARTITION BY "Plant"
        ORDER BY "Date" ASC)
        AS "MWh day Moving Average"
FROM "Generation History"
ORDER BY 1, 2


## Horizontal Partitioning on SQL Server tables

Horizontal partitioning divides a table into multiple tables that contain the same number of columns, but fewer rows. For example, if a table contains a large number of rows that represent monthly reports it could be partitioned horizontally into tables by years, with each table representing all monthly reports for a specific year. This way queries requiring data for a specific year will only reference the appropriate table. Tables should be partitioned in a way that queries reference as few tables as possible.

## DuckDB vs SQLite

DuckDB contains a columnar-vectorized query execution engine, where queries are still interpreted, but a large batch of values (a “vector”) are processed in one operation. This greatly reduces overhead present in traditional systems such as PostgreSQL, MySQL or SQLite which process each row sequentially.
