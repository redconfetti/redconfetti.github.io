---
layout: page
title: PostgreSQL
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

These commands are specific to Postgres installed on a Mac using [Homebrew](http://brew.sh/). I recommend using [Lunchy](https://github.com/mperham/lunchy) to manage the daemons running on Mac OSX machines.

See also [PostgreSQL SELECT Docs](http://www.postgresql.org/docs/9.1/static/sql-select.html){:target="_blank"}

## Command Line Commands

``` shell
# initialize your Postgres database cluster (collection of databases managed by Postgres server)
$ initdb /usr/local/var/postgres -E utf8

# Start Postgres server manually
$ pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start

# Stop Postgres server manually
$ pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log stop

# Create a user without a corresponding database
$ createuser myusername --no-createdb --no-superuser --no-createrole --pwprompt

# Create a databse with owner specified
$ createdb my_database --owner=myusername

# Drop a database
$ dropdb my_database

# Use PostgreSQL command line client to view default 'postgres' table
$ psql postgres

# Use PostgreSQL command line client to connect as a specific user, connected to specific database
$ psql -U myusername -d my_database

# Run an PostgreSQL command with user specified
$ psql -c 'CREATE DATABASE my_database WITH OWNER myusername ENCODING 'UTF8';' -d canvas_test

# Backup single database to file
$ pg_dump my_database > backup_file_path

# Restore single database from file
$ psql my_database < backup_file_path

# Backup entire database cluster
$ pg_dumpall > full_backup_file_path

# Restore entire database cluster
psql -f full_backup_file_path postgres
```

## PSQL Client Commands

``` sql
-- get list of non SQL commands
\?

-- execute query every 5 seconds
select id from tablename limit 5; \watch 5

-- list databases
\l

-- connect to database
\c my_database

-- list tables in connected database
\dt

-- list columns on table
\d table_name

-- quit psql client
\q
```

