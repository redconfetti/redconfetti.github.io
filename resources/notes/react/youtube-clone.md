---
layout: page
title: Build a YouTube Clone Application Using React
---

Notes from [Build a YouTube Clone Application Using React]

Repository with code available at [redconfetti/react-youtube-clone]

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
npm install --save axios @material-ui/core
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
touch App.js
mkdir components
mkdir api
```

### Create Index and App

```javascript
// src/index.js
import React from "react"
import ReactDOM from "react-dom"

import App from "./App"

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
will be defined within the `src` folder moving forward.

## API Access

Under the 'src/api' folder, create a file called `youtube.js`.

This is where we're going to define our function that gets data from the
[YouTube API]. You will need a Google Account to access the API console,
from which you will obtain an API key.

You'll have to setup a new project, then choose 'Library' and search for the
YouTube Data API v3. Choose 'Enable' and proceed to setup the credential. Make
sure to choose that you'll be using it from a Web browser (JavaScript),
and that it will be accessing 'Public data'.

We're using Axios here to configure the API settings which include the key
provided by Google to access the YouTube Data API v3.

```javascript
// src/api/youtube.js
export default axios.create({
  baseURL: "https://www.googleapis.com/youtube/v3",
  params: {
    part: "snippet",
    maxResults: 5,
    key: "abcdEFGHijklmNOPQrstuvWXYZabcdEFGHdPpLk"
  }
})
```

## The Basics of our Application

Let's import the Grid component that we're going to use from Material-UI Core,
and also our YouTube API request object.

```javascript
// src/App.js
import React from "react"

import { Grid } from "@material-ui/core"
import youtube from "./api/youtube"

class App extends React.Component {
  render() {
    return <h1>YouTube Clone App</h1>
  }
}

export default App
```

Next we're going to update our App component so that it uses the Grid container.

