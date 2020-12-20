---
layout: post
published: true
title: Generating a New Rails Project
date: 2020-12-10 08:38:55 -0700
comments: true
categories:
  - rails
  - vuejs
tags:
  - api
  - webpacker
  - editorconfig
---

The great thing about working with a framework like Ruby on Rails is how you
can pick and choose the components that make up your application. Developers
often have preferences concerning solutions or tools they use in their projects.
They have the choice of testing tools such as [Rspec] or [Cucumber], testing
factory libraries such as [FactoryBot] or [Fabrication], authentication
solutions such as [Devise] or [AuthLogic], and authorization solutions such as
[CanCan] or [Pundit].

It can feel heavy having to reconfigure a freshly generated Rails application
to use these different components, because you spend most of your time
configuring the new application rather than jumping into actually building
the functionality.

[Rspec]: https://rspec.info/
[Cucumber]: https://github.com/cucumber/cucumber-rails
[FactoryBot]: https://github.com/thoughtbot/factory_bot
[Fabrication]: https://www.fabricationgem.org/
[Devise]: https://github.com/heartcombo/devise
[AuthLogic]: https://github.com/binarylogic/authlogic
[CanCan]: https://github.com/ryanb/cancan
[Pundit]: https://rubygems.org/gems/pundit

## Application Templates

Thankfully Rails supports [Application Templates] that make configuring a new
application a breeze.

It's possible for you to configure your own custom template with your primary
preferences all in one file. You could maintain your own template in a
[Github Gist] using this option.

See [my example template].

[Application Templates]: https://guides.rubyonrails.org/rails_application_templates.html
[Github Gist]: https://gist.github.com/
[my example template]: https://gist.github.com/redconfetti/011aac614ebe3bd7d798b431fe789f5a

### App Template Command

Even better than this is that you can choose to apply certain configurations
to your application ala carte by using the 'app:template' rails command. This
command allows you to run template fragments from publicly hosted template
scripts. You can find many scripts for various solutions on [Rails Bytes].

[Rails Bytes]: https://railsbytes.com/

## My Recipe

I prefer to use tools provided by the JavaScript / ECMAScript eco-system
facilitated by NodeJS packages for my front-end, using Webpack. Ruby on Rails
applications get support for this from [Webpacker]. I like ReactJS, but I
prefer VueJS for my own personal projects.

Because I use Webpack as my asset pipeline for the front-end application and
assets, I don't need the Rails asset-pipeline or page rendering helpers. This
causes me to prefer using an API-only configuration with Ruby on Rails.

To create a new application, I use the following command to generate an
[API Application], using [Webpacker] to with a VueJS front-end.

```shell
rails new my_application --api --webpack=vue
```

[API Application]: https://guides.rubyonrails.org/api_app.html#creating-a-new-application
[Webpacker]: https://github.com/rails/webpacker#installation
[VueJS]: https://vuejs.org/

After I'm done generating the application, I use the following application
templates to setup my app.

* [EditorConfig][0] - Specifies file formatting guidelines for many code editors
* [Rspec][1] - Popular unit testing framework for Ruby
* [FactoryBot][2] - Fixtures replacement for testing
* [StandardRB][3] - Ruby style guide, linter, and formatter. See [StandardRB]
* [dotenv][4] - A Ruby gem to load environment variables from `.env`.
  See [dotenv-rails]

[StandardRB]: https://github.com/testdouble/standard
[dotenv-rails]: https://github.com/bkeepers/dotenv

[0]: https://railsbytes.com/public/templates/V4Yslo
[1]: https://railsbytes.com/public/templates/z0gsLX
[2]: https://railsbytes.com/public/templates/XnJsbX
[3]: https://railsbytes.com/public/templates/xjNs4x
[4]: https://railsbytes.com/public/templates/zOvsQ0

Here are some optional recommendations that depend on the type of application
you are developing:

* [Friendly ID][50] - “Swiss Army bulldozer” of slugging and permalink plugins
  for Ruby on Rails. See [friendly_id]
* [Ahoy][51] - Track visits and events in Ruby, JavaScript, and native apps.
  Data is stored in your database by default so you can easily combine it with
  other data. See [Ahoy]
* [PgHero][52] - Performance Dashboard for PostgreSQL. See [PgHero]
* [Pundit][53] - Authorization system. See [Pundit]
* [Vue with InertiaJS][54] - Library supporting conventional interface between
  front-end tools (React, Vue.js, and Svelte) and back-end (Laravel and Rails).
  See [InertiaJs]
* [SendGrid][55] - Configures Ruby support for [SendGrid] email service API
* [GraphQL][56] - Add a GraphQL API to your Rails app
* [DelayedJob][57] - Background Job processing. See [DelayedJob]
* [Sidekiq][58] - Background Job processing. See [Sidekiq]

[Ahoy]: https://github.com/ankane/ahoy
[friendly_id]: https://github.com/eric/friendly_id
[PgHero]: https://github.com/ankane/pghero
[InertiaJs]: https://inertiajs.com/
[SendGrid]: https://sendgrid.com/
[GraphQL]: https://graphql.org/
[DelayedJob]: https://github.com/collectiveidea/delayed_job
[Sidekiq]: https://github.com/mperham/sidekiq/

[50]: https://railsbytes.com/public/templates/VqqsgV
[51]: https://railsbytes.com/public/templates/V1bs4X
[52]: https://railsbytes.com/public/templates/VWesJz
[53]: https://railsbytes.com/public/templates/X6ks6o
[54]: https://railsbytes.com/public/templates/z5OsYq
[55]: https://railsbytes.com/public/templates/x7msBK
[56]: https://railsbytes.com/public/templates/VwysPq
[57]: https://railsbytes.com/public/templates/zmnsgN
[58]: https://railsbytes.com/public/templates/VAjsZ6
