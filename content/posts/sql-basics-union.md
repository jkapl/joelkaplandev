+++ 
date = "2021-03-27" 
title = "SQL Notes: Performance, useful commands" 
slug = "sql-basics" 
tags = ["SQL", "performance", "notes"] 
categories = [] 
series = ["Theme", "Hugo"] 
+++

# Intro

Got slow-running SQL query? This post might be able to help. It's intended for beginner/intermediate SQLers like myself, so we'll cover the basics: the use of an `ALIAS`, the different types of `JOIN`s, the use of the [index](https://use-the-index-luke.com) to improve performance, and a nifty feature, the `UNION` - heavily crediting [this post](https://www.foxhound.systems/blog/sql-performance-with-union/) - to make queries super fast.

## SELECT

In this post we'll follow the performance of a query - from very slow, but correct, to super fast. We'll solve issues with missing column names, then when the data looks good, we'll focus on improving performance. Here's the table structure:

`table here`

|users     |user_groups|requests|request_additional_data|
|----------|-----------|--------|-----------------------|
|user_id   |group_id   |req_id  |req_id                 |
|first name|user_id    |data    |data3                  |
|last name |           |data2   |data4                  |

We want to support querying this data in many ways through the use of an Advanced Search UI. Users can filter on different types of records and opt to show only records that belong to their group(s) or their submitter ID. A first crack at a query would be the classic `SELECT *`

```
SELECT * FROM users, user_groups, requests, request_additional_data
JOIN users ON users.user_id = user_groups.user_id
JOIN 
```

In order to make these types of queries possible, we'll need to use the `LEFT JOIN`, which returns all rows from the left table, and matching rows from the right table.


## Notes


- SUBSTRING function, use to extract a substring from a field
  - `SUBSTRING(COL_NAME, 3)` 
- CAST function, use to cast to a type
  - `CAST(SUBSTRING(COL_NAME, 3) AS INT)`
- MAX, MIN
  - `SELECT MAX(rating), MIN(review_count)`