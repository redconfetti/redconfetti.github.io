---
layout: post
title: Generating Jekyll Posts from an External Source
date: '2023-09-16 17:31:00 -0400'
comments: true
categories:
  - Ruby
tags:
  - jekyll
  - generators
  - posts
  - cms
---

## Mission

I want to use a headless CMS with Jekyll as the source of my blog posts.
There aren't many plugins that aim to faciliate this.

There is a [WordPress jekyll-import][1] tool, but this is intended for a one
time import of Wordpress content to Markdown files inside of your Jekyll
project, not a continual build process that sources all content from an API.

[1]: https://import.jekyllrb.com/docs/wordpress/

## The Jekyll Engine

Jekyll Posts are just a natively supported form of [Jekyll collection][2]. The
documentation for Jekyll even states that if you configure your "collections" to
load from a different directory, you will need to move your `_posts` and
`_drafts` folder under that directory as well.

Jekyll builds your site through a process that involves the following steps:

* Read - Reads data from directories/files into the [Jekyll::Site][3] object
* Generate - Runs each of the Generators defined by plugins you've installed or
  coded yourself
* Render - Renders content in memory (markdown converted to HTML, SASS converted
  to CSS, etc.)
* Cleanup - Removes orphaned files and empty directories in destination
* Write - Writes static files, pages, and posts to build directory
  (e.g. `_site`)

The entire process is about "reading" data from files into Ruby objects that are
stored inside of the [Jekyll::Site][3] object. This includes pages, posts,
collections, and data.

After this it runs the generators defined by Jekyll plugins you install, or that
you write yourself.

Next it goes through a rendering process, where the content loaded from Markdown
files is converted to HTML. For instance, inside of each [Jeyll::Document][4]
object used to represent each blog post, the Markdown is stored to the
'content' attribute, but the rendered HTML is stored in the 'output' attribute.

The cleanup step performs the necessary file cleaning up in the site destination
directory.

Lastly all the rendered data is then written to actual files under the
destination directory (e.g. defaults to `_site`).

Here's how I inspected objects in memory through each step of the site building
process using IRB.

```ruby
require 'jekyll'

options = {
  "source"      => File.expand_path("."),
  "destination" => File.expand_path("./_site"),
  "incremental" => true,
  "profile"     => true,
  "watch"       => true,
  "serving"     => true,
}

# merge build options with configuration data
options = Jekyll.configuration(options)

# initialize the site object
site = Jekyll::Site.new(options)
site.class
# => Jekyll::Site

# initialize attribute defaults
site.reset

# read data from directories/files
site.read

# inspect posts collection
site.collections['posts'].class
# => Jekyll::Collection
site.collections['posts'].docs.count
# => 163

example_doc = site.collections['posts'].docs[0]
# => #<Jekyll::Document _posts/2004-09-09-bug-tracking.md collection=posts>

example_doc.path
# => "/Developer/redconfetti.github.io/_posts/2004-09-09-bug-tracking.md"
example_doc.type
# => :posts
example_doc.data
# => {
#   "draft"=>false,
#   "categories"=>["php"],
#   "layout"=>"post",
#   "published"=>true,
#   "title"=>"PHP/MySQL Bug Tracking",
#   "author"=>"maxwell keyes",
#   "date"=>2004-09-09 16:51:00 -0400,
#   "comments"=>true,
#   "tags"=>["bug tracking"],
#   "slug"=>"bug-tracking",
#   "ext"=>".md",
#   "excerpt"=><Jekyll::Excerpt id=/2004/09/bug-tracking#excerpt>
# }
example_doc.data["permalink"]
# => nil

# Content is the markdown string
example_doc.content
# => "For anyone who needs a free web based Bug Tracking system programmed
# using\nPHP/MySQL, check out Flyspray.\n"

example_doc.output
# => nil

# After file content is loaded into Jekyll::Site, it is rendered from Markdown
# to actual HTML using site.render
site.render
# => nil

example_doc.output
# => "<!DOCTYPE html>\n<html>\n  <head>\n  <meta charset=\"utf-8\">\n  
# <meta name=\"viewport\" content=\"width=device-width initial-scale=1\" />\n
# <meta http-equiv=\"X-UA-Compatible\" content=\"IE=edge\">\n\n
# <title>PHP/MySQL Bug Tracking</title>
# ...
# <footer class=\"footer\">\n  <span class=\"footer__copyright\">
# &copy; 2023 Jason Miller. All rights reserved.</span>\n</footer>\n\n
# </body>\n</html>\n"

site.cleanup

site.write
```

[2]: https://jekyllrb.com/docs/collections/
[3]: https://github.com/jekyll/jekyll/blob/d86ba153f/lib/jekyll/site.rb
[4]: https://github.com/jekyll/jekyll/blob/d86ba153f/lib/jekyll/document.rb#L34,L62

## Custom Post Approach

I tried to write a plugin/generator for Jekyll that used a class that inherits
from Jekyll::Document, and patches various methods so that it can be used
without sourcing data from a Markdown file under the `_posts` directory. I was
not able to get this to work without errors and complications.

