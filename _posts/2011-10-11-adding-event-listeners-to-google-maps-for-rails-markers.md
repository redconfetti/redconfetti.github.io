---
layout: post
title: Adding Event Listeners to Google-Maps-for-Rails Markers
date: '2011-10-11 11:24:27 -0700'
categories:
- View
tags: []
comments:
- id: 68948
  author: Wellington Torrejais da Silva
  author_email: wtds.trabalho@gmail.com
  author_url: ''
  date: '2015-03-03 13:57:02 -0800'
  date_gmt: '2015-03-03 13:57:02 -0800'
  content: Thanks!
---
<p>I'm currently working on a Ruby on Rails project that was setup under Rails 2, and we're upgrading it to Rails 3 via a separate branch.</p>
<p>We were using a gem called <a href="https://github.com/yawningman/eschaton" target="_blank">Eschaton</a> to provide Google Maps on certain pages of the website, but it appears that Eschaton is abandoned and actually going to be deleted on October 31, 2011. Because of this I <a href="https://github.com/redconfetti/eschaton" target="_blank">forked the project</a> under my own Github account.</p>
<p>After looking around for a suitable replacement we decided on <a href="https://github.com/apneadiving/Google-Maps-for-Rails" target="_blank">Google-Maps-for-Rails</a>, version (1.3.0). Previously with Eschaton we had jQuery code associated with event listeners that were triggered when the markers on the map are selected. When selected the items being plotted on the map, which are displayed to the left of the map, were highlighted using CSS after being clicked on, and page also scrolled down to the item selected on the map. To reproduce this we need to tie events to the markers using Google Maps for Rails (gmaps4rails).</p>
<p>After some searching and testing I found this to be the solution. The 'content_for :scripts' container ensures that the Javascript is inserted in the bottom of the page via the '<%= yield :scripts %>' that is placed in your layout. The Javascript is also outputted at the bottom of the page AFTER the Javascript which the gem outputs that initializes and loads the Google Map.</p>
<pre class="brush:rails">
<script charset="utf-8" type="text/javascript"><br />
// <![CDATA[<br />
		Gmaps.map.callback = function() {<br />
			for (var i = 0; i <  Gmaps.map.markers.length; ++i) {<br />
 				google.maps.event.addListener(Gmaps.map.markers[i].serviceObject, 'click', function(object) {<br />
  					alert('lat: '+object.latLng.Na+' long: '+object.latLng.Oa);<br />
 				});<br />
 			}<br />
 		}<br />
// ]]><br />
</pre></p>
<p>After further coding I found that I needed to reference the MySQL ID of each of the objects, which is included in the ID of each element on the page. I updated my controller to use the following so that the MySQL ID would be available in the JSON:</p>
<pre class="brush:rails">@json = @properties.to_gmaps4rails do |property|<br />
  ""id": "#{property[0].id}""<br />
end</pre><br />
I then needed to figure out how to pass this ID to the function that is passed to the 'google.maps.event.addListener'. I did this like so:</p>
<pre class="brush:rails">
<script charset="utf-8" type="text/javascript"><br />
// <![CDATA[<br />
		Gmaps.map.callback = function() {<br />
			for (var i = 0; i <  Gmaps.map.markers.length; ++i) {<br />
				var marker = Gmaps.map.markers[i].serviceObject;<br />
 				marker.marker_id = Gmaps.map.markers[i].id;<br />
 				google.maps.event.addListener(marker, 'click', function() {<br />
 					alert('marker id: '+this.marker_id);<br />
 				});<br />
 			}<br />
 		}<br />
// ]]<br />
></script></pre></p>
