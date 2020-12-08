---
layout: page
title: React Hooks - Most Used Features
---

{% raw %}
Notes from [React Hooks - Most Used Features]

[react hooks - most used features]: https://www.youtube.com/watch?v=-9M9CGSd69I

- [Github repository](https://github.com/adrianhajdin/tutorial_react_hooks)

```shell
git clone git@github.com:redconfetti/react-youtube-clone.git
git checkout v1.0
```

## Current State of React

Possibilities that current React components offer us.

- Class based components
  - Smart
  - Manage state
  - Have access to lifecycle methods
- Function based components
  - Quite Stupid
  - Do nothing but return some JSX

Often you'll start with a function based component, and once you realize you
need to manage the state, or have access to lifecycle methods, you have to
refactor your component to be a Class based component.

## What are Hooks

Hooks are functions that let you "hook into" React state and lifecycle features
from function components.

React provides a few built-in Hooks like `useState` or `useEffect`. You can also
create your own Hooks to reuse stateful behavior between different components.

Hooks are just an addition. You can use them in a few components without
rewriting existing code. Class based components are here to stay.

Introduction of Hooks allows re-using of stateful logic between components. You
can write your own hooks that can be re-used between components.

## Rules of Hooks

Hooks are JavaScript functions, but they impose two additional rules:

1. Only call Hooks _at the top level_. Don't call Hooks inside loops, conditions,
   or nested functions. Hooks need to be called in the same order each time a
   component renders.
2. Only call Hooks _from React function components_. Don't call Hooks from
   regular JavaScript functions.

## FAQ

What is a Hook?

A Hook is a special function that lets you "hook into" React features. For
example, `useState` is a Hook that lets you add React state to function
components. We'll learn other React Hooks later.

When would I use a Hook?

If you write a function component and realize you need to add some state to it,
previously you had to convert it to a class. Now you can use a Hook inside the
existing function component.

## Demo Application

We're going to use dummy APIs hosted by JSON Placeholder

- [Posts](http://jsonplaceholder.typicode.com/posts)
- [Todos](http://jsonplaceholder.typicode.com/todos)

In our tutorial app, we have our primary App component.

```javascript
// src/components/App.js
import React from "react"

import ResourceList from "./ResourceList"

class App extends React.Component {
  state = {
    resourceName: "posts"
  }

  render() {
    return (
      <React.Fragment>
        <button onClick={() => this.setState({ resourceName: "posts" })}>
          Posts
        </button>
        <button onClick={() => this.setState({ resourceName: "todos" })}>
          Todos
        </button>
        <ResourceList resourceName={this.state.resourceName} />
      </React.Fragment>
    )
  }
}
export default App
```

This has a single state property called `resourceName` that gets changed when
one of the two buttons are clicked ('Posts' or 'Todos').

We're passing this `resourceName` to the `ResourceList` component.

We're using `componentDidMount()`, which is a lifecycle method supported by
React. Within this function we're making the call to the JSON Placeholder
API that corresponds to the resource ('posts' or 'todos'), and setting the
response as `resources` within the state of our component.

`componentDidMount()` is called the first time our component renders.

```javascript
// src/components/ResourceList.js

import React from "react"
import axios from "axios"

class ResourceList extends React.Component {
  state = {
    resources: []
  }

  async componentDidMount() {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/${this.props.resourceName}`
    )

    this.setState({ resources: response.data })
  }

  async componentDidUpdate(prevProps) {
    if (prevProps.resourceName !== this.props.resourceName) {
      const response = await axios.get(
        `https://jsonplaceholder.typicode.com/${this.props.resourceName}`
      )

      this.setState({ resources: response.data })
    }
  }

  render() {
    return (
      <ul>
        {this.state.resources.map(resource => (
          <li key={resource.id}>{resource.title}</li>
        ))}
      </ul>
    )
  }
}