Instead, it's better that you create a custom page template, as suggested by
the [Jekyll Generators][6] documentation, with a generator that locates the
custom page by name, and simply adds the custom data under the pages `data`
hash attribute.

[6]: https://jekyllrb.com/docs/plugins/generators/

### Liquid Drops

The only complication that could not be avoided is that the
[Liquid templating system][7] used by Jekyll will raise errors if you use
custom defined objects in your page template
(.e.g `undefined method 'to_liquid'`).

If the objects you are iterating over and injecting into your page are not
one of the [basic Ruby types][8], then you'll need to make sure the objects
you're iterating over inherit from [Liquid::Drop][9].

See [Liquid Drops][10]

[7]: https://shopify.github.io/liquid/basics/variations/#jekyll
[8]: https://shopify.github.io/liquid/basics/types/
[9]: https://github.com/Shopify/liquid/blob/main/lib/liquid/drop.rb
[10]: https://github.com/Shopify/liquid/wiki/Introduction-to-Drops

## Wordpress RSS Feed Example

Here's an example of code needed for a simple Jekyll generator that can
retrieve posts from an RSS/XML feed hosted under Wordpress.com.

```ruby
##########################################
# Gemfile

# XML to Hash translator
# https://github.com/savonrb/nori
gem 'nori', '~> 2.6'

# Nokogiri
# https://github.com/sparklemotion/nokogiri
gem 'nokogiri', '~> 1.15'

# Backport Jekyll Sass Converter to avoid deprecation warnings 
gem 'jekyll-sass-converter', '~> 2.2'

##########################################
# _config.yml

wp_posts_page:
  title: 'Blog'
  layout: 'wp_posts_page'
  feed_url: 'https://redconfetti.wordpress.com/feed/'

##########################################
# _plugins/wordpress_posts.rb

require "net/http"
require "uri"
require "nori"

module WordpressPosts
  class Generator < Jekyll::Generator
    def generate(site)

      @post_page_config = site.config['wp_posts_page']
      raise 'Missing Wordpress configuration in _config.yml' unless @post_page_config

      page_layout = @post_page_config['layout']
      page_title = @post_page_config['title']
      page_slug = page_title.strip
        .downcase
        .gsub(/[\s\.\/\\]/, '-')
        .gsub(/[^\w-]/, '')
        .gsub(/[-_]{2,}/, '-')
        .gsub(/^[-_]/, '')
        .gsub(/[-_]$/, '')

      feed_url = @post_page_config['feed_url']
      post_feed = WordpressFeed.new(feed_url)

      # get template
      posts_page = site.pages.find { |page| page.name == 'wp_posts_page.html'}

      posts_page.data['post_feed'] = post_feed.items
    end
  end
end

class WordpressFeed
  attr_accessor :rss_url

  def initialize(rss_url)
    self.rss_url = rss_url
  end

  def rss_channel
    @channel ||= begin
      rss = hash_data['rss'] || {}
      rss['channel']
    end
  end

  def items
    @item ||= begin
      [rss_channel['item']].flatten.collect do |item|
        ItemDrop.new(item)
      end
    end
  end

  private

  def hash_data
    @hash_data ||= begin
      if !xml_string.blank?
        return Nori.new.parse(xml_string)
      end
    end
  end

  def xml_string
    @xml_string ||= begin
      uri = URI(rss_url)
      Net::HTTP.get(uri)
    end
  end

  class ItemDrop < Liquid::Drop
    attr_accessor :feed_item
    def initialize(feed_item)
      self.feed_item = feed_item || {}
    end

    def content
      feed_item['content:encoded']
    end

    def title
      feed_item['title']
    end

  end
end
```

Page template (`_layouts/wp_posts_page.html`)

```html
---
layout: page
title: Blog
permalink: /blog/
---

<ul style="list-style-type: none; padding: 0; margin: 0;">
  {% for post in page.post_feed %}
    <li>
      <div>
        <div>
          <h2>{{ post.title }}</h2>
          <p>{{ post.content }}</p>
        </div>
      </div>
    </li>
  {% endfor %}
</ul>
```

## Storyblok

The above approach isn't very ideal due to how Wordpress.com embeds oversized
images into the posts. Also the free Wordpress hosting account limits the
feed to 350 posts.

Hopefully the above example provides you with enough understanding to obtain
content from any external data source and embed it into your custom Jekyll
pages.

If you want to go with a free CMS for your own site, consider [Storyblok][11].
The article, [Add a headless CMS to Jekyll][12], gives good instructions on
how to use the [Storyblok Ruby Gem][13] with Jekyll. With Storyblok you can
define your own schema for the objects you're embedding in your pages,
and nest the objects inside of the content of other objects.

[11]: https://www.storyblok.com/
[12]: https://www.storyblok.com/tp/headless-cms-jekyll
[13]: https://github.com/storyblok/storyblok-ruby-client
