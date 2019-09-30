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
$ npm install
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

We're going to start over by removing the `src` folder and recreate it with
our files from scratch.

```shell
rm -rf src
mkdir src
cd src
touch index.js
touch app.js
mkdir components
```

### Create Index and App

```javascript
// src/index.js
import React from "react"
import ReactDOM from "react-dom"

import App from "./app"

ReactDOM.render(<App />, document.querySelector("#root"))
```

```javascript
// src/app.js
import React from "react"

class App extends React.Component {
  render() {
    return <h1>YouTube Clone App</h1>
  }
}

export default App
```

Here we create our App class as a Class based component. Another type of
component you can create is a functional component.

Developers use class based components usually if there is any complexity to
the component ("smart components"). Class components support lifecycle methods,
and can manage the state.

Functional components, also known as "Dummy components", are basic JavaScript
functions that process the input and return the component to be rendered.

The above class component could be rewritten using the code below, however
this won't have the same level of support as a class based component.

```javascript
const App = () => {
  return <h1>YouTube Clone App</h1>
}
```

### App Binding to Root Element

In our `public/index.html` file you'll see that in the body of the page there
is a DIV defined with id of "root". All of our application will be rendered
within this root division.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- ... -->
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

There is no need to modify this HTML file from this point forward. Everything
will be defined within the `src/components/` folder.

[create-react-app]: https://www.npmjs.com/package/create-react-app
[build a youtube clone application using react]: https://www.youtube.com/watch?v=VPVzx1ZOVuw
[material-ui]: https://material-ui.com/
[maerial design]: https://material.io/design/introduction/
