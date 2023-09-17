---
layout: post
published: true
title: Updating Vue-Loader to v15 with Webpacker
description:
date: 2020-03-01 11:37:00 -0800
comments: true
categories:
  - rails
tags:
  - webpacker
  - rails
  - vue
  - vue-loader
---

I recently decided to jump into working with [VueJS] again within the context
of a project I'm working on that uses Rails 5 with Webpacker.

I upgraded Webpacker from v3.6 to v4.2.2, Vue from v2.5.16 to v2.6.11,
and Vue-Loader from v14.2.2 to v15.9.0.

After making this update I got the following error:

<!--more-->

```javascript
ERROR in ./app/javascript/my_pack/components/App.vue
Module Error (from ./node_modules/vue-loader/lib/index.js):
vue-loader was used without the corresponding plugin. Make sure to include
VueLoaderPlugin in your webpack config.
Error: vue-loader was used without the corresponding plugin. Make sure to
include VueLoaderPlugin in your webpack config. at Object.module.exports
(/Users/jason/Projects/my_app/node_modules/vue-loader/lib/index.js:36:29)
```

I was able to identify that I needed to make some configuration changes after
[updatiing Vue-Loader from v14 to v15].

I'm a bit rusty on how to navigate the Node/JS eco-system, and the documentation
on Webpack doesn't apply to the Rails method of configuring the app with
Webpacker.

I recall that after I first setup this Rails app to use Vue with Webpacker that
I had to use the following rake task to configure it to use Vue:

```bash
bundle exec rails webpacker:install:vue
```

I wasn't able to resolve this error until I applied [these modifications] to
`config/webpack/environment.js`.

I didn't want to miss any other updates to the boilerplate configuration,
nor do I want my own modifications overwritten. So what I recommend you do
to upgrade is to make any commits in Git before re-running the above rake
task again. Here's what I got from the output:

```bash
$ bundle exec rails webpacker:install:vue
Copying vue loader to config/webpack/loaders
    conflict  config/webpack/loaders/vue.js
Overwrite /Users/jason/Projects/seed/config/webpack/loaders/vue.js? (enter "h" for help) [Ynaqdhm] Y
       force  config/webpack/loaders/vue.js
Adding vue loader plugin to config/webpack/environment.js
      insert  config/webpack/environment.js
      insert  config/webpack/environment.js
Adding vue loader to config/webpack/environment.js
      insert  config/webpack/environment.js
      insert  config/webpack/environment.js
Updating webpack paths to include .vue file extension
File unchanged! The supplied flag value not found!  config/webpacker.yml
Copying the example entry file to /Users/jason/Projects/seed/app/javascript/packs
      create  app/javascript/packs/hello_vue.js
Copying Vue app file to /Users/jason/Projects/seed/app/javascript/packs
      create  app/javascript/app.vue
Installing all Vue dependencies
         run  yarn add vue vue-loader vue-template-compiler from "."
```

Expect that it will duplicate certain modifications it makes, but if you use
`git diff` on the modified files afterwards you can surely detect these and fix
them.

```bash
$ git diff config/webpack/environment.js
diff --git a/config/webpack/environment.js b/config/webpack/environment.js
index 08d2baf..0f3a57d 100644
--- a/config/webpack/environment.js
+++ b/config/webpack/environment.js
@@ -1,8 +1,12 @@
 const { environment } = require("@rails/webpacker")
 const { VueLoaderPlugin } = require("vue-loader")
 const vue = require("./loaders/vue")
+const { VueLoaderPlugin } = require("vue-loader")
+const vue = require("./loaders/vue")

 environment.loaders.append("vue", vue)
 environment.plugins.prepend("VueLoaderPlugin", new VueLoaderPlugin())

+environment.plugins.prepend("VueLoaderPlugin", new VueLoaderPlugin())
+environment.loaders.prepend("vue", vue)
 module.exports = environment
```

I already manually added '.vue' under 'extensions' in the config/webpacker.yml
file. I deleted `app/javascript/app.vue` and `app/javascript/packs/hello_vue.js`
because I don't need them. Now I'm no longer getting the error from
`./bin/webpack-dev-server`.

[vuejs]: https://vuejs.org/
[updatiing vue-loader from v14 to v15]: https://vue-loader.vuejs.org/migrating.html#a-plugin-is-now-required
[these modifications]: https://github.com/rails/webpacker/blob/cb4e4c8c/lib/install/vue.rb#L7,L13
