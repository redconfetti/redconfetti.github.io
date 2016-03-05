---
layout: post
title: Form Fields not Displaying with Formtastic
date: '2011-10-18 10:48:34 -0700'
categories:
- View
tags: []
comments: []
---
<p>The Rails project I'm currently working on uses <a href="https://github.com/justinfrench/formtastic" target="_blank">Formtastic</a>, a Rails form builder plugin. The projects description says "Formtastic is a Rails FormBuilder DSL (with some other goodies) to make it far easier to create beautiful, semantically rich, syntactically awesome, readily stylable and wonderfully accessible HTML forms in your Rails applications." I wasn't sure what DSL means, but found in the projects wiki on the <a href="https://github.com/justinfrench/formtastic/wiki/1-About" target="_blank">About page</a> that it stands for <a href="http://en.wikipedia.org/wiki/Domain_specific_language" target="_blank">Domain Specific Language</a>.</p>
<p>So I'm currently upgrading from Rails 2.3.8 to Rails 3.1, and I found that the form fields were not showing for the pages using the Formtastic semantic_form_for code blocks. I updated my own code so that the equal sign is included after the opening Ruby code tag in the views such as:</p>
<pre class="brush:rails">
<%= semantic_form_for @product do |f| %><br />
</pre></p>
<p>&#8230;instead of&#8230;</p></p>
<pre class="brush:rails">
<% semantic_form_for @product do |f| %><br />
</pre></p>
<p>This was one of the first steps I performed as advised by the rails 3 upgrade instructions. I just now realized however that inside of the code block are other code blocks for the form fields and the submit button. These too must include the equal sign, which was the reason why my form fields were not displaying.</p></p>
<pre class="brush:rails">
<%= f.inputs :name => "Author", :for => :author do |author_form| %><br />
  <%= author_form.input :first_name %><br />
  <%= author_form.input :last_name %><br />
<% end %></p>
<p><%= f.buttons do %><br />
  <%= f.commit_button %><br />
<% end %><br />
</pre></p>
