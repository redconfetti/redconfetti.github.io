---
layout: page
title: Build a YouTube Clone Application Using React
---

Notes from [Build a YouTube Clone Application Using React]

The following notes are for Mac users. You'll need to use some commands specific
to your system as needed.

## Setup

### VSCode

This course uses VSCode to demonstrate all examples. You can enable the terminal
within VSCode you can use CTRL + ` (backtick), or use the menu - View > Terminal.

### Create-React-App

You need to have the [create-react-app] tool installed to use from the command
line, which requires that you have NodeJS installed.

```shell
$ npm i -g create-react-app

$ which create-react-app
/usr/local/bin/create-react-app

$ cd Projects
$ mkdir youtube-api
$ cd youtube-api
$ create-react-app ./
```

### Install Dependencies

Once the script finishes, we'll install our dependencies.

```shell
npm -i --save axios @material-ui/core
```

### Material UI

[Material-UI] provides pre-styled React components for you to use, similar to
how Bootstrap provides UI elements out of the box for new projects. It follows
the patterns of [Material Design].

It has a container system, grid system, buttons, etc.

### Start the Dev Server

```shell
npm start
```

### Remove Source Folder

We're going to start over by removing the `src` folder and creating a new one.

```shell
rm -rf src
mkdir src
```

[create-react-app]: https://www.npmjs.com/package/create-react-app
[build a youtube clone application using react]: https://www.youtube.com/watch?v=VPVzx1ZOVuw
[material-ui]: https://material-ui.com/
[maerial design]: https://material.io/design/introduction/
