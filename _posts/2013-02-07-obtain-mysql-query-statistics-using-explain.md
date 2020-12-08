---
layout: post
title: Obtain MySQL Query Statistics using Explain
date: '2013-02-07 00:56:31 -0800'
categories:
- Model
tags:
- mysql
- explain
---

Sometimes it really counts to restructure the queries made to your MySQL
database, especially so that they do make use of indexes which are present on
the table.

You can obtain information on which keys are being used with a query by using
the [EXPLAIN][1] statement before your SELECT statement. Here are some
examples of it's [output][2].
<!--more-->

```sql
mysql> EXPLAIN SELECT * FROM USERS WHERE area_id = 2;
+----+-------------+---------+------+---------------+------+---------+------+------+-------------+
| id | select_type | table   | type | possible_keys | key  | key_len | ref  | rows | Extra       |
+----+-------------+---------+------+---------------+------+---------+------+------+-------------+
|  1 | SIMPLE      | USERS   | ALL  | NULL          | NULL | NULL    | NULL |    6 | Using where |
+----+-------------+---------+------+---------------+------+---------+------+------+-------------+
1 row in set (0.03 sec)

mysql> EXPLAIN SELECT * FROM USERS WHERE group_id = 16515319;
+----+-------------+---------+------+---------------------------+--------------------------+---------+-------+------+-------+
| id | select_type | table   | type | possible_keys             | key                      | key_len | ref   | rows | Extra |
+----+-------------+---------+------+---------------------------+--------------------------+---------+-------+------+-------+
|  1 | SIMPLE      | USERS   | ref  | index_users_on_group_id   | index_users_on_group_id  | 4       | const |    1 |       |
+----+-------------+---------+------+---------------------------+--------------------------+---------+-------+------+-------+
1 row in set (0.00 sec)
```

[1]: http://dev.mysql.com/doc/refman/5.0/en/explain.html
[2]: http://dev.mysql.com/doc/refman/5.0/en/explain-output.html
