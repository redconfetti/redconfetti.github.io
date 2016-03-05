---
layout: post
status: publish
published: true
title: Obtain MySQL Query Statistics using Explain
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1453
wordpress_url: http://www.rubycoloredglasses.com/?p=1453
date: '2013-02-07 00:56:31 -0800'
date_gmt: '2013-02-07 00:56:31 -0800'
categories:
- Model
tags:
- mysql
- explain
comments: []
---
<p>Sometimes it really counts to restructure the queries made to your MySQL database, especially so that they do make use of indexes which are present on the table.</p>
<p>You can obtain information on which keys are being used with a query by using the <a href="http://dev.mysql.com/doc/refman/5.0/en/explain.html" target="_blank">EXPLAIN</a> statement before your SELECT statement. Here are some examples of it's <a href="http://dev.mysql.com/doc/refman/5.0/en/explain-output.html" target="_blank">output</a>.</p>
<pre class="brush:plain">mysql> EXPLAIN SELECT * FROM USERS WHERE area_id = 2;<br />
+----+-------------+---------+------+---------------+------+---------+------+------+-------------+<br />
| id | select_type | table   | type | possible_keys | key  | key_len | ref  | rows | Extra       |<br />
+----+-------------+---------+------+---------------+------+---------+------+------+-------------+<br />
|  1 | SIMPLE      | USERS   | ALL  | NULL          | NULL | NULL    | NULL |    6 | Using where |<br />
+----+-------------+---------+------+---------------+------+---------+------+------+-------------+<br />
1 row in set (0.03 sec)</p>
<p>mysql> EXPLAIN SELECT * FROM USERS WHERE group_id = 16515319;<br />
+----+-------------+---------+------+---------------------------+--------------------------+---------+-------+------+-------+<br />
| id | select_type | table   | type | possible_keys             | key                      | key_len | ref   | rows | Extra |<br />
+----+-------------+---------+------+---------------------------+--------------------------+---------+-------+------+-------+<br />
|  1 | SIMPLE      | USERS   | ref  | index_users_on_group_id   | index_users_on_group_id  | 4       | const |    1 |       |<br />
+----+-------------+---------+------+---------------------------+--------------------------+---------+-------+------+-------+<br />
1 row in set (0.00 sec)</pre></p>
