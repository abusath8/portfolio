---
title: "SQL for Data Wrangling"
---

# SQL Queries Every Data Science Student Should Know

## Introduction
SQL stands for Structured Query Language, and is a way to define, manage, and manipulate databases. In this tutorial, we will walk through basic SQL syntax and patterns that cover many data analysis tasks: filtering rows with `WHERE`, summarizing with `GROUP BY`, and combining tables with `JOIN`. Knowing these three patterns opens the door to answering many questions directly in the database.

## 1. Basic SQL Syntax
Before we write full queries, it helps to understand the basic pieces that show up in almost every SQL statement. At a minimum, you usually need two parts:
```sql
SELECT columns_you_want
FROM table_name;
```
- `SELECT` tells the database which columns to return
- `FROM` tells it which table to read from

## 2. Filtering rows with WHERE
The `WHERE` clause lets you filter a table down to only the rows you care about. You can still use `SELECT` to choose columns and `FROM` to choose the table, but `WHERE` adds a logical condition on top.

Example:
```sql
SELECT name, gpa
FROM students
WHERE major = 'STAT' AND gpa >= 3.5;
```
In this query:
- `SELECT name, gpa` chooses the columns to return
- `FROM students` says we are reading from the students table
- `WHERE major = 'STAT' and gpa >= 3.5` keeps only rows where the major column is exactly `STAT` and the GPA is at least 3.5

You can combine conditions with logical operators like `AND`, `OR`, and `NOT`:
```sql
SELECT *
FROM students
WHERE major = 'STAT' OR major = 'CS';
```

## 3. Summarizing data with GROUP BY
So far, our queries have worked at the row level. Often we want summaries by groups: "average GPA by major" or "total sales per customer". The `GROUP BY` clause lets us aggregate many rows into one row per group. 

The basic pattern is:
```sql
SELECT group_column,
    AGG_FUNCTION(other_column) AS summary_name
FROM table_name
GROUP BY group_column;
```
Suppose we had a table with the following columns:
- id_number
- name
- major
- gpa

To compute the average GPA by major:
```sql
SELECT major,
    AVG(gpa) AS avg_gpa
FROM students
GROUP BY major;
```
This query produces one row per `major`. Within each major, the AVG function is applied to all rows in that group and returns the result as `avg_gpa`. Other possible functions to be used here include `COUNT`, `SUM`, `MIN`, `MAX`.

## 4. Combining tables with JOIN
Databases rarely keep everything in one big table. Instead, related information is split across tables and linked by key columns. The `JOIN` clause lets you combine those tables so you can analyze them together.

The most common pattern is an inner join:
```sql
SELECT *
FROM table1 AS t1
JOIN table2 AS t2
    ON t1.key = t2.key;
```

Imagine we have two tables.

Table 1 (students) variables:
- student_id
- name
- major

Table 2 (enrollments) variables:
- student_id
- course
- grade

To see each student's name alongside their grade for a given course, we can join on the student ID:
```sql
SELECT s.name,
       e.course,
       e.grade
FROM students AS s
JOIN enrollments AS e
    ON s.id = e.student_id
WHERE e.course = 'STAT386';
```
HERE `JOIN ... ON s.id = e.student_id` tells the database how the two tables are related: match each row in `students` to `enrollments` with the same id. After the join, the `WHERE` clause filters down to just the course we care about. This pattern can be chained so there are multiple tables joined in a single query.

## Conclusion and Next Steps
In this tutorial, you saw how a few SQL patterns can cover a lot of everyday analysis work. You learned the basic shape of a query with `SELECT`, `FROM`, and `WHERE`, how to filter rows and columns using conditions, how to summarize by different groups using `GROUP BY` and aggregate functions like `AVG`, and how to combine related tables using `JOIN`. Together, these patterns let you pull exactly the data you need from a database instead of exporting huge CSV files and cleaning everything later.

Next, try making these ideas concrete in your own environment, Start by writing a simple query that filters a table with a `WHERE` clause, then turn it into a grouped summary with `GROUP BY`. After that, practice joining two tables into a grouped summary with `GROUP BY`. After that, practice joining two tables that share a key to answer a question you couldn't answer from either table alone.