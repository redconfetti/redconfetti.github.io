---
layout: post
published: true
title: A New Solution for Personal and Small Business Websites
description: The Promise of the JAMstack Approach
date: 2020-12-05 09:02:40 -0700
categories:
  - jamstack
tags:
  - gatsby
  - headlesscms
  - render
  - github-pages
---

## Static Website Beginnings

I've maintained my own personal blog, and maintained websites for small
businesses, since 2000. I started building websites by manually coding the files
in HTML, and creating images I crafted in Photoshop. I'd update the website by
uploading the files to the web hosting server using FTP. I later expanded on
this by learning CSS and JavaScript. For the longest time I used
[Macromedia Dreamweaver] (now owned by Adobe), because it was the closest thing
to a web-standard compliant WYSIWYG editor, with a text/code editor that
supported code coloring.<!--more--> In 2002 I multiplied the power I yielded in
developing functional websites by learning how to use server-side PHP coding
with the MySQL database. This was my humble beginning as a full-stack developer.

For myself this was fine, but for small businesses their relationship with
their "webmaster" could easily become strained because being available to update
a clients website usually didn't pay much. Many website maintainers might be
unresponsive for days or weeks before updating your website. Even communicating
the simple text changes you desired were difficult to convey via email to your
webmaster.
## My Romance with Wordpress

I worked for a hosting company from 2004 - 2008 so that I could expand upon my
ability to create websites by understanding the web hosting servers that host
the websites. I pursued learning Linux system administration, which included
learning how to install and configure all related services manually from the
Linux command line. The WHM/cPanel system, which made management of a server
simpler via a web-based management interface, had just grown in popularity in
2004, and was expanding in use to power armies of website servers in data
centers around the world. The majority of websites hosted from these servers
were powered by PHP/MySQL.

During this time many individuals and companies were adopting Content Management
Systems (CMS) to power their website, because they provided an "administration"
area that allowed them to edit the content on their webpages, or even create
new pages or blog posts, without needing to contact a website developer. These
systems were PHP/MySQL applications, so working with them was a no brainer.

The one CMS that stuck out due to its ease of use was [Wordpress]. Even though
Wordpress was designed as a blog rather than a CMS, Wordpress can be configured
to support a website by configuring a custom page as the homepage, rather
than the blog index page it defaults to. Additionally the blog functionality can
be hidden from the public website by removing the blog index page from the
websites navigation menu altogether. This approach, combined with the hundreds
of plugins and themes made for Wordpress made it a perfect platform for any
personal or small business website. Really it could meet the needs of any
website short of complex custom web applications.

The cost and hassle of designing and maintaining a website had been
eliminated. Someone is now able to setup and maintain a website themselves
without coding skills necessary what so ever. Today you can sign-up for
Wordpress hosting with companies such as [BlueHost] or [HostGator], purchase and
download a beautiful premium design/theme from a site such as [ThemeForest], and
then upload and activate that theme in your Wordpress dashboard. Setting up the
pages and content for the website are all accessible to those with average
computer skills when using the [Wordpress administrative Dashboard].

I would tell everyone I knew that if they have a small business, Wordpress is
the best solution for their website, as opposed to limited template based
websites provided by platforms like Wix or SquareSpace. Wordpress has much more
flexibility and ability to expand in functionality as your business grows. You
can use free themes/plugins to begin, upgrade to premium themes/plugins later,
and if necessary you can even hire designers or developers to create completely
custom themes or plugins as needed.

For a period of time I had my own cPanel web server running from a [Virtual
Private Server (VPS)] hosted by [Linode]. I had created and hosted several
websites for small businesses and and friends hosted from this platform.

## The Pitfalls of Wordpress

I had tried to support these Wordpress sites by logging into their admin area
and updating the themes, plugins, and the Wordpress software itself when I
could. Everytime I did this I worried that I would unknowingly break the
functionality somewhere on these websites. Eventually down the line, this is
exactly what would happen.

Updates to the Wordpress software, which are necessary to avoid security issues
with your website, often cause plugins and themes you are using to stop working
properly. The developers of Wordpress plugins and themes bare no guarantee that
they will provide updates so that your website continues to function properly.
Premium themes or plugins may offer some additional support and updates in the
future, but there is still no guarantee of this.

I purchased an all-purpose theme builder framework called "Headway" for some of
the sites I maintained. It was not able to support upgrades or migrations to the
newer major versions of the framework theme they had released, so many sites
were stuck with the version they were using. Unexpectedly, "Headway" had longer
continued to be supported and maintained as of 2016. This was very frustrating
for the websites I had invested so much time into designing using their
framework.

