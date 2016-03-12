---
layout: post
title: Database Schema Information
date: '2009-03-13 17:20:50 -0700'
categories:
- Ruby on Rails
tags:
- mysql
- rails
---
It can be very useful to have the database table schema information available to you when you are working on a model in a Ruby on Rails application. There is a plugin available which provides the schema information in comments at the top of each model called Annotate Models Plugin.

``` sql
# == Schema Information
# Schema version: 20090215021706
#
# Table name: orders
#
#  id                     :integer(11)     not null, primary key
#  order_number           :integer(11)     default(0), not null
#  created_on             :datetime
#  shipped_on             :datetime
#  order_user_id          :integer(11)
#  order_status_code_id   :integer(11)     default(1), not null
#  notes                  :text
#  referer                :string(255)
#  order_shipping_type_id :integer(11)     default(1), not null
#  product_cost           :float           default(0.0)
#  shipping_cost          :float           default(0.0)
#  tax                    :float           default(0.0), not null
#  auth_transaction_id    :string(255)
#  promotion_id           :integer(11)     default(0), not null
#  shipping_address_id    :integer(11)     default(0), not null
#  billing_address_id     :integer(11)     default(0), not null
#  order_account_id       :integer(11)     default(0), not null
#  subscription_id        :integer(11)
```

You can install the plugin using the following command from the root of your Rails application.

``` shell
script/plugin install http://repo.pragprog.com/svn/Public/plugins/annotate_models
```

After you are done installing the plugin, simply run the rake task by using this command:

``` shell
rake annotate_models
```
