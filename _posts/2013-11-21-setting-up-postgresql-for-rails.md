---
layout: post
title: Setting up PostgreSQL for Rails
date: '2013-11-21 20:14:34 -0800'
categories:
- Ruby on Rails
tags:
- postgresql
---
I've always used either SQLite (the default) with new Rails projects, or I've used MySQL because I've been using it ever since 2002 when I started doing web development with PHP. Recently however I was challenged with deploying an application to Heroku as part of a code challenge I'm taking part in. Unfortunately, Heroku doesn't support SQLite, and recommends PostgreSQL. Rather than waste time trying to create a MySQL app and running into problems, I'm going to go the easy route and use PostgreSQL.

The first step I had to take was installing [PostgreSQL using Homebrew](http://www.moncefbelyamani.com/how-to-install-postgresql-on-a-mac-with-homebrew-and-lunchy/). I figured it would default to using 'root' as the super user locally without a password, just like MySQL. Postgres is actually setup owned by your local user account though. This makes sense given that usually a daemon is setup to run as a certain user. Unix admins should create a 'postgres' user, login to that account, then initialize and run the database as that user.

I find that it's useful to control when the Postgres server is running using the [Lunchy gem](https://github.com/mperham/lunchy). It makes it easy to start and stop daemons such as this that are installed via Homebrew.

``` shell
$ gem install lunchy
$ lunchy start postgres
$ lunchy stop postgres
```

## Creating a User for your Rails App

You likely don't want to configure your Rails app to use your username in development. It's best to use a username that is related to your application. The following command will let you add a super user/role to Postgres with the ability to create databases, create other user/roles, and login.

``` shell
$ createuser --superuser --createrole --createdb --login myappuser
```

If you need to delete a user/role, you can use 'dropuser'.

``` shell
$ dropuser myappuser
```

Now that a super user is setup, you can run the rake commands to create the database and run migrations.

``` shell
$ bundle exec rake db:create:all

$ bundle exec rake db:migrate

$ bundle exec rake db:migrate RAILS_ENV=test
```

## Command Line Client

The command line client is '[psql](http://www.postgresql.org/docs/8.4/static/app-psql.html)'. It defaults to using your actual Linux/Mac username, which is fine because Postgres is running under your username locally anyway. It requires a database name also, so you'll have to specify a database name to even get the prompt. You can use 'postgres' as the database name. Otherwise use your applications database name if you want.

``` shell
$ psql postgres
psql (9.2.4)
Type "help" for help.
postgres=#
```

Also 'quit' or 'exit' don't get you out of the client. You have to use '\q'. You can use the 'help' command as prompted to see other commands.

If you want to view a list of users directly from the 'postgres' database, you can use the following query.

``` shell
SELECT * FROM pg_roles;
```

To view a list of databases, you can directly use the psql command from the command line.

``` shell
$ psql -l
```