Often enough I would bare the responsible for helping to fix a website that had
become defaced by hackers, or contain mysterious scripting used to run phishing
scams. Sometimes hackers would try to send spam email from my server via a
hacked website, thus causing other website users to have issues sending email
from my server, because my server had been added to a spam blacklist as a result
of the spam activity.

Just a note: Hosting your email from a cPanel service is not worth the hassle
either. I recommend using an email service such as [Google Workspace],
[Fastmail], or [ProtonMail].

## The Promise of JAMstack

So what's a better solution to setting up and maintaining a person or small
business website? JAMstack!

Right now the this approach is only available to companies with budgets large
enough to hire custom website developers to implement, but I forsee this
becoming more acccessible to non-developers in the future.

So what is this solution, and how is it better?

The JAMstack recipe calls for using a static website generator to generate a
website that is powered by modern development tools such as [ReactJS] and
[GraphQL], that sources your content from a Headless Content Management System
(CMS). Static websites have no code running on the server side, so there is much
lower risk of your website being hacked. The only code is the JavaScript which
runs in the visitors web browser. Because your content is managed separately
by a Headless CMS, your content can be published not only to your website but
also to other platforms like mobile applications.

The full JAMstack solution is made possible by using multiple components that
are provided as separate or integrated services. These include:

* Headless CMS - Like Wordpress, this enables you to manage the text and images
  that show up in your website in the form of pages, posts, or other types of
  content.
* Custom Website Code - Powered by a static website generator like [Gatsby];
  provides your site design, sources your updated content from the Headless
  CMS, to create your static website presentation.
* Git repository host - Most people use [Github], [Gitlab], or [BitBucket] to
  host their website code in a Git repository.
* Build system - This system is triggered by updates to your website code,
  or by updates to your content in the Headless CMS. This system might be a
  continuous integration / continuous delivery (CI/CD) system, or simply
  a script setup on your web server. When triggered it builds the updated
  version of your website and deploys it to the public website.
* Hosting - This can be a simple web server, a cluster of servers, or a content
  delivery network (CDN).

Several Headless CMS services currently exist with free tier plans.
See [JAMstack - Top Headless Content Management Systems]

So all you need to do if you're a small business with the resources to hire a
developer is ask them to to integrate a website layout you choose from a premium
website design template market such as [ThemeForest] into a JAMstack codebase
powered by frameworks such as [Gatsby] or one of the other [site generators],
and configure it to automatically regenerate and publish to your static hosting
provider each time you update or add any content to the Headless CMS.

[Netlify] currently provides a CMS, build system, and CDN hosting with a free
tier plan to help you get started, with paid plans once your business needs
more. [Render] also provides an integrated build service and hosting.

Hopefully at some point in the future a flexible all-in-one solution will exist
that enables a business to create a website as quick and easy as Wordpress, but
powered by a flexible JAMstack infrastructure that you can override with your
own custom front-end once your business needs custom development work.

It turns out that designers are starting to publish Gatsby themes already.

There may be some themes on ThemeForest that support Gatsby, such as [Flexiblog]
which supports the [Contentful] or [Sanity] CMS. If you really want to find a
design that supports this approach, I'd recommend checking out
[JAMstack Themes].

[Flexiblog]: https://themeforest.net/item/flexiblog-react-gatsby-blog-template/27538998
[JAMstack Themes]: https://jamstackthemes.dev/
[Macromedia Dreamweaver]: https://en.wikipedia.org/wiki/Adobe_Dreamweaver
[Wordpress]: https://wordpress.org/
[Wordpress administrative Dashboard]: https://wordpress.com/support/dashboard/
[ThemeForest]: https://themeforest.net/category/wordpress
[BlueHost]: https://www.bluehost.com/wordpress
[HostGator]: https://www.hostgator.com/managed-wordpress-hosting
[Virtual Private Server (VPS)]: https://en.wikipedia.org/wiki/Virtual_private_server
[Linode]: https://www.linode.com/
[Google Workspace]: https://workspace.google.com/
[Fastmail]: https://www.fastmail.com/
[ProtonMail]: https://protonmail.com/
[Github]: https://github.com/
[Github Pages]: https://pages.github.com/
[Gitlab]: https://about.gitlab.com/
[Bitbucket]: https://bitbucket.org/
[What is JAMstack]: https://jamstack.org/what-is-jamstack/
[JAMstack - Top Headless Content Management Systems]: https://jamstack.org/headless-cms/
[Gatsby]: https://www.gatsbyjs.com/
[Site Generators]: https://jamstack.org/generators/
[Netlify]: https://www.netlify.com/
[Render]: https://render.com/
[ReactJS]: https://reactjs.org/
[GraphQL]: https://graphql.org/
[Sanity]: https://www.sanity.io/
[Contentful]: https://www.contentful.com/
