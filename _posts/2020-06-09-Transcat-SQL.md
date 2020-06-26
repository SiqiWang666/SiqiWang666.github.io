---
title: "Intro to Transact-SQL"
date: 2020-06-09
read_time: true
categories:
  - post
tags:
  - SQL server
---

# 🤨 Get confused about the terms?

- 📌Data. Obviously, data is data, the known facts out there, like text, images, videos.

- 📌Database organizes collections of logically related data.

- 📌DBMS, database management system, is a piece of software who helps you manage your database.

<div class="notice">
  <p>Some DBMS are open source, like MySQL, PostgreSQL.</p>
  <p>Some DBMS cost you 💸, like MS SQL Server.</p>
</div>

## MS SQL Server (free version)

MS SQL Server is a collection of _objects_ that hold and manipulate data.

> What is objects? Tables, views, stored procedures, and constraints.

a SQL Server = a Database Engine Instance. A database engine manages several **system databases** and one or more **user databases**.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/SQL-Server/database-structure.png" alt="">

A database file is nothing more than an operating system file.

- Primary data file (`mdf`). Every database has one primary data file that stores the rest of files of the database, in addition to storing data.
- Secondary data file (`ndf`). Every database has zero or more secondary data files.
- Log data file (`ldf`). Every database has at least one log files that contains necessary information to recover from transactions.

FYI: I use MSSQL container and Azure Data Studio. The data files are under `/var/opt/mssql/data` folder.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/SQL-Server/database-file.png" alt="">

# Transact-SQL

What is a batch directive?
A batch directive instructs SQL server to parse and execute the statements in a batch. There are two ways to handle over batch directive to SQL server, `GO` and `EXEC`.

## Variables

## Result Set

Result set is a set of data, could be empty or not, returned by a select statement, or a stored procedure, that is saved in RAM or displayed on the screen.

## VIEW

A quick recap: views are objects in SQL server. A view is a virtual table based on the result set of SQL statement(SELECT). View can be defined based on single table or multiple tables (JOIN, UNION).

```SQL
CREATE VIEW <viewName>
AS
SELECT <column_name> FROM <table_name> -- base table
```

All the DML statements that perform on view are actually executed on the base tables.
{: .notice--danger}

### Why we use VIEW?

🙂Views make complex and reusable query easier. You can just perform queries on one view instead of multiple tables.

🙁 1. View don't accept parameters (while you can use sql injection, but not elegant). 2. Sometimes updating a data through view that are based on multiple tables doesn't give you a desired result.

## Stored Procedure

Stored procedure is a commonly used object in SQL server. It is a collection of DML, DDL statements that are executed together. View can't accept parameters.BUT stored procedure CAN. Stored procedure have a better performance since the most optimal execution plan will be cached after the first execution.

- In mode: passing input parameters to the procedure `@id INT = 3`.
- Out mode: set results through out parameters. Or, _RETURN_ the result, but only a single _INT_ variable works.

```SQL
CREATE PROC addSum
@a INT = 1, @b INT, @res INT OUT
AS
BEGIN
  IF @a IS NULL OR @b IS NULL OR
    @res = 0
  ELSE
    @res = @a + @b
END
```

How to call a stored procedure? A explicitly `EXEC` is needed.

```SQL
DECLARE @res INT
EXEC addSum 1, 3, @res OUT
```

## Function

Fuction consists of built-in function and user-defined function. Function will be commonly used in _SELECT, UPDATE_ statement. The return value can either be a scalar (single) value or a table. Suppose you have a column of base salary and a column of bonus salary, you can define a function to return the total salary of a person.

## Triggers

Triggers are a special type of stored procedure which will be implicitly called after SQL statements.

- AFTER: called after execution of DML statement
- INSTEAD OF: called before execution of DML statement
- for: ?

Once we create a trigger on a table, the inserted record will be insterted into `INSERTED` and similarly the deleted records are at `DELETED`. These tables are conceptual tables.

What are the scenarios to use Triggers? Trigger in sql server is used for business logics to be executed. We can prevent creation of duplicate records. To create logs and so on.

## Transaction

Transaction is a collection of logical related statements. The outcome of a transaction is either commit or roll back. Transaction must maintain ACID properties, atomic, consistent, isolated (locking facility) and durable (log facility).

## Query

### Join

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

count(1) is faster than count(\*)
