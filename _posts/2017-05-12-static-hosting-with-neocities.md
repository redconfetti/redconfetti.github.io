---
layout: post
title:  "Static Hosting with Neocities"
date:   2017-05-12 09:43:51 -0700
categories:
- hosting
tags:
- static
- neocities
- jekyll
---

I've been using a Wordpress site for my blog for years, but that has become
cumbersome, especially when you have to deal with your website being exploited
due to holes in one of the many plugins that your site is relying on.

I used to focus on LAMP stack development, and so running my own
[cPanel/WHM server] was a no brainer. I more recently migrated my tech blog,
[Ruby Colored Glasses], from Wordpress to Github Pages. This is nice because the
site is hosted for free by Github, however that's limited to one site per each
account.

So I've decided to try to find another cheap low-cost static website solution
that works with [Jekyll](https://jekyllrb.com/).

[cPanel/WHM server]: https://cpanel.com/
[Ruby Colored Glasses]: http://www.rubycoloredglasses.com/
<!--more-->

## Neocities

Back in the 90's there used to be a free hosting solution known as GeoCities
that hosted many awesome websites for many people. This is where many people
were able to express themselves in their own unique ways, while also
learning HTML.

Neocities hopes to provide the same type of community. For $5 a month you can
[become a Supporter](https://neocities.org/supporter), which earns you the
ability to create as many sites as you wish, use a custom domain with each site,
and also use WebDAV to manage files. If you want the same for a lifetime, you
can send them [$100 via Bitcoin](https://neocities.org/supporter/bitcoin).

So my goal at the current moment is to explore if it's possible to generate a
website via Jekyll, and then upload it to one of the Neocities sites.

## Import

One of the first steps for me is to [import my Wordpress site]. There is a
plugin that one can use to import all of their Wordpress content into a Jekyll
site, however it requires that you have direct MySQL access to your server.

Note: The importer only converts your posts and creates YAML front-matter. It
does not import any layouts, styling, or external files (images, CSS, etc.).

[import my Wordpress site]: http://import.jekyllrb.com/docs/wordpress/

### Configure Server for Public Access

I had to go into my server and configure MySQLd to bind to more than just the
local host address. This required that I edit
`/etc/mysql/mysql.conf.d/mysqld.cnf` on the Ubuntu machine I'm currently hosting
the site from and change the IP to `0.0.0.0`.

```ini
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
# bind-address          = 127.0.0.1
bind-address            = 0.0.0.0
```

I was then able to connect directly to the server and authenticate. Luckily I
didn't have any sort of firewall blocking the ports. I tested the connection
using this command:

```shell
mysql --host=123.321.123.5 --user=my_user --password=mySecr3tPaSS my_database_name
```

### Install Gems

```shell
gem install jekyll-import unidecode sequel mysql2 htmlentities
```

### Perform Import

I'm choosing to import all the files to `md` (Markdown) file extensions instead
of 'html'. This command worked just fine for me.

```shell
ruby -rubygems -e 'require "jekyll-import";
  JekyllImport::Importers::WordPress.run({
    "dbname"   => "my_db_name",
    "user"     => "my_db_username",
    "password" => "my_secret_password",
    "host"     => "192.123.193.12",
    "socket"   => "",
    "table_prefix"   => "wp_",
    "site_prefix"    => "",
    "clean_entities" => true,
    "comments"       => true,
    "categories"     => true,
    "tags"           => true,
    "more_excerpt"   => true,
    "more_anchor"    => true,
    "extension"      => "md",
    "status"         => ["publish"]
  })'
```

This resulted in all my pages and posted imported into the repository, and ready
for a long cleanup.

## Resources

Here are some various links I've explored in finding a low-cost Jekyll based
hosting solution.

* [DesignRope - Static Web Hosting: Who's Best?](https://designrope.com/toolbox/static-web-hosting/)
* [Aerobatic](https://www.aerobatic.com/#features)
* [Neocities](https://neocities.org/)
  * [Neocitizen](https://github.com/aergonaut/neocitizen) - Used to public site
    from a folder
* [Neocities API](https://neocities.org/api)
* [Jekyll Docs - Wordpress Import](http://import.jekyllrb.com/docs/wordpress/)
* [Jekyll Docs - Deployment Methods](https://jekyllrb.com/docs/deployment-methods/)
* [James Ward - Jekyll on Heroku](https://www.jamesward.com/2014/09/24/jekyll-on-heroku)
* [Netlify](https://www.netlify.com/) - Static hosting with a free entry tier
* [Hugo](https://gohugo.io/overview/usage/) - an alternative to Jekyll I assume
* [Smashing Magazine - Static Website Generators Reviewed](https://www.smashingmagazine.com/2015/11/static-website-generators-jekyll-middleman-roots-hugo-review/)
