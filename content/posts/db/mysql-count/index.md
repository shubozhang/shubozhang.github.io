---
title: "How to Use MySQL count"
date: 2020-12-20
menu:
    sidebar:
        name: How to Use MySQL count
        identifier: db-mysql-count
        parent: db
        weight: 10
hero: mysql-logo.png
---


### How many ways to use ``count``?
* `count(*)`: returns a count of the number of all rows (including NULL).
* `count(1)`: `1` evaludates to non-NULL for every row, so it returns the same results as `count(*)`
* `count(col_name)`: returns a count of the number of the rows that col_name is not NULL


### `count(col_name)` VS `count(*)`？
* `count(*)`: returns a count of the number of all rows (including NULL).
* `count(col_name)`: returns a count of the number of the rows that col_name is not NULL
* `count(*)` is recommended even though `count(pk)` can return the same result. Because MySQL optimizes `count(*)` and 
  makes it work more efficient. Note, this optimization does not work when `where` and `group by` in the query.


### `count(1)` VS `count(*)` ?
* `count(*)` is prefered. Because it is SQL92 standard syntax.
* In MySql, they should have the same performance. In PostgreSQL, it seems `count(*)` is faster about 10% than `count(1)`.