- [Material-UI Grid](https://material-ui.com/components/grid/)

Here you see we've created our main container using the full 16 spaces. Inside
of it we've created an item using only 12 spaces. This establishes the main
area where content shows with a margin of 2 spaces on the left and right side.

Within this there is yet another container defined to represent our main space.

It has the search bar at the top using 12 spaces, with the video details and
video list items displayed beneath it.

```javascript
// src/App.js
import React from "react"

import { Grid } from "@material-ui/core"

import youtube from "./api/youtube"

class App extends React.Component {
  render() {
    return (
      <Grid justify="center" container spacing={10}>
        <Grid item xs={12}>
          <Grid container spacing={10}>
            <Grid item xs={12}>
              {/* SEARCH BAR */}
            </Grid>
            <Grid item xs={8}>
              {/* VIDEO DETAILS*/}
            </Grid>
            <Grid item xs={4}>
              {/* VIDEO LIST */}
            </Grid>
          </Grid>
        </Grid>
      </Grid>
    )
  }
}

export default App
```

Note: We're using inline CSS for this tutorial. This obviously isn't recommended for
real projects, but works for this demonstration.

## Our Components

Let's add an import statement for the components we're about to create.

```javascript
import SearchBar from "./components/SearchBar"
import VideoDetail from "./components/VideoDetail"
// import VideoList from "./components/VideoList"
```

```shell
mkdir -p src/components
touch src/components/SearchBar.js
touch src/components/VideoList.js
touch src/components/VideoDetail.js
```

### Search Bar Component

We're using a class based component because the state will be used.

```javascript
// src/components/SearchBar.js
import React from "react"

class SearchBar extends React.Component {
  state = {
    searchTerm: ""
  }
  render() {
    return <h1>This is a search bar</h1>
  }
}

export default SearchBar
```

### Video Detail Component

```javascript
// src/components/VideoDetail.js
import React from "react"

const VideoDetail = () => {
  return <h1>This is a Video Detail component</h1>
}

export default VideoDetail
```

Now that we've established the basic boilerplate for these components, let's add
them to our App.js. Because we're not putting anything within these components,
we add them using the self-closing XML syntax (`<SearchBar />`, `<VideoDetail />`).

```javascript
// src/App.js
import React from "react"

import { Grid } from "@material-ui/core"

import youtube from "./api/youtube"

class App extends React.Component {
  render() {
    return (
      <Grid justify="center" container spacing={16}>
        <Grid item xs={12}>
          <Grid container spacing={16}>
            <Grid item xs={12}>
              <SearchBar />
            </Grid>
            <Grid item xs={8}>
              <VideoDetail />
            </Grid>
            <Grid item xs={4}>
              {/* VIDEO LIST */}
            </Grid>
          </Grid>
        </Grid>
      </Grid>
    )
  }
}

export default App
```

### Creating a Component Index

If you'd like to define your components separately, but import them all at once,
you can create an `index.js` file in `src/components` that exports them
individually.

```javascript
// src/components/index.js
export { default as SearchBar } from "./SearchBar"
export { default as VideoDetail } from "./VideoDetail"
```

Now we can redefine our import in `src/App.js` like so:

```javascript
import { SearchBar, VideoDetail } from "./components"
```

## Building the Search Bar

Let's import the components we're going to use from the Material-UI library.

```javascript
// src/components/SearchBar.js
import React from "react"

import { Paper, TextField } from "@material-ui/core"

class SearchBar extends React.Component {
  state = {
    searchTerm: ""
  }

  render() {
    return (
      <Paper elevation={6} style={{ padding: "25px" }}>
        <form>
          <TextField fullWidth label="Search..."></TextField>
        </form>
      </Paper>
    )
  }
}

export default SearchBar
```

If you check in your browser, you'll have a nice long search bar at the top.

### Search Bar Event Handlers

Now we want to add an event handler to the form that is executed when the search
is submitted (`<form onSubmit={this.handleSubmit}>`).

```javascript
// src/components/SearchBar.js
import React from "react"

import { Paper, TextField } from "@material-ui/core"

class SearchBar extends React.Component {
  state = {
    searchTerm: ""
  }

  render() {
    return (
      <Paper elevation={6} style={{ padding: "25px" }}>
        <form onSubmit={this.handleSubmit}>
          <TextField fullWidth label="Search..."></TextField>
        </form>
      </Paper>
    )
  }
}

export default SearchBar
```

We can also add an 'onChange' method to the TextField. This will handle input
changes to the text field.

```javascript
// src/components/SearchBar.js
import React from "react"

import { Paper, TextField } from "@material-ui/core"

class SearchBar extends React.Component {
  state = {
    searchTerm: ""
  }

  render() {
    return (
      <Paper elevation={6} style={{ padding: "25px" }}>
        <form onSubmit={this.handleSubmit}>
          <TextField
            fullWidth
            label="Search..."
            onChange={this.handleChange}
          ></TextField>
        </form>
      </Paper>
    )
  }
}

export default SearchBar
```

#### Binding our Event Handling Functions

Within the [React Docs for Handling Events] the example shows a function
declared within the class as per the normal method.

When a normal function is declared like this, the function has it's own scope
where `this` refers to the function itself. This is why there is a call to bind
the class to `this` within the constructor.

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props)
    this.state = { isToggleOn: true }

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this)
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }))
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? "ON" : "OFF"}
      </button>
    )
  }
}
```

There is a simple work-around to this issue. You can simply declare the function
using an arrow-function, as they do not have their own `this` defined in their
scope.

```javascript
handleChange = event => {
  this.setState({
    searchTerm: event.target.value
  })
}
```

We can also write this in a single line.

```javascript
handleChange = event => this.setState({ searchTerm: event.target.value })
```

Now we can also add our `handleSubmit` function also. As you can see this
makes use of the [destructuring assignment syntax] added by ES6 to define
a constant called `searchTerm` from the `searchTerm` property of `this.state`.

```javascript
// src/components/SearchBar.js
import React from "react"

import { Paper, TextField } from "@material-ui/core"

class SearchBar extends React.Component {
  state = {
    searchTerm: ""
  }

  handleChange = event => this.setState({ searchTerm: event.target.value })

  handleSubmit = () => {
    const { searchTerm } = this.state
  }

  render() {
    return (
      <Paper elevation={6} style={{ padding: "25px" }}>
        <form onSubmit={this.handleSubmit}>
          <TextField
            fullWidth
            label="Search..."
            onChange={this.handleChange}
          ></TextField>
        </form>
      </Paper>
    )
  }
}

export default SearchBar
```

To make the searchTerm string available to other components, we need to pass in
a function via a prop called `onFormSubmit`.

```javascript
// src/App.js

