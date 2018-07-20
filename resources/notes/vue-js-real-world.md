---
layout: page
title: VueJS Real World Vue
---

# Real World Vue

Covering everything from installation to deployment.

- Vue CLI
- Vue Router
- Vuex

We'll learn how to create and work with:

- Filters
- Mixins
- Lifecycle hooks
- Custom directives

We'll be using real data, making API calls with Axios, using Firebase's new
Cloud Firestore for the backend, and adding user authentication.

We'll even implement some beautiful transitions and animations.

# Vue CLI 3 - Creating our Project

The Vue CLI (command line interface) provides a full system for rapid Vue.js
development. It allows you to select libraries your project will use,
configures Webpack, provides options for the recipe you want to use (single-file
.vue components, TypeScript, SCSS, Pug, latest versions of ECMAScript, etc.),
and it enables Hot Module Replacement (HMR) via the [webpack-dev-server].

## Installing the CLI

Node.js version 8 or above should be installed.

```bash
npm i -g @vue/cli
```

## Creating a Vue project

You can create a project using [Vue UI], or from the CLI client.

```bash
vue create real-world-vue
```

You're presented with the option to pick a default preset, or to manually select
features. Using the down arrow key, highlight _Manually select features_ then
press the ENTER key.

Next use the down arrow key to move down, then use the space bar to select
_Router_, _Vuex_, and _Linter / Formatter_, then press ENTER.

Next choose _ESLink + Prettier_ as the Linter / Formatter, and then
_Lint on Save_.

When prompted on where to place config for Babel, PostCSS, ESLint, etc, choose
_In dedicated config files_.

It will ask you if you wish to save all of this as a preset for future projects.
Type 'N' and press ENTER to decline that option. If you choose 'Y' instead,
a JSON file named `.vuerc` will be stored in your home directory.

## Serving Our Project

Once the project is finished being created, you can `cd` into it.

To view it live in your browser, run the command:

```shell
npm run serve
```

By default it will already have a _Home_ page and an _About_ page, using Vue
Router.

[webpack-dev-server]: https://github.com/webpack/webpack-dev-server
[vue ui]: https://github.com/vuejs/ui
