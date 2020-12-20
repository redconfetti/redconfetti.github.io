---
layout: post
title: Duplicate associated records when using FactoryGirl
date: '2012-09-09 23:26:46 -0700'
comments: true
categories:
- Testing
tags:
- factory_girl
comments: []
---
When I decided to start using tests as part of my development practice, I had
the choice of using the [default fixture system], or using one of the recommended
[fixture alternatives], also known as factories. I decided upon using
[FactoryGirl] given it's popular mention on the web, and one of the projects at
work was using it.

I had read much about the horrors if using fixtures, with the most memorable
opinion of them being that they were 'brittle'. This, and many other complaints,
caused me to skip using fixtures and jump directly to using FactoryGirl.
<!--more-->

FactoryGirl has been pretty great. The one issue I've found however is that the
methods for creating associations between parent and child records, and
scripting the generation of those records, has proven to be poorly documented
and thus difficult to work with.

After a bit of investigation I gained the understanding that the 'association'
attribute used with a factory will cause the parent factory to be generated. If
you need the presence of child records in your test, then you need to use the
'after_create' callback with a code block that generates the children records.
So the 'association' option goes up (parent), not down (child).

This appears to be intended for [creating multiple child records] in a has_many
relationship. Often, I really don't like having 'Article 1', 'Article 2',
'Article 3', etc. be the name for my content generated. I want to be able to
define a handful (or more) example records that are either used alone, or
associated with generated records.

Recently on another project I had a unique relationship in place. This project
calls for 'words' which have child 'definitions', and the definitions have
parent 'publications', but the 'words' do not have any association with
'publications'. I really didn't know how to handle this. I'd rather keep it
simple, not so complicated. I just want FactoryGirl to create a record, and
associated records, without creating duplicates (or errors where the duplicate
record cannot be created due to unique value constraints).

So today I found the holy grail of overcoming this issue. I wish it was part of
the official FactoryGirl documentation, because seriously this seems common
enough of an issue for me that I'd expect that others have the issue. Anyway, I
found the answer in this [StackOverflow question].

You can code your factories so that they find the other factory that already
exists, and makes the association, or generates a new one. Not only is this
possible through this rather simple solution, but you can also implement more
advanced creation code.

So in my case I'm building a [glossary system] that has a 'definition' model
which has two parents, 'phrase' and 'publication'. The following code implements
this solution for creating a single 'phrase' parent of the 'definition'.

<script src="https://gist.github.com/redconfetti/6255612.js"></script>

As you can see my specialized version of the 'definition' factory, named
'definition_cheese_and_spinach_omelette' uses the get_phrase_named() method to
generate the 'Omelette' phrase it should belong to. It will always associate
with the phrase that has the name 'Omelette', whether it exists already or not.

Another part of the beauty here is that I was also able to add coding to this
method which generates the letter and 'cached-slug' when the Phrase with name
submitted doesn't already exist.

[default fixture system]: http://guides.rubyonrails.org/testing.html#the-low-down-on-fixtures
[fixture alternatives]: https://www.ruby-toolbox.com/categories/rails_fixture_replacement
[factorygirl]: https://github.com/thoughtbot/factory_girl/
[creating multiple child records]: https://github.com/thoughtbot/factory_girl/blob/master/GETTING_STARTED.md#associations
[stackoverflow question]: http://stackoverflow.com/questions/7145256/find-or-create-record-through-factory-girl-association
[glossary system]: http://glossary.ahalmaas.com/
