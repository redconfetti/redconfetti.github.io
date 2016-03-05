---
layout: post
title: Refinery Extension Not Named After Model
date: '2013-06-18 06:01:25 -0700'
categories:
- Ruby on Rails
tags:
- refinery-cms
comments: []
---
<p>A project I'm working on currently relies on <a href="http://refinerycms.com/" target="_blank">Refinery CMS</a> to administrate the pages. Instead of building our own separate admin area for our own custom models, we're continuing to use Refinery for our non-page models as well.</p>
<p>Refinery and it's extensions are generated under their own namespace to ensure that they play nice with any system you install Refinery into. It can provide page management inside of an existing Rails app you have, or it can act as the center of the entire website. I'm pretty sure the main concept is that it leaves the view/layout presentation up to you, but provides the page administration back-end for you.</p>
<p>If you have special models that you want Refinery to manage as well, you can generate a <a href="http://refinerycms.com/guides/getting-started#extending-refinery-with-your-first-engine" target="_blank">Refinery extension</a>, which is essentially a Rails engine created under vendor/extensions. Something that I really didn't like however was how every new model you want to add gets it's own extension folder. Since the extensions are namespaced, you could add a new extension called 'book' and end up with the model under vendor/extensions/books/app/models/refinery/book/book.rb, and defined as Refinery::Books::Book.</p>
<p>I found out later though that you can add <a href="http://refinerycms.com/guides/multiple-resources-in-an-extension" target="_blank">new resources to existing extensions</a>. Now instead of having a large tree/hierarchy of files displayed in my project file view, it can be compacted under one single folder, with all the interrelated models under the same namespace. I can see that this same paradigm applies to the <a href="https://github.com/refinery/refinerycms-blog" target="_blank">Refinery-Blog plugin</a> with<span style="line-height: 21px;"> posts, comments, and categories configured under the same extension.</span></p>
<h2>Extension with Independent Name</h2><br />
<span style="line-height: 21px;">One of the things that bothered me here is that you cannot generate an extension using a name not associated with a model. For instance, the Refinery-Blog plugin mentioned above is called 'Refinery-Blog', but there is no 'blog' model. There are just posts, comments, categories, etc. The plugin is configured with the post admin page being the defined URL for the plugin itself, as a 'post' is the centerpiece of a 'blog'.</span></p>
<p><span style="line-height: 21px;">For someone that isn't familiar with the logic behind the extension code generated, this can be frustrating.</span></p>
<p>I've found the following method to accomplish this using the generator however.</p>
<p>First, create a folder for your new extension. For this example we'll call the extension 'shapes', with 'square' being first resource we plan on adding to the extension. You'll have to create and populate certain files so that the generator can modify the existing files as it's designed to do. Do replace 'shapes' in these examples with the name of your extension where applicable.</p>
<ol>
<li>Create folder 'vendor/extensions/shapes/'</li>
<li>Create folder 'vendor/extensions/shapes/config'</li>
<li>Create file 'vendor/extensions/shapes/config/routes.rb'</li>
<li>Create file 'vendor/extensions/shapes/lib/refinerycms-shapes.rb'</li><br />
</ol><br />
Make sure your config/routes.rb file has the following inside it.</p>
<pre class="brush:ruby">Refinery::Core::Engine.routes.append do<br />
end</pre><br />
After this is done, I ran the following command to generate my first resource within the extension I created by hand.</p>
<pre class="brush:shell">rails g refinery:engine square size:integer --extension shapes --namespace shapes</pre><br />
 </p>
<p>It created the new resource without any errors. Now I can re-creating the resources that were previously created under their own extensions, so that they're all under my new extension. I expect this will be cleaner, and more appropriate to how they should be added.</p>
<p> </p>
