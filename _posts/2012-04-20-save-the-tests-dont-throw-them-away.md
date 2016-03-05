---
layout: post
status: publish
published: true
title: Save the Tests, Don't Throw Them Away
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1158
wordpress_url: http://www.redconfetti.com/?p=1158
date: '2012-04-20 17:48:02 -0700'
date_gmt: '2012-04-20 21:48:02 -0700'
categories:
- Testing
tags:
- testing
- tdd
comments: []
---
<p>So it's been several weeks since I started using test driven development. I'm using FactoryGirl instead of fixtures, because I've heard that fixtures are limiting. I'd rather just write Factories from the beginning. I'm also using standard Test::Unit based unit and functional tests. Haven't touched on integration testing yet.</p>
<p>As I've gone along and written these tests, I've learned how to do things effectively and am carving out my own style.</p>
<p>For instance you could either test for a link with the content 'Edit' on the page, or you could add an ID or class to the link and test for the presence of that link with that class. I found that I'd rather do both just to ensure that the button is or is not present in a certain circumstance.</p>
<pre class="brush:rails">assert_select "a.btn", { :count => 0, :text => "Edit"}, "Edit button shouldn't be present for sent request"<br />
assert_select "a.btn", { :count => 0, :text => "Delete"}, "Delete button shouldn't be present for sent request"<br />
assert_select "a.edit-request", false, "Edit button shouldn't be present for sent request"<br />
assert_select "a.delete-request", false, "Delete button shouldn't be present for sent request"</pre><br />
It may seem like a lot more coding is necessary for the tests than the code itself which it is testing. This is true, and is one of the reasons I was leery about writing tests. But here is the thing I wasn't aware of, at least not in the way I understand it now.</p>
<p>First off, it may look like a lot more code, but really it's just a lot of duplicated code that varies. Like in the above example, it's really two types of tests applied to two different buttons on the page. One type tests to ensure a link with the 'btn' class (used by Twitter Bootstrap) with certain text content isn't present, and the other checks to make sure a link with a specific class name isn't present. I did this so that if someone else changes the text of the button to say 'Edit Request', the class test will still catch if the link/button is present when it's not supposed to. Now that I have this type of test in place, when I need it again I can just copy and paste these tests for the correct syntax, then modify them to meet the needs of the new page I'm testing. So ultimately, it's not that much more coding. It's kind of like when a person looks at a mixing console in a recording studio and thinks "Wow, how can that guy understand what all those knobs and buttons do?". Once they realize that you just have to understand one column of those knobs and buttons, and that each other column applied to a different instrument in the music mix, all of a sudden understanding it becomes extremely simplified. It's the same thing with testing.</p>
<p><a href="http://www.redconfetti.com/wp-content/uploads/2012/04/mixingconsole.jpg"><img class="alignnone size-medium wp-image-1161" title="mixingconsole" src="http://www.redconfetti.com/wp-content/uploads/2012/04/mixingconsole-300x199.jpg" alt="" width="300" height="199" /></a></p>
<p> </p>
<p>Another point is that you don't have to write tests for every single little thing. For my application, requests that have been sent to a remote system later should not be edited or deleted. I don't want people trying to use buttons that shouldn't be showing on the screen, which is why I wrote the tests above. I know that I'd receive complaints from the users using the interface if those buttons are showing, and if they result in error when someone attempts to use them in the wrong context...or worse if they result in a sent request being modified, and thus an accurate history for that request is compromised instead of protected. However I found that when I was editing one of these requests, and the data I entered was invalid, a select form field that relied on a collection of options rendered as a text field instead of a select field. This is one of those special bugs that doesn't necessarily compromise a core function of the system. It's not going to result in some major failure or side effect that would warrant a test, so I've chosen to just fix the bug and move on without a test for that scenario. If the issue does pop up in practice more than I expect, then I'll write a test to avoid it happening again, but for now I'm picking and choosing my battles based on my experience and expectations.</p>
<p>The main point of this post that I wanted to make however is that you're doing it anyway. If you're not using test driven development, you're testing your models in the Rails console, or opening your browser and testing the app within the development environment. For model method testing, once you close your terminal window the coding you wrote in the IRB console is gone forever. The same principle of loss applies to browser based tests. You have to adjust the records in your development environment often, and go through a special sequence of events just to get the right state in your development environment with a browser, just to test one single result. This is EXTREMELY time consuming and laborious. With unit tests you can test the result of each method, and are actually more likely to implement tests using rare cases you would be too lazy to test for otherwise. With functional tests, the environment needed to properly test a specific scenario is much easier to setup and maintain, and is performed in seconds instead of minutes (or even hours).</p>
<p>I'm not yet opinionated regarding other testing options such as Rspec, Cucumber, etc. however I definitely see the benefit of picking up a habit of writing tests as the solution to a major waste of time later on after your project has grown. I have much less anxiety regarding unexpected bugs, and I know that once one is identified it can be fixed and a test can be written for it if necessary.</p>
