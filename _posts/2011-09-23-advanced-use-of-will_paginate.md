---
layout: post
status: publish
published: true
title: Advanced Use of Will_Paginate
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 859
wordpress_url: http://www.redconfetti.com/?p=859
date: '2011-09-23 18:08:56 -0700'
date_gmt: '2011-09-23 22:08:56 -0700'
categories:
- Ruby on Rails
tags:
- rails
- pagination
- will_paginate
comments: []
---
<p>I'm building an index of contacts, displayed with paginated links provided by <a href="https://github.com/mislav/will_paginate/wiki" target="_blank">will_paginate</a>.</p>
<p>The wiki for this plugin advises you on how to do setup your controller method, and what to put in the view to obtain a simple set of links, such as:</p>
<pre class="brush:rails"># /app/controllers/contact_controller.rb<br />
def index<br />
  @contacts = Contact.paginate :page => params[:page], :per_page => 10, :order => 'created_at DESC'<br />
end</p>
<p># /app/views/contact/index.html.erb<br />
<%= will_paginate @contacts %></pre><br />
What I'm not finding however are more advanced methods of using the will_paginate plugin. Here are a few things I've found.</p>
<h3>Links at top and bottom of paginated section</h3><br />
You can add the paginated links to the top and bottom of your paginated section using this syntax:</p>
<pre class="brush:rails"><% paginated_section @contacts do %></p>
<table id="contacts">
		<%= render(:partial => "contact_row", :collection => @contacts) %><br />
	</table><br />
<% end %></pre></p>
<h3>Display Page Entries Info</h3><br />
You can display text on your page such as 'Displaying contacts <strong>11 - 12</strong> of <strong>12</strong> in total' by using the following view helper.</p>
<pre class="brush:rails"><%= page_entries_info @contacts %></pre></p>
<h3>Current and Total Page Number</h3><br />
If you want to display the total number of pages in your own way, you can do this by using the 'total_pages' method of the paginated collection.</p>
<pre class="brush:rails">
<p>You are viewing page <%= @contacts.current_page %><br />
of <%= @contacts.total_pages %></p></pre><br />
 </p>