// ...
<SearchBar onFormSubmit={this.handleSubmit} />
// ...
```

Within our SearchBar components we can then update our `handleSubmit` function
so that it is able to call this function.

```javascript
// src/components/SearchBar.js
import React from "react"

import { Paper, TextField } from "@material-ui/core"

class SearchBar extends React.Component {
  state = {
    searchTerm: ""
  }

  handleChange = event => this.setState({ searchTerm: event.target.value })

  handleSubmit = event => {
    const { searchTerm } = this.state
    const { onFormSubmit } = this.props

    onFormSubmit(searchTerm)

    event.preventDefault()
  }

  render() {
    return (
      <Paper elevation={6} style={{ padding: "25px" }}>
        <form onSubmit={this.handleSubmit}>
          <TextField
            fullWidth
            label="Search..."
            onChange={this.handleChange}
          ></TextField>
        </form>
      </Paper>
    )
  }
}

export default SearchBar
```

So now the SearchBar component will run the method we inject into it and pass
it the `searchTerm`.

We've also updated the `handleSubmit` function so that it receives the event
and makes a call to `event.preventDefault()` to stop the form submit from
refreshing the page.

Next we're going to define our `handleSubmit` method that we're passing into
the SearchBar component within our `App.js`.

As you can see we're using the `async` keyword before our function, and the
`await` keyword before the call to the YouTube API call.

This is a new feature of ES2017 that makes it possible to make asynchronous
calls using a standard synchronous functional definition instead of having to
rely on promise chain.

- [MDN web docs - async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [How to use Async Await in JavaScript](https://medium.com/javascript-in-plain-english/async-await-javascript-5038668ec6eb)

```javascript
// src/App.js
import React from "react"

import { Grid } from "@material-ui/core"

import { SearchBar, VideoDetail } from "./components"

import youtube from "./api/youtube"

class App extends React.Component {
  handleSubmit = async searchTerm => {
    const response = await youtube.get("search", {
      params: {
        q: searchTerm
      }
    })

    console.log(response)
  }

  render() {
    return (
      <Grid justify="center" container spacing={10}>
        <Grid item xs={12}>
          <Grid container spacing={10}>
            <Grid item xs={12}>
              <SearchBar onFormSubmit={this.handleSubmit} />
            </Grid>
            <Grid item xs={8}>
              <VideoDetail />
            </Grid>
            <Grid item xs={4}>
              {/* VIDEO LIST */}
            </Grid>
          </Grid>
        </Grid>
      </Grid>
    )
  }
}

export default App
```

#### Fixing Axios Call

It turns out that this doesn't work, we end up getting an HTTP 400 error from
the YouTube API because Axios is not passing our default parameters.

First let's cut those `params` from our youtube.js file.

```javascript
// src/api/youtube.js
import axios from "axios"

export default axios.create({
  baseURL: "https://www.googleapis.com/youtube/v3"
})
```

And then paste them into the call from `src/App.js`.

```javascript
handleSubmit = async searchTerm => {
  const response = await youtube.get("search", {
    params: {
      part: "snippet",
      maxResults: 5,
      key: "[Api Key]",
      q: searchTerm
    }
  })

  console.log(response)
}
```

If you look at the console you'll see that the 'data' object in the API response
contains the 'items' returned by the search. We can narrow down our console
log statement to this.

```javascript
handleSubmit = async searchTerm => {
  const response = await youtube.get("search", {
    params: {
      part: "snippet",
      maxResults: 5,
      key: "[Api Key]",
      q: searchTerm
    }
  })

  console.log(response.data.items)
}
```

Now we have all the data we need to create our YouTube video list.

## Displaying Search Results

### Adding results to state

Within our App.js, we can now establish the definition of the default state
within our App class, and we can also use `this.setState()` within our
`handleSubmit` function so that it sets the 'videos' to equal the YouTube
search results we obtained, and it sets the 'selectedVideo' to the first video
in the search results collection.

```javascript
// src/App.js
// ...
class App extends React.Component {
  state = {
    videos: [],
    selectedVideo: null
  }

  handleSubmit = async searchTerm => {
    const response = await youtube.get("search", {
      params: {
        part: "snippet",
        maxResults: 5,
        key: "[App Key]",
        q: searchTerm
      }
    })

    this.setState({
      videos: response.data.items,
      selectedVideo: response.data.items[0]
    })
  }

