---
layout: post
title:  "SQLite: SQL error or missing database – foreign key violation"
date:   2026-05-11 19:10:36 +0100
categories: SQLite
---
When using SQLite “SQL error or missing database” is a very generic error that doesn’t help identify the root cause. The error usually occurs when inserting data where the SQL and/or data is incorrect. The root cause in this case was a foreign key definition that referred to a table that did not exist. I had created a foreign key relationship between two tables e.g A and B as shown below:

{% highlight SQL %}
CREATE TABLE [A] (
[Id] UNIQUEIDENTIFIER NOT NULL PRIMARY KEY,
[Name] TEXT NOT NULL
)

CREATE TABLE [B] (
[Id] UNIQUEIDENTIFIER NOT NULL PRIMARY KEY,
[FkId] UNIQUEIDENTIFIER NOT NULL,
[Data1] REAL NULL,
[Data2] REAL NULL,
FOREIGN KEY(FkId) REFERENCES C(Id)
)
{% endhighlight %}

The foreign key for table B was referencing a table that didn’t exist, called C. When I tried to insert data into table B I received the generic error “SQL error or missing database”. When written as above it is easy to spot the error. Unfortunately in reality the difference between the correct table name and the incorrect table name was quite subtle and it took me some time to spot my mistake.

(Note – SQLite allows you to legitimately create a table with an invalid foreign key relationship: “The parent key definitions of foreign key constraints are not checked when a table is created. There is nothing stopping the user from creating a foreign key definition that refers to a parent table that does not exist, or to parent key columns that do not exist or are not collectively bound by a PRIMARY KEY or UNIQUE constraint.”)