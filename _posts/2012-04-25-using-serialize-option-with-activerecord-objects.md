---
layout: post
title: Using Serialize Option with ActiveRecord Objects
date: '2012-04-25 19:01:26 -0700'
categories:
- Model
tags:
- rails3.1
- forms
comments: []
---
Documentation seems to be more available on how to build forms with check boxes or a multiple select field for ActiveRecord objects that have a has_many or <a href="http://railscasts.com/episodes/17-habtm-checkboxes-revised?view=asciicast" target="_blank">has_many_and_belongs_to</a> association with other ActiveRecord objects. This article shows you how provide a multiple select form based on a custom defined array, with the selected options stored in a single attribute of your ActiveRecord object.

Lets say you are working on a form for a blog post that needs a multi-select field of statically defined adjectives, with the one or many adjectives saved to one field for the post.

<pre class="brush:rails">
def self.adjectives

  [

      'awesome',

      'phenomenal',

      'terrific',

      'fantastic',

      'amazing',

      'outstanding',

      'stupendous',

      'great',

      'incredible',

      'magnificent',

      'impressive',

      'excellent',

      'sensational',

      'fantasmagoric',

      'legendary',

      'marvelous'

  ]

end

```

Next, inside of your model, insert a line indicating the name of the string or text field you're going to use to store the serialized values from the form.

<pre class="brush:rails">
class Post < ActiveRecord::Base

  serialize :positive_adjectives, Array

end

```

In the view file for your form, insert the following tag to create a select tag which loads all the options with the previously selected ones saved to the post field in a single field, serialized in YAML format.

<pre class="brush:rails">
<%= select_tag 'post[positive_adjectives]', options_for_select(Post.adjectives, @post.positive_adjectives), { :multiple => true, :size => 10 } %>

```

It appears that <a href="http://stackoverflow.com/questions/2080347/activerecord-serialize-using-json-instead-of-yaml/5979949" target="_blank">there are methods</a>, possibly native ones for Rails 3.2.2 soon, for storing your objects in the database in JSON format instead of YAML.

