---
layout: post
published: false
title: Webpacker Jumpstart
date: 2017-11-24 16:45:00 -0500
categories:
- rails
tags:
- webpacker
- yarn
- vue
- angular
- react
---

I've been working on an application that utilizes [Gulp] to automate builds,
with [Browserify] to support packaging the assets for an [AngularJS]
application.

I've wondered if there is a better way of integrating a NodeJS driven
task/asset pipeline system with a Rails application. I was excited to see that
the Rails team has adopted integration of [Webpack][Webpack] (an alternative to
Browserify) via the [Webpacker gem](https://github.com/rails/webpacker/).

Rails 5.1 supports an option to generate your application using
[AngularJS](https://angularjs.org/), [Vue.js](https://vuejs.org/), or
[React](https://reactjs.org/).

``` shell
# Install with Webpack for Vue
rails new my_app --webpack=vue

# Install with Webpack for Angular
rails new my_app --webpack=angular

# Install with Webpack for React
rails new my_app --webpack=react
```

You'll notice that the command examples above do not suggest that you use the
`--api` option. This isn't recommended because Webpacker still expects to
include your JavaScript compiled assets into Rails generated pages, using
helpers intended for use in your applications ERB driven layout. We have to
remember that Vue, Angular, or React are not always used to support single page
applications.

I've tried to follow some really good articles on this such as:

* [Embracing Change Rails 5.1 Adopts Yarn Webpack and the JS Ecosystem][1]
* [Webpacker README][2]
* [Rails 5 &amp; Vue: How to Stop Worrying and Love the Frontend][3]

## Yarn

[Yarn] is an improved alternative to the NPM package manager. It makes use of
packages provided by the NPM registry, and recognizes the same `package.json`
file configuration.

The Rails supports Webpack with help from the [Webpacker gem], which requires
that Yarn be installed.

[Gulp]: https://gulpjs.com/
[Browserify]: http://browserify.org/
[AngularJS]: https://angularjs.org/
[Webpack]: https://webpack.js.org/concepts/
[Webpacker gem]: https://github.com/rails/webpacker/
[Yarn]: https://code.facebook.com/posts/1840075619545360
[1]: http://samuelmullen.com/articles/embracing-change-rails51-adopts-yarn-webpack-and-the-js-ecosystem/
[2]: https://github.com/rails/webpacker/tree/b160e6bb#webpacker
[3]: https://mkdev.me/en/posts/rails-5-vue-js-how-to-stop-worrying-and-love-the-frontend
