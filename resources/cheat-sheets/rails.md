---
layout: page
title: Ruby on Rails
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

## Generate New Rails App

```bash
# Generate for API only
rails new my_app --api

# Using MySQL database
rails new_my_app --database=mysql

# Using PostgreSQL database
rails new my_app --database=postgresql

# Without Turbolinks
rails new my_app --skip-turbolinks

# Without JavaScript
rails new my_app --skip-javascript
rails new my_app -J

# Without Sprockets
rails new my_app --skip-sprockets
rails new my_app -S

# Without Tests
rails new my_app --skip-test

# Don't run Bundle Install
rails new my_app --skip-bundle

# Preconfigure for app-like JavaScript with Webpack (options: react/vue/angular)
rails new my_app --webpack=WEBPACK

# Preconfigure for Vue with Webpack
rails new my_app --webpack=vue
```

## Rake Tasks

```ruby
# Display Rails routing table
rake routes
```

## ActiveRecord

```ruby
# Get name of table associated with model
Model.table_name
# Get field/column names from database table
Model.column_names
```

## Capistrano

```bash
# View available Capistrano tasks
bundle exec cap -vT
```
