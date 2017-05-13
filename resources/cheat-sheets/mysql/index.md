---
layout: page
title: MySQL
---

[Back to Cheat Sheets](/resources/cheat-sheets/)

## Command Line Commands

``` shell
# Log into MySQL server as root with password
$ mysql -u root -p
````

## MySQL Client Commands

``` sql
# quit client
quit;

# show list of databases
show databases;

# use database
use redconfetti;

# show tables in current database
show tables;

# create user account accessible from localhost
CREATE USER 'user'@'localhost' IDENTIFIED BY 'secretpass';

# create user account accessible from any host
CREATE USER 'user'@'%' IDENTIFIED BY 'secretpass';

# grant all privileges on database to user
GRANT ALL PRIVILEGES ON my_database.* TO 'user'@'%';
```
