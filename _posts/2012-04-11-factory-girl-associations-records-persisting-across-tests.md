---
layout: post
title: Factory Girl Associations and Records Persisting Across Tests
date: '2012-04-11 22:32:23 -0700'
comments: true
categories:
- Testing
tags:
- rails3.1
- testing
- factory_girl
- tdd
comments:
- id: 338
  author: Philip Hale
  author_email: phil989@gmail.com
  author_url: http://www.pghale.com
  date: '2012-04-25 12:44:56 -0700'
  content: "Thanks for the article - I'm experiencing just this problem.  It's
    a shame there isn't more of a resolution to the problem though....\n\nFor
    example. I have a controller test which tests that the 'index' action
    correctly assigns Course.all to an array @courses. \n\n
    assigns(:courses).should eq([course])\n\nA simple
    and robust test which uses FactoryGirl to make course.  Unfortunately, it
    doesn't work because the :id's do not match between the two.\n\nI'm not
    sure how to fix this yet but I'm working on it.  \n\nI don't want a test
    that says \"@courses is an array containing an object which has the same
    name as course ...\"\n\nI want a test that says \"@courses is an array
    containing course\" !\n\nBest of
    luck with TDD - I'm loving it, like you say it catched bugs so early, some of which you don't even mean to test for."
---

I just recently started to adopt test driven development practices. The
project I'm working on needs to get done soon, and I didn't want to get held
up learning Rspec. After much consulting with other developers at the
[company I work][1] for, I had decided to use basic Test::Unit tests with
FactoryGirl factories instead of fixtures, and adopt Shoulda if a scenario
arises where the options it provides (contexts) are needed.

So far things have been running well, and I'm starting to understand just how
important testing is. You don't have to write tests for every single thing you
do, but if you implement some sort of feature that you seriously don't want to
break at some point in the future, setup a test for it. Once you setup a test
for one type of feature, you can re-use the code later for similar testing. So
don't worry about how long it takes the first time around, it will pay off
later when that function isn't broken because you caught it. I didn't realize
it, but errors you didn't expect it to directly catch, like the dreaded
"undefined method 'foo' for nil:NilClass" exception, also popup periodically
and alert you that you broke something, even though your test wasn't built to
catch those. This is nice because you might change something in a model, and
then all of a sudden something in a view is broken.
<!--more-->

Earlier today I had a new functional test I wrote fail because it was
expecting a view to render an index of child records that Factory Girl wasn't
creating. For this example 'User' has multiple 'Posts', and the post records
belonging to the user weren't being generated. I expected that FactoryGirl was
ActiveRecord aware, and would simply create the dependent post records, but
that's not the case. Much research online, sifting through articles with the
older syntax used by previous version of FactoryGirl, led to much confusion.
For a bit I was thinking that one needs to declare associations for children,
instead of parents, and then use Factory.create on the highest level parent so
that all the children records are generated before your test. This wasn't the
case.

It turns out that you should only use associations to define the relationship
inside of a child factory for it's parent, not the other way around. If you do
it the other way around, you'll end up getting 'stack level too deep' errors.
I expected that perhaps there would be some sort of way of defining that a
factory should create or build the children records, which are defined
separately, but for practicality it doesn't seem this is the case. Instead,
Factory Girl expects you to use the 'after_create' callback to cause
associated children records to be created. I guess this makes sense, as it
would be too redundant to create multiple factories in a separate file, much
like you define child fixtures. Its more encapsulated to generate the children
with the parent in the same code block.

``` ruby
factory :user do
  association :group
  name "John Smith"
  created_at "2011-04-11 12:00:00"
  factory :user_with_posts do

    # default to 5 posts
    ignore do
      posts_count 5
    end

    after_create do |user, evaluator|
      FactoryGirl.create_list(:post, evaluator.posts_count, user: user)
    end

  end
end
```

The above declaration would allow one to create a user with 5 posts by
default, or create one with 15 posts instead. Ironically enough, I figured
this out by referring to the [official FactoryGirl GETTING STARTED][2] docs,
after searching elsewhere on the internet.

``` ruby
FactoryGirl.create(:user).posts.length # 0
FactoryGirl.create(:user_with_posts).posts.length # 5
FactoryGirl.create(:user_with_posts, posts_count: 15).posts.length # 15
```

I've been creating factories instead of fixtures, using factories exclusively
in my application. I assumed that somehow when I ran tests that Test::Unit
and/or FactoryGirl would automatically create and destroy the records which
are created for each test, so that there is a clean slate each time an
individual test is run. Further investigation pointed to the term being
'transactional', with a deprecated 'use_transactional_fixtures' setting that
was declared as either TRUE or FALSE for ActiveSupport::TestCase. It appears
this is the default now for tests.

Once I added a factory that generates a parent with children records, I had
another controller test report an error where the assert_select didn't find
the form with ID I had expected. That ID was for one of the children records,
with a form expected using id "edit_message_1", but was instead getting
"edit_message_41". Further investigation suggested that records are persisting
across tests.

Then I inspected log/test.log, and saw that there were transactional commands
occurring with BEGIN and ROLLBACK commands [used by MySQL][3].

``` shell
   (0.1ms)  BEGIN
   (0.1ms)  SAVEPOINT active_record_1
  SQL (0.2ms)  INSERT INTO `posts` (`title`, `body`, `created_at`, `updated_at`) VALUES ('Foo', 'This is Foo', '2012-04-12 03:56:24', '2012-04-12 03:56:24')
   (0.1ms)  RELEASE SAVEPOINT active_record_1
   (0.1ms)  SAVEPOINT active_record_1
  SQL (0.2ms)  DELETE FROM `posts` WHERE `posts`.`id` = 1
   (0.1ms)  RELEASE SAVEPOINT active_record_1
   (0.4ms)  ROLLBACK
```

And yet for each ID of each record created by Factory Girl the number was
incremented each time. I was perplexed why the tests were transactional,
however the ID's were being incremented. I'm using MySQL 5.5.19, with InnoDB
tables, so the transactional commands being used should be supported.

I then spoke with a co-worker and he informed me that the tests do use
transactions, so the records are removed after each test. The transactional
queries however do not stop the MySQL database from auto-incrementing the ID
for the other additional records. He advised that I simply not write the tests
to rely on a specific ID, but instead rely on something static in the view
like the class name for an element, or that there is the correct number of
elements for a given class.

Further more he recommended not relying on view testing so much via the
Functional/Controller tests and to instead do more of that via integration or
acceptance tests, such as those using Capybara to do a full-stack test from
the browser perspective.

[1]: http://www.kabam.com/
[2]: https://github.com/thoughtbot/factory_girl/blob/master/GETTING_STARTED.md
[3]: http://dev.mysql.com/doc//refman/5.0/en/commit.html
