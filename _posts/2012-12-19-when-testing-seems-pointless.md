---
layout: post
title: When Testing Seems Pointless
date: '2012-12-19 02:51:53 -0800'
categories:
- Testing
tags:
- testing
- tdd
- unit-testing
comments: []
---
<p>I remember when I was first exposed to the concept of test driven development (TDD), it seemed like you were writing a test that did the same thing as the function itself. This really left me perplexed as to why everyone was raving about it's value.</p>
<p>Take for instance the following method:</p>
<pre class="brush:rails">class SomeClass<br />
  def self.todays_date<br />
    Time.now.strftime("%Y-%m-%d")<br />
  end<br />
end</pre><br />
All this method does is return the date in 'YYYY-MM-DD' format. This might be used to name a log file, or in an <a href="http://railscasts.com/episodes/202-active-record-queries-in-rails-3" target="_blank">ActiveRecord finder method</a> call.</p>
<p>The unit test for this method, using RSpec, would look like this:</p>
<pre class="brush:rails">describe ".todays_date" do<br />
  it 'returns todays date in YYYY-MM-DD format' do<br />
    result = Contest.todays_date<br />
    result.should == Time.now.strftime("%Y-%m-%d")<br />
  end<br />
end</pre><br />
Doesn't that just seem silly? Yes!</p>
<p>But one thing to remember about tests is that they ensure that parts of your application do what you expect them to do. It's only with simple methods like this that you have tests that are so simple that they seem useless. However, even in this case, the test shown above is not a total waste. With this test in place, I can trust that any other part of my application which needs to use this method can do so and expect the same result. This test is my enforcer, ensuring that the contract between that method and the rest of my code is maintained, a contract which says SomeClass.todays_date will always return todays date in 'YYYY-MM-DD' format.</p>
<p>Ensuring that the modules, classes, and objects you've designed provide the expected interface, perform the expected actions, and return the expected results, makes it so that you can move on and code other parts of your system without worrying if you should handle some situation where another entity might fail. You can focus on one unit at a time, and switch to the context of the dependencies and integration between the units when necessary, without having to think of them all at once within the limited memory of your human mind.</p>
<p>For more complex methods, the test helps you catch errors as you create the method, and in the future when you modify the method. The tests even act as a form of documentation, as they provide an example of how the rest of your code might interface with your method.</p>
<p>There is surely a learning curve to unit testing like this, and integration testing which ensures your units work together as expected. Once you develop the skills necessary to employ testing in your application, you'll realize that the peace of mind obtained once your system becomes a large system is very valuable. It allows you to be more agile, not needing to be extra careful. Able to bring on new developers that aren't completely familiar with your code like you are. Refactoring major parts of the system are also easier, as the tests point out every area of the application which isn't behaving in a way which the rest of the application expects.</p>
<p> </p>
