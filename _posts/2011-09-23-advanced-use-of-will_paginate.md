---
layout: post
title: Advanced Use of Will_Paginate
date: '2011-09-23 18:08:56 -0700'
comments: true
categories:
- Ruby on Rails
tags:
- rails
- pagination
- will_paginate
---

I'm building an index of contacts, displayed with paginated links provided by [will_paginate][1].

The wiki for this plugin advises you on how to do setup your controller
method, and what to put in the view to obtain a simple set of links, such as:

``` ruby
# /app/controllers/contact_controller.rb
def index
  @contacts = Contact.paginate :page => params[:page], :per_page => 10, :order => 'created_at DESC'
end
```

<!--more-->

``` html
<!-- /app/views/contact/index.html.erb -->
<%= will_paginate @contacts %>
```

What I'm not finding however are more advanced methods of using the
will_paginate plugin. Here are a few things I've found.

### Links at top and bottom of paginated section

You can add the paginated links to the top and bottom of your paginated
section using this syntax:

``` html
<% paginated_section @contacts do %>
  <table id="contacts">
    <%= render(:partial => "contact_row", :collection => @contacts) %>
  </table>
<% end %>
```

### Display Page Entries Info

You can display text on your page such as 'Displaying contacts
__11 - 12__ of __12__ in total' by using the following view helper.

``` html
<%= page_entries_info @contacts %>
```

### Current and Total Page Number

If you want to display the total number of pages in your own way, you can do
this by using the 'total_pages' method of the paginated collection.

``` html
You are viewing page <%= @contacts.current_page %>
of <%= @contacts.total_pages %>
```

[1]: https://github.com/mislav/will_paginate/wiki
