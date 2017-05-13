---
layout: post
status: publish
published: true
title: Adding Event Listeners to Google-Maps-for-Rails Markers
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: ''
author_login: redconfetti
author_email: jason@redconfetti.com
wordpress_id: 5
wordpress_url: http://rails.redconfetti.com/?p=5
date: '2011-10-11 11:24:27 -0700'
date_gmt: '2011-10-11 18:24:27 -0700'
categories:
- prerequisites
---
I'm currently working on a Ruby on Rails project that was setup under Rails 2, and we're upgrading it to Rails 3 via a separate branch.

We were using a gem called [Eschaton](https://github.com/yawningman/eschaton) to provide Google Maps on certain pages of the website, but it appears that Eschaton is abandoned and actually going to be deleted on October 31, 2011. Because of this I [forked the project](https://github.com/redconfetti/eschaton) under my own Github account.

After looking around for a suitable replacement we decided on [Google-Maps-for-Rails](https://github.com/apneadiving/Google-Maps-for-Rails), version (1.3.0). Previously with Eschaton we had jQuery code associated with event listeners that were triggered when the markers on the map are selected. When selected the items being plotted on the map, which are displayed to the left of the map, were highlighted using CSS after being clicked on, and page also scrolled down to the item selected on the map. To reproduce this we need to tie events to the markers using Google Maps for Rails (gmaps4rails).

After some searching and testing I found this to be the solution. The 'content_for :scripts' container ensures that the Javascript is inserted in the bottom of the page via the '&lt;%= yield :scripts %&gt;' that is placed in your layout. The Javascript is also outputted at the bottom of the page AFTER the Javascript which the gem outputs that initializes and loads the Google Map.

``` html
<%= gmaps4rails(@json) %>

<% content_for :scripts do %>
	<script type="text/javascript" charset="utf-8">
		Gmaps.map.callback = function() {
			for (var i = 0; i <  Gmaps.map.markers.length; ++i) {
				google.maps.event.addListener(Gmaps.map.markers[i].serviceObject, 'click', function(object) { 
					alert('lat: '+object.latLng.Na+' long: '+object.latLng.Oa);
				});
			}
		}
	</script>
<% end %>
```

After further coding I found that I needed to reference the MySQL ID of each of the objects, which is included in the ID of each element on the page. I updated my controller to use the following so that the MySQL ID would be available in the JSON:

``` html
@json = @properties.to_gmaps4rails do |property|
  "\"id\": \"#{property[0].id}\""
end
```

I then needed to figure out how to pass this ID to the function that is passed to the 'google.maps.event.addListener'. I did this like so:

``` html
<%= gmaps4rails(@json) %>

<% content_for :scripts do %>
	<script type="text/javascript" charset="utf-8">
		Gmaps.map.callback = function() {
			for (var i = 0; i <  Gmaps.map.markers.length; ++i) {
				var marker = Gmaps.map.markers[i].serviceObject
				marker.marker_id = Gmaps.map.markers[i].id;
				google.maps.event.addListener(marker, 'click', function() {
					alert('marker id: '+this.marker_id);
				});
			}
		}
	</script>
<% end %>
```