export default ResourceList
```

We're also defining that the same thing occur via `componentDidUpdate()`, which
runs when our component is updated. It only runs the call to the remote API
if the `resourceName` in the props changed.

## Recreate App as a Functional Component

Let's recreate our App component as a functional component. We've also added
`useState` to our import statement from React.

```javascript
// src/components/App.js
import React, { useState } from "react"

import ResourceList from "./ResourceList"

const App = () => {
  const [resourceName, setResourceName] = useState("posts")

  return (
    <React.Fragment>
      <button onClick={() => this.setState({ resourceName: "posts" })}>
        Posts
      </button>
      <button onClick={() => this.setState({ resourceName: "todos" })}>
        Todos
      </button>
      <ResourceList resourceName={this.state.resourceName} />
    </React.Fragment>
  )
}

export default App
```

As you can see we've added a new statement that assigns two values returned from
the call to `useState("posts")` to `resourceName` and `setResourceName`.

`resourceName` is a variable that represents the current state of `resourceName`,
just like it did before within our `state` object.

The second parameter in the array is a function that can change the state of
`resourceName`.

We're using Array destructuring in JavaScript to assign the results of the array
to the two variables locally.

```javascript
// non-destructuring example
const arr = [1, 2]
const first = arr[0]
const second = arr[1]

// destructuring example
const arr = [1, 2]
const [first, second] = arr
// < first
// > 1
// < second
// > 2
```

Next we need to update our JSX so that it uses the new variable for our state,
as well as use the `setResourceName` function to update the state when the
buttons are clicked.

```javascript
// src/components/App.js
import React, { useState } from "react"

import ResourceList from "./ResourceList"

const App = () => {
  const [resourceName, setResourceName] = useState("posts")

  return (
    <React.Fragment>
      <button onClick={() => setResourceName("posts")}>Posts</button>
      <button onClick={() => setResourceName("todos")}>Todos</button>
      <ResourceList resourceName={resourceName} />
    </React.Fragment>
  )
}

export default App
```

## Using Hooks in ResourceList

Now let's update our ResourceList component. We're importing `useState` and
`useEffect`. As you can see we've moved our API call code into a single function
that is using `async` and `await`. The `async` keyword tells JavaScript that
a call inside the function will be asynchronous, and the `await` keyword
lets it know that the `axios.get` call will be done asynchronously. It runs
the rest of the function after the response is received, thus using our
`setResources` function to change the state of `resources`.

```javascript
// src/components/ResourceList.js
import React, { useState, useEffect } from "react"
import axios from "axios"

const ResourceList = ({ resourceName }) => {
  const [resources, setResources] = useState([])

  const fetchResources = async resourceName => {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/${resourceName}`
    )
    setResources(response.data)
  }

  return (
    <ul>
      {resources.map(resource => (
        <li key={resource.id}>{resource.title}</li>
      ))}
    </ul>
  )
}

export default ResourceList
```

Now let's apply the use of `useEffect`. This function takes in a function as
it's first argument that gets run when the component mounts or updates.

You can pass it an array as a second argument to help it support skipping
the call to the function if certain values have not changed. The values in the
array could be either props or state.

So in this case the `fetchResources` function won't be called unless the
`resourceName` has changed.

See [Hooks Effect](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)

```javascript
// src/components/ResourceList.js
import React, { useState, useEffect } from "react"
import axios from "axios"

const ResourceList = ({ resourceName }) => {
  const [resources, setResources] = useState([])

  const fetchResources = async resourceName => {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/${resourceName}`
    )
    setResources(response.data)
  }

  useEffect(() => {
    fetchResources(resourceName)
  }, [resourceName])

  return (
    <ul>
      {resources.map(resource => (
        <li key={resource.id}>{resource.title}</li>
      ))}
    </ul>
  )
}

