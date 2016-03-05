---
layout: post
status: publish
published: true
title: Finding Records without Specific Child in Many-to-Many Relationship
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1289
wordpress_url: http://www.rubycoloredglasses.com/?p=1289
date: '2012-07-16 21:57:49 -0700'
date_gmt: '2012-07-16 21:57:49 -0700'
categories:
- Model
tags:
- mysql
- many-to-many
comments: []
---
<p>Okay. Here is a tricky challenge. Let's say you are coding a blog system where Posts may have many Tags, and a tag can have many posts. Your database would have a 'posts' table, a 'tags' table, and a 'post_tags'. With Ruby on Rails this would be configured for an ActiveRecord model using the has_many through method.</p>
<pre class="brush:ruby">has_many :tags, :through => :post_tags</pre><br />
So, my dilema is...I only want posts which have an absence of a relationship with a specific record, which is the tag record representing 'horrible'. How do I query for a list of posts which are absolutely without a specific tag? Like say I have a 'horrible' tag, and I want all posts which are not tagged with 'horrible'. How would I accomplish this?</p>
<p>I'm not limiting myself to coding using an ActiveRecord relational query chain. I'm trying to accomplish this via MySQL using three test tables. I've identified that there is hope in accomplishing this using a <a href="http://www.codinghorror.com/blog/2007/10/a-visual-explanation-of-sql-joins.html" target="_blank">full outer join</a>, however such a query is not supported by MySQL. I've read other articles that have suggested <a href="http://www.xaprb.com/blog/2006/05/26/how-to-write-full-outer-join-in-mysql/" target="_blank">using a union</a> between a right and left outer join, but this didn't provide me with the records containing the null values I expected.</p>
<p>I suspect that the solution would first involve devising a query that iterates over each tag for each post, and where a record does not exist in the post_tags table it there is a NULL value for post_tags.id. After this is accomplished could a 'WHERE' statement be added which filters results to those which have the tag.id for 'horrible', which have a NULL value for post_tags.id.</p>
<p>Further searching and I <a href="http://stackoverflow.com/questions/6839500/need-sql-query-to-find-parent-records-without-child-records" target="_blank">found a solution</a> that isn't related to what I expected. Using a 'NOT EXISTS' option in the WHERE clause makes it possible to insert a query which returns a result. If a result is returned, the parent query includes the result.</p>
<pre class="brush:ruby">"SELECT p.id, p.title<br />
FROM posts p<br />
WHERE NOT EXISTS (<br />
    SELECT p.id FROM tags t<br />
    WHERE t.post_id = p.id<br />
    AND t.tag_name IN ('horrible')<br />
)"</pre><br />
This query is designed for a one-to-many relationship between two tables, where the 'tags' table includes the post_id and the tag in 'tag_name'. This is at least a little closer, but doesn't cover my many-to-many relationship requirement.</p>