  // render (){ ... }
}
// ...
```

### Populating Video Detail

Now we can pass the `selectedVideo` information to the `VideoDetail` component.

```javascript
// src/App.js

// ...
class App extends React.Component {
  // state = ...
  // handleSubmit = ...

  render() {
    const { selectedVideo } = this.state
    return (
      <Grid justify="center" container spacing={10}>
        <Grid item xs={12}>
          <Grid container spacing={10}>
            <Grid item xs={12}>
              <SearchBar onFormSubmit={this.handleSubmit} />
            </Grid>
            <Grid item xs={8}>
              <VideoDetail video={selectedVideo} />
            </Grid>
            <Grid item xs={4}>
              {/* VIDEO LIST */}
            </Grid>
          </Grid>
        </Grid>
      </Grid>
    )
  }
}

// ...
```

Now we can open the `VideoDetail` component and build it out to use the object
we're passing via the props.

First off we added an import of `Paper` and `Typography` from the Material-UI
library. With a function based components the props are passed as the first
argument to the function, so we're using variable destructuring to bring in
only the 'video' property from the props passed in.

We're using a `React.Fragment` wrapper to wrap the two Paper component.

```javascript
// src/components/VideoDetail.js
import React from "react"

import { Paper, Typography } from "@material-ui/core"

const VideoDetail = ({ video }) => {
  return (
    <React.Fragment>
      <Paper elevation={6} style={{ height: "70%" }}></Paper>
      <Paper elevation={6} style={{ padding: "15px" }}></Paper>
    </React.Fragment>
  )
}

export default VideoDetail
```

Within the first Paper element, we're going to place an iframe that will display
the video.

Note that the `videoSrc` constant we define uses backticks around the string
so that the interpolation of the `videoId` is supported.

```javascript
// src/components/VideoDetail.js
import React from "react"

import { Paper, Typography } from "@material-ui/core"

const VideoDetail = ({ video }) => {
  if (!video) return <div>Loading...</div>
  const videoSrc = `https://www.youtube.com/embed/${video.id.videoId}`
  return (
    <React.Fragment>
      <Paper elevation={6} style={{ height: "70%" }}>
        <iframe
          frameBorder="0"
          height="100%"
          width="100%"
          title="Video Player"
          src={videoSrc}
        />
      </Paper>
      <Paper elevation={6} style={{ padding: "15px" }}></Paper>
    </React.Fragment>
  )
}

export default VideoDetail
```

Now if you go check the app by doing a search, the video will load.

### Video Text

Below the video we want to display the information about the video. The
Typography component can support paragraphs, headers, etc.

```javascript
// src/components/VideoDetail.js
import React from "react"

import { Paper, Typography } from "@material-ui/core"

const VideoDetail = ({ video }) => {
  if (!video) return <div>Loading...</div>
  const videoSrc = `https://www.youtube.com/embed/${video.id.videoId}`
  return (
    <React.Fragment>
      <Paper elevation={6} style={{ height: "70%" }}>
        <iframe
          frameBorder="0"
          height="100%"
          width="100%"
          title="Video Player"
          src={videoSrc}
        />
      </Paper>
      <Paper elevation={6} style={{ padding: "15px" }}>
        <Typography variant="h4">
          {video.snippet.title} - {video.snippet.channelTitle}
        </Typography>
        <Typography variant="subtitle1">
          {video.snippet.channelTitle}
        </Typography>
        <Typography variant="subtitle2">{video.snippet.description}</Typography>
      </Paper>
    </React.Fragment>
  )
}

export default VideoDetail
```

There we have our video detail component.

Left off at [55:15](https://www.youtube.com/watch?v=VPVzx1ZOVuw&t=55m15s)

[destructuring assignment syntax]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
[react docs for handling events]: https://reactjs.org/docs/handling-events.html
[youtube api]: https://developers.google.com/youtube/v3/getting-started
[create-react-app]: https://www.npmjs.com/package/create-react-app
[build a youtube clone application using react]: https://www.youtube.com/watch?v=VPVzx1ZOVuw
[material-ui]: https://material-ui.com/
[maerial design]: https://material.io/design/introduction/
[redconfetti/react-youtube-clone]: https://github.com/redconfetti/react-youtube-clone
