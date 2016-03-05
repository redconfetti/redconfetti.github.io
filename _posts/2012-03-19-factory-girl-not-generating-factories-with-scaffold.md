---
layout: post
title: Factory Girl Not Generating Factories with Scaffold
date: '2012-03-19 14:18:44 -0700'
categories:
- Testing
tags:
- rails3.1
- factory_girl
comments: []
---
<p>I just started a new Rails 3.2 project, and to ensure that the proper test files are generated using Shoulda or Factory_Girl, I've installed those gems and configured the application to generate the test files using these gems.</p>
<p>Added to config/application.rb:</p>
<pre class="brush:rails">
    # Configure generators values.<br />
    # http://guides.rubyonrails.org/generators.html<br />
    config.generators do |g|<br />
      g.stylesheets false<br />
      g.test_framework :shoulda<br />
      g.fallbacks[:shoulda] = :test_unit<br />
      g.fixture_replacement :factory_girl<br />
    end<br />
</pre></p>
<p>Each time I would try to create a new scaffold, it would use shoulda to generate the test unit file, but would generate a YAML fixture.</p>
<pre class="brush:shell">
invoke active_record<br />
create db/migrate/20120319180004_create_posts.rb<br />
create app/models/post.rb<br />
invoke shoulda<br />
create test/unit/post_test.rb<br />
create test/fixtures/posts.yml<br />
</pre></p>
<p>Over a year ago there was <a href="https://github.com/indirect/rails3-generators" target="_blank">a gem needed</a> to ensure that generators were present to generate Factory_Girl factories instead of YAML fixtures, but the code for those generators was moved to the official Factory_Girl gem, so that's not the cause of this issue.</p>
<p>It turns out that I had configured factory_girl_rails in my Gemfile only under the 'test' group, and not the 'development' group as well.</p>
<pre class="brush:rails">
group :development, :test do<br />
  gem 'shoulda'<br />
  gem 'factory_girl'<br />
  gem 'factory_girl_rails'<br />
end<br />
</pre></p>
<p>After configuring these under both test and development, the scaffold generator created the factory under 'test/factories' as I had expected.</p>
