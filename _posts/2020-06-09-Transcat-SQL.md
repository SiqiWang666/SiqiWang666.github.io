---
title: "Intro to Transact-SQL"
date: 2020-06-09
read_time: true
categories:
  - post
tags:
  - SQL
---

# 🤨Get confused about the terms?
- 📌Data. Obviously, data is data, the known facts out there, like text, images, videos.

- 📌Database organizes collections of logically related data.

- 📌DBMS, database management system, is a piece of software who helps you manage your database.

<div class="notice">
  <p>Some DBMS are open source, like MySQL, PostgreSQL.</p>
  <p>Some DBMS cost you 💸, like MS SQL Server.</p>
</div>

## MS SQL Server (free version)
MS SQL Server is a collection of *objects* that hold and manipulate data.

> What is objects? Tables, views, stored procedures, and constraints.

a SQL Server = Database Engine Instance

# SQL Server Service
Replicated Database
SQL Server Agent:
Reporting Service
Notification Service
Distributed Transaction Coordinator


# SQL Queries
## Join
```
join --- inner join
      |
      |__ outer join ---- left join
                      |-- right join
                      |-- full join
```


● Using Aliases for Table Names
● Combining Data from Multiple Tables - Joins
● Combining Multiple Result Sets
● Recommended Practices

● Listing the TOP n Values
● Using Aggregate Functions
● GROUP BY Fundamentals
● Having Clause
● Recommended Practices
● SQL Server Day 2 Assignment

count(1) is faster than count(*)

## VIEW
A quick recap: views are objects in SQL server. A view is a virtual table whose contents are defined by a query. View can be defined based on single table or multiple tables (JOIN).

```SQL
CREATE VIEW <viewName>
AS
SELECT <column_name> FROM <table_name> -- base table
```

All the DML statements that perform on view are actually executed on the base tables.
{: .notice--danger}

### Why we use VIEW?

🙂Views make complex query easier. You can just perform queries on one view instead of multiple tables.

🙁 1. View don't accept parameters (while you can use sql injection, but not elegant). 2. Sometimes updating a view with multiple base tables doesn't give you a desired result.


## Stored Procedure
View can't accept parameters.BUT stored procedure does.

- In mode: passing input values to the procedure.
- Out mode: set results through out parameters. Or, *RETURN* the result, but only single *INT* variable works.

```SQL
CREATE PROC addSum
@a INT, @b INT, @res INT OUT
AS
BEGIN
  IF @a IS NULL OR @b IS NULL OR
    @res = 0
  ELSE 
    @res = @a + @b
END
```

If you are using *RETURN* in the out mode, you can only return single *INT* value.
{: .notice--danger}

> When you use the stored procedure with *OUT* mode, be aware of using *EXEC* in the procedure call!

IMPORTANT! The execution plan of stored procedure will be cached, so the stored procedure will have a better performance.

## Function
Function will be commonly used in *SELECT, UPDATE* statement. Suppose you have a column of base salary and a column of bonus salary, you can define a function to return the total salary of a person.

## Triggers