export default ResourceList
```

## Custom Hooks

The most powerful thing that React Hooks offer are custom hooks.

You just need to define a function that starts with the word `use`. In our
case we will define `useResources`. Inside of this function we have moved
all our code for generating `resources` and `setResources`, defining
`fetchResources`, and `useEffect`.

We've also made our method take in `resourceName`, and return `resources`.
We've then added a call to `useResources` within our functional component.

```javascript
// src/components/ResourceList.js
import React, { useState, useEffect } from "react"
import axios from "axios"

const useResources = resourceName => {
  const [resources, setResources] = useState([])

  const fetchResources = async resourceName => {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/${resourceName}`
    )
    setResources(response.data)
  }

  useEffect(() => {
    fetchResources(resourceName)
  }, [resourceName])

  return resources
}

const ResourceList = ({ resourceName }) => {
  const resources = useResources(resourceName)

  return (
    <ul>
      {resources.map(resource => (
        <li key={resource.id}>{resource.title}</li>
      ))}
    </ul>
  )
}

export default ResourceList
```

If we want to take this further, we can move our custom hook to another file
and import it.

```javascript
// src/components/useResources.js
import { useState, useEffect } from "react"
import axios from "axios"

const useResources = resourceName => {
  const [resources, setResources] = useState([])

  const fetchResources = async resourceName => {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/${resourceName}`
    )
    setResources(response.data)
  }

  useEffect(() => {
    fetchResources(resourceName)
  }, [resourceName])

  return resources
}
```

Now we can greatly simplify our ResourcesList component.

```javascript
// src/components/ResourceList.js
import React from "react"
import useResources from "./useResources"

const ResourceList = ({ resourceName }) => {
  const resources = useResources(resourceName)

  return (
    <ul>
      {resources.map(resource => (
        <li key={resource.id}>{resource.title}</li>
      ))}
    </ul>
  )
}

export default ResourceList
```

Because this custom hook takes the name of the resource as it's argument,
we can re-use it in another context with a different resource name.

Note that we import `React` in our component because JSX will not be
interpretted without it. In our `useResources.js` it isn't necessary.

## Refactoring YouTube Clone Project

You can clone the
[YouTube Clone Project](https://github.com/adrianhajdin/project_youtube_video_player)
from Github.

```shell
git clone git@github.com:adrianhajdin/project_youtube_video_player.git
git checkout e7c3525
```

We're checking out commit `e7c3525` because this is where the application didn't
have any of the hooks applied yet.

### Converting App Component to Function Based

```javascript
// src/App.js
import React, { useState } from "react"
import { Grid } from "@material-ui/core"
import { SearchBar, VideoList, VideoDetail } from "./components"
import youtube from "./api/youtube"

const App = () => {
  const [videos, setVideos] = useState([])
  const [selectedVideo, setSelectedVideo] = useState(null)

  const handleSubmit = async searchTerm => {
    const response = await youtube.get("search", {
      params: {
        part: "snippet",
        maxResults: 5,
        key: "[YOUR_API_KEY]",
        q: searchTerm
      }
    })

    setVideos(response.data.items)
    setSelectedVideo(response.data.items[0])
  }

  const onVideoSelect = video => {
    this.setState({ selectedVideo: video })
  }

  return (
    <Grid style={{ justifyContent: "center" }} container spacing={10}>
      <Grid item xs={11}>
        <Grid container spacing={10}>
          <Grid item xs={12}>
            <SearchBar onFormSubmit={this.handleSubmit} />
          </Grid>
          <Grid item xs={8}>
            <VideoDetail video={selectedVideo} />
          </Grid>
          <Grid item xs={4}>
            <VideoList videos={videos} onVideoSelect={this.onVideoSelect} />
          </Grid>
        </Grid>
      </Grid>
    </Grid>
  )
}

class App extends React.Component {
  state = {
    videos: [],
    selectedVideo: null
  }

  render() {
    const { selectedVideo, videos } = this.state
  }
}

export default App
```

Left off at [32:13](https://www.youtube.com/watch?v=-9M9CGSd69I&t=32m13s)
{% endraw %}
