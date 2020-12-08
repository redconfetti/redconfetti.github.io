---
layout: post
title: Changing Column Order via ActiveRecord Migration
date: '2010-08-15 22:09:40 -0700'
comments: true
categories:
- Ruby on Rails
---

Is it possible to change the order of the columns in your MySQL (or other
database) table using a migration? Lets see.

If you check the [ActiveRecord::Migration documentation][1] you'll see there is
a method called 'change_column' which accepts various options.

```html
<tt>change_column(table_name, column_name, type, options)</tt>:
Changes the column to a different type using the same parameters as add_column
```

As of Rails 2.3.6 this is now available by using the :after option. You'll
have to include the field type, even though you are not modifying the type.

Example:

```ruby
change_column :orders, :tax_rate, :float, :after => :tax_state
```

[1]: http://api.rubyonrails.org/classes/ActiveRecord/Migration.html
