---
layout: page
title: Redux
---

Taken from [Lynda - How Redux Works](https://www.lynda.com/React-js-tutorials/How-Redux-works/540345/569712-4.html)

# What is Redux

## The History of Redux

Dan Abramov invented the idea for Redux during a React Europe Conference
presentation in 2015. Andrew Clark abandoned Flummox, another Flux
implementation, to work with Abramov to complete Redux.

## Flux

A design pattern developed by Facebook. An alternative to MVC, MVP, or MVVM.

Models manage the data within an application. Models are presented in Views.
Models can feed data to multiple views. When a user interacts with a view,
the model may become modified. This can change the data in other views. This
can have unexpected consequences in large complex systems.

Flux was developed by Facebook, a pattern where data flows in one direction.

Action -> Dispatcher -> Store -> View

Flux is a design pattern, not a library. Libraries that apply this design
pattern include Reflux, Flummox, Fluxxor, Alt, Redux, Marty.js, McFly, DeLorean,
Lux, Fluxy, and Material Flux.

Due to simplicity and ease of use, Redux has won out in the community.

## How Redux Works

Redux isn't exactly Flux, it's Flux-like. Data still flows in one direction,
but there is only one store (not multiple). The "single source of truth".

Moularity is achieved by using functions to manage specific leafs and branches
of the state tree.

Using functions for modularity comes from The Functional Programming paradigm.

## Functional Programming

- Pure functions - Do not cause side affects. Receive input, and return result.
  Do not modify arguments, global variables, or other state.
- Immutability - No variables are changed, instead new ones are created.
- Composition - Ability to put functions together in a way that one
  functions output becomes the next functions input.

### Example

Let's say we want to make a call to `getPercent(1,4)` and have it return
the string '25%'.

- getPercent(1,4)
  - convertToDecimal() - returns `0.25`
  - decimalToPercent() - returns '25'
  - addPercentSign() - returns '25%'

```javascript
import { compose } from "redux"

const getPercent = compose(
  addPercentSign,
  decimalToPercent,
  convertToDecimal
)

getPercent(1, 4)
```

In Redux composition is used in the store. The reducer functions that we create
to manage parts of the state tree are composed. The action and state is piped
through each of these reducers until a state is eventually mutated.

## Plan a Redux App

### Actions

In a Redux application, you want to define your actions.

- ADD_DAY
- REMOVE_DAY
- SET_GOAL
- ADD_ERROR
- CLEAR_ERROR
- FETCH_RESORT_NAMES
- CANCEL_FETCHING
- CHANGE_SUGGESTIONS
- CLEAR_SUGGESTIONS

We want to put these in a file called constants.

```javascript
// src/constants.js
const constants = {
  ADD_DAY: "ADD_DAY",
  REMOVE_DAY: "REMOVE_DAY",
  SET_GOAL: "SET_GOAL",
  ADD_ERROR: "ADD_ERROR",
  CLEAR_ERROR: "CLEAR_ERROR",
  FETCH_RESORT_NAMES: "FETCH_RESORT_NAMES",
  CANCEL_FETCHING: "CANCEL_FETCHING",
  CHANGE_SUGGESTIONS: "CHANGE_SUGGESTIONS",
  CLEAR_SUGGESTIONS: "CLEAR_SUGGESTIONS"
}

export default constants
```

This is done to make sure that any typos result in an error when working with
these strings that represent the different actions.

### State

- `allSkiDays -> []`
- `skiDay -> {resort, date, powder, backcountry}`
- `goal -> number`
- `errors -> []`
- `resortNames.fetching -> boolean`
- `resortNames.suggestions -> []`

```javascript
// initialState.json
{
  "allSkiDays": [
    {
      "resort": "Kirkwood",
      "date": "2016-12-7",
      "powder": true,
      "backcountry": false
    },
    {
      "resort": "Squaw Valley",
      "date": "2016-12-8",
      "powder": false,
      "backcountry": false
    },
    {
      "resort": "Mt Tallac",
      "date": "2016-12-9",
      "powder": false,
      "backcountry": true
    }
  ],
  "goal": 10,
  "errors": [],
  "resortNames": {
    "fetching": false,
    "suggestions": ["SquawValley","Snowbird","Stowe","Steamboat"]
  }
}
```

### Reducers

We will name the reducer the same thing as the key.

# Understanding Reducers

## Run Redux with babel-node

```shell
npm init
npm install babel-cli --save-dev
npm install babel-preset-latest --save-dev
npm install babel-preset-stage-0 --save-dev

mkdir -p src
mkdir -p src/store
touch .babelrc
touch src/index.js
touch src/constants.js
touch src/initialState.json
touch src/store/reducers.js
```

```javascript
// .babelrc
{
  "presets": ["latest", "stage-0"]
}
```

`./src/index.js` will automatically get run with the 'npm start' command.

```javascript
// package.json
{
  "name": "ski-day-counter",
  "version": "1.0.0",
  "description": "",
  "main": "constants.js",
  "scripts": {
    "start": "./node_modules/.bin/babel-node ./src/"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-latest": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1"
  }
}
```

```javascript
// src/index.js
import C from "./constants"
import { allSkiDays, goal } from "./initialState.json"

console.log(`

  Ski Day Counter
  ================
  The goal is ${goal} days
  Initially there are ${allSkiDays.length} ski days in state

  Constants (actions)
  -------------------
  ${Object.keys(C).join("\n     ")}

`)
```

## Build Your First Reducer

Reducers are pure functions that are designed to manage specific part of your
state object.

```javascript
// src/store/reducers.js
import C from "../constants"

export const goal = (state = 10, action) => {
  if (action.type === C.SET_GOAL) {
    return parseInt(action.payload)
  } else {
    return state
  }
}
```

```javascript
// src/index.js
import C from "./constants"
import { goal } from "./store/reducers"

const state = 10
const action = {
  type: C.SET_GOAL,
  payload: 15
}

const nextState = goal(state, action)

console.log(`

  initial goal: ${state}
  action: ${JSON.stringify(action)}
  new goal: ${nextState}

`)
```

```
   initial goal: 10
   action: {"type":"SET_GOAL", "payload":15}
   new goal: 15
```

## Create object reducers

```javascript
// src/index.js
import C from "./constants"
import { skiDay } from "./store/reducers"

const state = null
const action = {
  type: C.ADD_DAY,
  payload: {
    resort: "Heavenly",
    date: "2016-12-16",
    powder: true,
    backcountry: false
  }
}

const nextState = skiDay(state, action)

console.log(`

  initial state: ${state}
  action: ${JSON.stringify(action)}
  new State: ${JSON.stringify(nextState)}

`)
```

```javascript
// src/store/reducers.js
import C from "../constants"

export const goal = (state = 10, action) => {
  if (action.type === C.SET_GOAL) {
    return parseInt(action.payload)
  } else {
    return state
  }
}

export const skiDay = (state = null, action) => {
  if (action.type === C.ADD_DAY) {
    return action.payload
  } else {
    return state
  }
}
```

_Output_

```
  initial state: null
  action: {"type":"ADD_DAY", "payload":{"resort":"Heavenly","date":"2016-12-16","powder":true,"backcountry":false}}
  new state: {"resort":"Heavenly","date":"2016-12-16","powder":true,"backcountry":false}
```

### Refactor for oneline conditionals

```javascript
// src/store/reducers.js
import C from "../constants"

export const goal = (state = 10, action) =>
  action.type === C.SET_GOAL ? parseInt(action.payload) : state

export const skiDay = (state = null, action) =>
  action.type === C.ADD_DAY ? action.payload : state
```

## Create Array Reducers

### Adding Errors

```javascript
// src/index.js
import C from "./constants"
import { errors } from "./store/reducers"

const state = ["user not authorized", "server feed not found"]
const action = {
  type: C.ADD_ERROR,
  payload: "cannot connect to server"
}

const nextState = errors(state, action)

console.log(`

  initial state: ${state}
  action: ${JSON.stringify(action)}
  new State: ${JSON.stringify(nextState)}

`)
```

```javascript
// src/store/reducers.js
import C from "../constants"

export const goal = (state = 10, action) => {
  if (action.type === C.SET_GOAL) {
    return parseInt(action.payload)
  } else {
    return state
  }
}

export const skiDay = (state = null, action) => {
  if (action.type === C.ADD_DAY) {
    return action.payload
  } else {
    return state
  }
}

export const error = (state = [], action) => {
  switch (action.type) {
    case C.ADD_ERROR:
      // we don't want to mutate the actual state, we need to return a new object
      // state.push(action.payload)
      return [...state, action.payload]
    default:
      return state
  }
}
```

_Output_

```
  initial state: user not authorized, server feed not found
  action: {"type":"ADD_ERROR", "payload":"cannot connect to server"}
  new state: ["user not authorized","server feed not found","cannot connect to server"]
```

### Clearing Errors

```javascript
// src/index.js
import C from "./constants"
import { errors } from "./store/reducers"

const state = ["user not authorized", "server feed not found"]
const action = {
  type: C.CLEAR_ERROR,
  payload: 0
}

const nextState = errors(state, action)

console.log(`

  initial state: ${state}
  action: ${JSON.stringify(action)}
  new State: ${JSON.stringify(nextState)}

`)
```

```javascript
// src/store/reducers.js
import C from "../constants"

// ...

export const error = (state = [], action) => {
  switch (action.type) {
    case C.ADD_ERROR:
      // we don't want to mutate the actual state, we need to return a new object
      // state.push(action.payload)
      return [...state, action.payload]
    case C.CLEAR_ERROR:
      return state.filter((message, i) => i !== action.payload)
    default:
      return state
  }
}
```

_Output_

```
  initial state: user not authorized, server feed not found
  action: {"type":"CLEAR_ERROR", "payload":0}
  new state: ["server feed not found"]
```

## Composing Reducers

### Adding a Day

```javascript
// src/index.js
import C from "./constants"
import { allSkiDays } from "./store/reducers"

const state = [
  {
    resort: "Kirkwood",
    date: "2016-12-15",
    powder: true,
    backcountry: false
  }
]
const action = {
  type: C.ADD_DAY,
  payload: {
    resort: "Boreal",
    date: "2016-12-16",
    powder: false,
    backcountry: false
  }
}

const nextState = allSkiDays(state, action)

console.log(`

  initial state: ${JSON.stringify(state)}
  action: ${JSON.stringify(action)}
  new State: ${JSON.stringify(nextState)}

`)
```

```javascript
// src/store/reducers.js
import C from "../constants"

export const skiDay = (state = null, action) =>
  action.type === C.ADD_DAY ? action.payload : state

// ...

export const allSkiDays = (state = [], action) => {
  switch (action.type) {
    case C.ADD_DAY:
      return [...state, skiDay(null, action)]
    default:
      state
  }
}
```

_Output_

```
  initial state: [{"resort":"Kirkwood","date":"2016-12-15","powder":true,"backcountry":false}]
  action: {"type":"ADD_DAY","payload":{"resort":"Boreal","date":"2016-12-16","powder":false,"backcountry":false}}
  new State: [
    {"resort":"Kirkwood","date":"2016-12-15","powder":true,"backcountry":false},
    {"resort":"Boreal","date":"2016-12-16","powder":false,"backcountry":false}
  ]
```

### Avoiding a Duplicate Day

```javascript
// src/store/reducers.js
import C from "../constants"

export const skiDay = (state = null, action) =>
  action.type === C.ADD_DAY ? action.payload : state

// ...

export const allSkiDays = (state = [], action) => {
  switch (action.type) {
    case C.ADD_DAY:
      const hasDay = state.some(skiDay => skiDay.date === action.payload.date)
      return hasDay ? state : [...state, skiDay(null, action)]
    default:
      state
  }
}
```

_Output_

```
  initial state: [{"resort":"Kirkwood","date":"2016-12-15","powder":true,"backcountry":false}]
  action: {"type":"ADD_DAY","payload":{"resort":"Boreal","date":"2016-12-16","powder":false,"backcountry":false}}
  new State: [{"resort":"Kirkwood","date":"2016-12-15","powder":true,"backcountry":false},{"resort":"Boreal","date":"2016-12-16","powder":false,"backcountry":false}]
```

### Removing a Day

```javascript
// src/index.js
import C from "./constants"
import { allSkiDays } from "./store/reducers"

const state = [
  {
    resort: "Kirkwood",
    date: "2016-12-15",
    powder: true,
    backcountry: false
  },
  {
    resort: "Boreal",
    date: "2016-12-16",
    powder: false,
    backcountry: false
  }
]
const action = {
  type: C.REMOVE_DAY,
  payload: "2016-12-15"
}

const nextState = allSkiDays(state, action)

console.log(`

  initial state: ${JSON.stringify(state)}
  action: ${JSON.stringify(action)}
  new State: ${JSON.stringify(nextState)}

`)
```

```javascript
// src/store/reducers.js
import C from "../constants"

export const skiDay = (state = null, action) =>
  action.type === C.ADD_DAY ? action.payload : state

// ...

export const allSkiDays = (state = [], action) => {
  switch (action.type) {
    case C.ADD_DAY:
      return [...state, skiDay(null, action)]
    case C.REMOVE_DAY:
      return state.filter(skiDay => skiDay.date !== action.payload)
    default:
      return state
  }
}
```

_Output_

```
  initial state: [{"resort":"Kirkwood","date":"2016-12-15","powder":true,"backcountry":false},{"resort":"Boreal","date":"2016-12-16","powder":false,"backcountry":false}]
  action: {"type":"REMOVE_DAY","payload":"2016-12-15"}
  new State: [{"resort":"Boreal","date":"2016-12-16","powder":false,"backcountry":false}]
```

## Combine Reducers

We're going to make use of a method called `combineReducers` provided by Redux.

```javascript
import C from "../constants"
import { combineReducers } from "redux"

export const goal = (state = 10, action) =>
  action.type === C.SET_GOAL ? parseInt(action.payload) : state

export const skiDay = (state = null, action) =>
  action.type === C.ADD_DAY ? action.payload : state

export const errors = (state = [], action) => {
  switch (action.type) {
    case C.ADD_ERROR:
      return [...state, action.payload]
    case C.CLEAR_ERROR:
      return state.filter((message, i) => i !== action.payload)
    default:
      return state
  }
}

export const allSkiDays = (state = [], action) => {
  switch (action.type) {
    case C.ADD_DAY:
      const hasDay = state.some(skiDay => skiDay.date === action.payload.date)
      return hasDay
        ? state
        : [...state, skiDay(null, action)].sort(
            (a, b) => new Date(b.date) - new Date(a.date)
          )
    case C.REMOVE_DAY:
      return state.filter(skiDay => skiDay.date !== action.payload)
    default:
      return state
  }
}

export const fetching = (state = false, action) => {
  switch (action.type) {
    case C.FETCH_RESORT_NAMES:
      return true
    case C.CANCEL_FETCHING:
      return false
    case C.CHANGE_SUGGESTIONS:
      return false
    default:
      return state
  }
}

export const suggestions = (state = [], action) => {
  switch (action.type) {
    case C.CLEAR_SUGGESTIONS:
      return []
    case C.CHANGE_SUGGESTIONS:
      return action.payload
    default:
      return state
  }
}

const resortNames = combineReducers({
  fetching,
  suggestions
})

const singleReducer = combineReducers({
  allSkiDays,
  goal,
  errors,
  resortNames
})

export default singleReducer
```

We can use less code to accomplish the same thing like so:

```javascript
import C from "../constants"
import { combineReducers } from "redux"

// ...

export default combineReducers({
  allSkiDays,
  goal,
  errors,
  resortNames: combineReducers({
    fetching,
    suggestions
  })
})
```

Now let's test this out in our `index.js`

```javascript
// index.js
import C from "./constants"
import appReducer from "./store/reducers"
import initialState from "./initialState.json"

let state = initialState

console.log(`

  Initial State
  ==============
  goal: ${state.goal}
  resorts: ${JSON.stringify(state.allSkiDays)}
  fetching: ${state.resortNames.fetching}
  suggestions: ${state.resortNames.suggestions}

`)

state = appReducer(state, {
  type: C.SET_GOAL,
  payload: 2
})

state = appReducer(state, {
  type: C.ADD_DAY,
  payload: {
    resort: "Mt Shasta",
    date: "2016-10-28",
    powder: false,
    backcountry: true
  }
})

state = appReducer(state, {
  type: C.CHANGE_SUGGESTIONS,
  payload: ["Mt Tallac", "Mt Hood", "Mt Shasta"]
})

console.log(`

  Next State
  ==============
  goal: ${state.goal}
  resorts: ${JSON.stringify(state.allSkiDays)}
  fetching: ${state.resortNames.fetching}
  suggestions: ${state.resortNames.suggestions}

`)
```

_Output_

```
  Initial State
  ==============
  goal: 10
  resorts: [{"resort":"Kirkwood","date":"2016-12-7","powder":true,"backcountry":false},{"resort":"Squaw Valley","date":"2016-12-8","powder":false,"backcountry":false},{"resort":"Mt Tallac","date":"2016-12-9","powder":false,"backcountry":true}]
  fetching: false
  suggestions: Squaw Valley,Snowbird,Stowe,Steamboat


  Next State
  ==============
  goal: 2
  resorts: [{"resort":"Mt Tallac","date":"2016-12-9","powder":false,"backcountry":true},{"resort":"Squaw Valley","date":"2016-12-8","powder":false,"backcountry":false},{"resort":"Kirkwood","date":"2016-12-7","powder":true,"backcountry":false},{"resort":"Mt Shasta","date":"2016-10-28","powder":false,"backcountry":true}]
  fetching: false
  suggestions: Mt Tallac,Mt Hood,Mt Shasta
```

# The Store

## Create a static build with webpack

We need to install webpack and the webpack dev server

```bash
npm install webpack --save-dev
npm install webpack-dev-server --save-dev
```

We need to use loaders, which are the instructions that webpack follows
when transpiling our code and creating the bundle.

We need to install the Babel loader that converts our ES6 into ES5
compatible JavaScript.

```shell
npm install babel-loader --save-dev
npm install babel-core --save-dev
npm install json-loader --save-dev
```

We need to create a webpack configuration file - `webpack.config.js`.

```javascript
// webpack.config.js
module.exports = {
  entry: "./src/index.js"
}
```

This tells Webpack which file to start with to perform the
bundling on.

We have an HTML file under `dist/index.html`. This is the file
which the browser will run.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta
      name="viewport"
      content="minimum-scale=1.0, width=device-width, maximum-scale=1.0, user-scalable=no"
    />
    <meta charset="utf-8" />
    <title>Ski Day Counter</title>
  </head>
  <body>
    <div id="react-container"></div>
    <script src="assets/bundle.js"></script>
  </body>
</html>
```

As you can see it references `assets/bundle.js`, which is the file
we want Webpack to bundle our Javascript into.

We can specify this in our webpack configuration.

```javascript
// webpack.config.js
module.exports = {
  entry: "./src/index.js",
  output: {
    path: "dist/assets",
    filename: "bundle.js",
    publicPath: "assets"
  }
}
```

Next we can configure how the Webpack-Dev should operate.

```javascript
// webpack.config.js
module.exports = {
  entry: "./src/index.js",
  output: {
    path: "dist/assets",
    filename: "bundle.js",
    publicPath: "assets"
  },
  devServer: {
    inline: true,
    contentBase: "./dist",
    port: 3000
  }
}
```

The [inline mode](https://webpack.js.org/configuration/dev-server/#devserverinline)
causes a script to be inserted in the bundle to take care of live reloading.
Build messages will appears in the browser console.

There is also an iframe mode, where the page is iframed under a notification bar
with messages about the build.

Next we can configure Webpack to use the Babel loader.

```javascript
// webpack.config.js
module.exports = {
  entry: "./src/index.js",
  output: {
    path: "dist/assets",
    filename: "bundle.js",
    publicPath: "assets"
  },
  devServer: {
    inline: true,
    contentBase: "./dist",
    port: 3000
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /(node_modules)/,
        loader: ["babel"],
        query: {
          presets: ["latest", "stage-0"]
        }
      }
    ]
  }
}
```

If we import a module that has any ES6 or other emerging JavaScript syntax,
it will be included in the bundle.js as ES5 compatible JavaScript. We
want to run the Babel loader on any file that ends in `.js`. This is what
the 'test' regular expression does. We're also choosing to exclude
anything loaded from the 'node_modules' folder.

We also originally setup presets for our Babel-node command.
We want to make sure we include the same presets for Babel
in our Webpack config.

```javascript
// .babelrc
{
  "presets": ["latest", "stage-0"]
}
```

Note: [Stage presents are being deprecated](https://babeljs.io/blog/2018/07/27/removing-babels-stage-presets)
with Babel v7.

Lastly, we need to add a loader for including JSON files in our bundle.

```javascript
// webpack.config.js
module.exports = {
  entry: "./src/index.js",
  output: {
    path: "dist/assets",
    filename: "bundle.js",
    publicPath: "assets"
  },
  devServer: {
    inline: true,
    contentBase: "./dist",
    port: 3000
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /(node_modules)/,
        loader: ["babel"],
        query: {
          presets: ["latest", "stage-0"]
        }
      },
      {
        test: /\.json$/,
        exclude: /(node_modules)/,
        loader: "json-loader"
      }
    ]
  }
}
```

In our `package.json` you can see that all our dependencies have been
put under 'devDependencies'. You'll remember that we configured
the default script for `npm start` was to use 'babel-node' to run
our app.

```javascript
// package.json
{
  "name": "ski-day-counter",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "babel-node ./src"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "redux": "^3.6.0"
  },
  "devDependencies": {
    "babel-core": "^6.18.0",
    "babel-loader": "^6.2.6",
    "babel-preset-latest": "^6.16.0",
    "babel-preset-stage-0": "^6.16.0",
    "json-loader": "^0.5.4",
    "webpack": "^1.13.3",
    "webpack-dev-server": "^1.16.2"
  }
}
```

Instead we're going to change this to use the Webpack-Dev-Server instead.

```javascript
  "scripts": {
    "start": "./node_modules/.bin/webpack-dev-server"
  },
```

All executables installed by NPM are placed in `./node_modules/.bin`.
Webpack-Dev-Server will automatically start the Express server for us on port 3000.

```shell
$ npm start

> ski-day-counter@1.0.0 start /Users/jasonmiller/Projects/redux/exercises/Ch03/03_01/start
> webpack-dev-server

 http://localhost:3000/
webpack result is served from /assets
content is served from ./dist
Hash: 2afa1e19c1068e8225ac
Version: webpack 1.15.0
Time: 782ms
    Asset    Size  Chunks             Chunk Names
bundle.js  286 kB       0  [emitted]  main
chunk    {0} bundle.js (main) 265 kB [rendered]
    [0] multi main 40 bytes {0} [built]
    [1] (webpack)-dev-server/client?http://localhost:3000 4.16 kB {0} [built]
...
...
   [96] ./~/redux/lib/compose.js 927 bytes {0} [built]
   [97] ./src/initialState.json 381 bytes {0} [built]
webpack: Compiled successfully.
```

## Create a store

We've combined all our reducers into a single appReducer. With Redux
we don't have to use this because the store will do this for us.

The 'createStore' function provided by Redux is used to build
instance of Redux stores.

```javascript
// src/index.js
import C from "./constants"
import appReducer from "./store/reducers"
import initialState from "./initialState.json"
import { createStore } from "redux"

const store = createStore(appReducer)

console.log("initial state", store.getState())
```

By default, just using the appReducer, our initial state will
be created by using all of the default variables we defined
in every reducer. For instance our goal value defaults to '10'
and our allSkiDays was set to an empty array.

Once every reducer is invoked once, the default value for that
reducer will be stored as the initial state.

The store also provides the `dispatch` method used to dispatch
actions that mutate the state.

```javascript
// src/index.js
import C from "./constants"
import appReducer from "./store/reducers"
import initialState from "./initialState.json"
import { createStore } from "redux"

const store = createStore(appReducer)

console.log("initial state", store.getState())

store.dispatch({
  type: C.ADD_DAY,
  payload: {
    resort: "Mt Shasta",
    date: "2016-10-28",
    powder: false,
    backcountry: true
  }
})

console.log("next state", store.getState())
```

Now we run our server, access our browser via http://localhost:3000/, and we look at the console.

```
$ npm start
```

The `createStore` method will also accept an object to use for initialState.

```javascript
const store = createStore(appReducer, initialState)
```

After making this modification to `index.js` and saving the file,
our Webpack-Dev-Server will reload the page and we'll see the new outcome.

_Console Output_

```
initial state
  {allSkiDays: Array(3), goal: 10, errors: Array(0), resortNames: {…}}
    allSkiDays: Array(3)
      0: {resort: "Kirkwood", date: "2016-12-7", powder: true, backcountry: false}
      1: {resort: "Squaw Valley", date: "2016-12-8", powder: false, backcountry: false}
      2: {resort: "Mt Tallac", date: "2016-12-9", powder: false, backcountry: true}
      length: 3
      __proto__: Array(0)
    errors: []
    goal: 10
    resortNames: {fetching: false, suggestions: Array(4)}
    __proto__: Object

next state
  {allSkiDays: Array(4), goal: 10, errors: Array(0), resortNames: {…}}
    allSkiDays: Array(4)
      0: {resort: "Mt Tallac", date: "2016-12-9", powder: false, backcountry: true}
      1: {resort: "Squaw Valley", date: "2016-12-8", powder: false, backcountry: false}
      2: {resort: "Kirkwood", date: "2016-12-7", powder: true, backcountry: false}
      3: {resort: "Mt Shasta", date: "2016-10-28", powder: false, backcountry: true}
      length: 4
      __proto__: Array(0)
    errors: []
    goal: 10
    resortNames: {fetching: false, suggestions: Array(4)}
    __proto__: Object
```

## Subscribe to the store

It's possible to subscribe to the store so that your callback methods are
called anytime the state changes.

```javascript
import C from "./constants"
import appReducer from "./store/reducers"
import { createStore } from "redux"

const store = createStore(appReducer)

store.subscribe(() => console.log(store.getState()))

store.dispatch({
  type: C.ADD_DAY,
  payload: {
    resort: "Mt Shasta",
    date: "2016-10-28",
    powder: false,
    backcountry: true
  }
})

store.dispatch({
  type: C.SET_GOAL,
  payload: 2
})
```

Console Output:

```
{allSkiDays: Array(1), goal: 10, errors: Array(0), resortNames: {…}}
  allSkiDays: Array(1)
    0: {resort: "Mt Shasta", date: "2016-10-28", powder: false, backcountry: true}
    length: 1
    __proto__: Array(0)
  errors: []
  goal: 10
  resortNames: {fetching: false, suggestions: Array(0)}
  __proto__: Object

{allSkiDays: Array(1), goal: 2, errors: Array(0), resortNames: {…}}
  allSkiDays: Array(1)
    0: {resort: "Mt Shasta", date: "2016-10-28", powder: false, backcountry: true}
    length: 1
    __proto__: Array(0)
  errors: []
  goal: 2
  resortNames: {fetching: false, suggestions: Array(0)}
  __proto__: Object
```

We can even use a subscriber to store data to local storage.

```javascript
store.subscribe(() => {
  const state = JSON.stringify(store.getState())
  localStorage["redux-store"] = state
})
```

We can then load this data from local storage when our application loads.

```javascript
import C from "./constants"
import appReducer from "./store/reducers"
import { createStore } from "redux"

const initialState = localStorage["redux-store"]
  ? JSON.parse(localStorage["redux-store"])
  : {}

const store = createStore(appReducer, initialState)

window.store = store

store.subscribe(() => {
  const state = JSON.stringify(store.getState())
  localStorage["redux-store"] = state
})

store.dispatch({
  type: C.SET_GOAL,
  payload: 2
})
```

It's possible to add your store to `window`, which might be helpful for
debugging, but you don't want to leave that in place in production.

```javascript
const store = createStore(appReducer, initialState)
window.store = store
```

Console:

```
> store.getState();
< {allSkiDays: Array(0), goal: 10, errors: Array(0), resortNames: {…}}
```

You can view the data in localStorage as well, as a JSON string.

Console:

```
> localStorage
< Storage {redux-store: "{"allSkiDays":[],"goal":2,"errors":[],"resortNames":{"fetching":false,"suggestions":[]}}", loglevel:webpack-dev-server: "INFO", length: 2}
```

You can clear localStorage by using `localStorage.clear()`.

```javascript
> localStorage.clear()
< undefined
> localStorage
< Storage {length: 0}
```

Now the key is gone. When we refresh, and it makes the first mutation to the
state, the current state is saved to localStorage, and loaded when the page
refreshes.

## Unsubscribe from the store

It's also possible to turn off store subscriptions using `unsubscribe()`.

Let's say we have this subscription to load the state every time it's modified,
and we're using a loop (ever 250 milliseconds, 4 times a second) to change
the goal to a random number.

```javascript
import C from "./constants"
import appReducer from "./store/reducers"
import { createStore } from "redux"

const store = createStore(appReducer)

store.subscribe(() => console.log(`   Goal: ${store.getState().goal}`))

setInterval(() => {
  store.dispatch({
    type: C.SET_GOAL,
    payload: Math.floor(Math.random() * 100)
  })
}, 250)
```

When you call `store.subscribe()`, it returns a function that can be used to
unsubscribe.

```javascript
import C from "./constants"
import appReducer from "./store/reducers"
import { createStore } from "redux"

const store = createStore(appReducer)

const unsubscribeGoalLogger = store.subscribe(() =>
  console.log(`   Goal: ${store.getState().goal}`)
)

setInterval(() => {
  store.dispatch({
    type: C.SET_GOAL,
    payload: Math.floor(Math.random() * 100)
  })
}, 250)

setTimeout(() => {
  unsubscribeGoalLogger()
}, 3000)
```

The output in the console should be like so, running for only 3 seconds:

```
  Goal: 40
  Goal: 45
  Goal: 58
  Goal: 86
  Goal: 13
  Goal: 91
  Goal: 35
  Goal: 98
  Goal: 9
  Goal: 48
  Goal: 41
  Goal: 47
```

## Create middleware

Middleware gives you control over how actions are dispatched. You can add
functionality before or after the action is dispatched. We can delay actions, or
skip them altogether.

Here's a simple way of establishing our store.

```javascript
// store/index.js
import C from "../constants"
import appReducer from "./reducers"
import { createStore } from "redux"

export default (initialState = {}) => {
  return createStore(appReducer, initialState)
}
```

Middleware uses a [Higher-Order Function](https://en.wikipedia.org/wiki/Higher-order_function),
that is, a function that takes a function as an argument, or returns a function.

Let's make a method to log messages to the console. The store is going to be
injected into this function.

```javascript
const consoleMessages = function(store) {
  return function(next) {
    return function(action) {
      // ...
    }
  }
}
```

We can write this more simply like so using ES6 syntax:

```javascript
const consoleMessages = store => next => action => {
  // ...
}
```

Because each arrow function only have one argument, the parenthesis aren't
necessary. This function only dispatches the action. This makes sure
that we are not breaking the stores current dispatch pipeline.

We can add functionality before or after the dispatching of the action as
needed with this function, thus modifying the pipeline... thus middleware.

```javascript
const consoleMessages = store => next => action => {
  let result
  result = next(action)
  return result
}
```

Let's create a console group before we dispatch the action. Console groups allow
us to group all of the logs associated with this action into a collapsible
group in the console.

We replace the `createStore` method in our exported default method with a call
to `applyMiddleware`. It returns a store with our middleware applied, which we
want to send the `createStore` function to, which we want to pass our
`appReducer` and `initialState` to.

```javascript
// src/store/index.js
import appReducer from "./reducers"
import { createStore, applyMiddleware } from "redux"

const consoleMessages = store => next => action => {
  let result

  console.groupCollapsed(`dispatching action => ${action.type}`)
  console.log("ski days", store.getState().allSkiDays.length)

  result = next(action)

  let { allSkiDays, goal, errors, resortNames } = store.getState()

  console.log(`

    ski days: ${allSkiDays.length}
    goal: ${goal}
    fetching: ${resortNames.fetching}
    suggestions: ${resortNames.suggestions}
    errors: ${errors.length}

  `)
  console.groupEnd()

  return result
}

export default (initialState = {}) => {
  return applyMiddleware(consoleMessages)(createStore)(appReducer, initialState)
}
```

Let's use this with our main code.

```javascript
// src/index.js
import C from "./constants"
import storeFactory from "./store"

const initialState = localStorage["redux-store"]
  ? JSON.parse(localStorage["redux-store"])
  : {}

const saveState = () => {
  const state = JSON.stringify(store.getState())
  localStorage["redux-store"] = state
}

const store = storeFactory(initialState)

store.subscribe(saveState)

store.dispatch({
  type: C.ADD_DAY,
  payload: {
    resort: "Mt Shasta",
    date: "2016-10-28",
    powder: true,
    backcountry: true
  }
})

store.dispatch({
  type: C.ADD_DAY,
  payload: {
    resort: "Squaw Valley",
    date: "2016-3-28",
    powder: true,
    backcountry: false
  }
})

store.dispatch({
  type: C.ADD_DAY,
  payload: {
    resort: "The Canyons",
    date: "2016-1-2",
    powder: false,
    backcountry: true
  }
})
```

Our console output:

```
dispatching action => ADD_DAY
  ski days 0

    ski days: 1
    goal: 2
    fetching: false
    suggestions:
    errors: 0

dispatching action => ADD_DAY
  ski days 1

    ski days: 2
    goal: 2
    fetching: false
    suggestions:
    errors: 0

dispatching action => ADD_DAY
  ski days 2

    ski days: 3
    goal: 2
    fetching: false
    suggestions:
    errors: 0
```

# Action Creators

## What are action creators?

With Redux the store is only intended to manage state data. It should not
contain application logic such as generating unique ids, reading or writing
data to a persistence layer, changing global variables, or fetching data from
a REST endpoint via AJAX request.

Your application should use the store, the store should not be your application.

So where should our logic go?

Action creators are functions that create and return actions, allowing us to
encapsulate the logic of our application using functions not objects.

```javascript
// src/index.js
import storeFactory from "./store"
import { addDay } from "./actions"

const store = storeFactory()

store.dispatch(addDay("Heavenly", "2016-12-22"))
```

If you need to add application specific logic, you could do it within the
action creator.

```javascript
// src/actions.js
import C from "./constants"

export function addDay(resort, date, powder = false, backcountry = false) {
  // Add app logic here if needed

  return {
    type: C.ADD_DAY,
    payload: { resort, date, powder, backcountry }
  }
}
```

Let's add an action creator for removing a day.

```javascript
// src/actions.js
import C from "./constants"

export function addDay(resort, date, powder = false, backcountry = false) {
  // Add app logic here if needed

  return {
    type: C.ADD_DAY,
    payload: { resort, date, powder, backcountry }
  }
}

export const removeDay = function(date) {
  return {
    type: C.REMOVE_DAY,
    payload: date
  }
}

export const setGoal = goal => ({
  type: C.SET_GOAL,
  payload: goal
})
```

Let's add those to our main script.

```javascript
// src/index.js
import storeFactory from "./store"
import { addDay, removeDay, setGoal } from "./actions"

const store = storeFactory()

store.dispatch(addDay("Heavenly", "2016-12-22"))

store.dispatch(removeDay("2016-12-22"))

store.dispatch(setGoal(55))
```

## Async actions with redux-thunk

Your logic often has to deal with asynchronicity, such as asynchronous requests
to a server. We need to be able to work with action creators that will wait
for a response before dispatching an action.

Redux-Thunk is middleware that we can add to our store. Thunks are higher-order
functions that give you control over when and how often actions are dispatched.

Redux-thunk looks at every action that is dispatched, and if it's a function, it calls that function.

```bash
npm install redux-thunk --save
```

```javascript
// src/store/index.js

import C from "../constants"
import appReducer from "./reducers"
import thunk from "redux-thunk"
import { createStore, applyMiddleware } from "redux"
const consoleMessages = store => next => action => {
  let result
  // ...
  return result
}

export default (initialState = {}) => {
  return applyMiddleware(thunk, consoleMessages)(createStore)(
    appReducer,
    initialState
  )
}
```

Just like other action creators, Thunks are functions.

We're going to dispatch this just like any other action creator. The difference
is that Thunks don't return the action object directly, they return another
function.

We can call dispatch actions as often as we like from within a Thunk, and we can
also delay the dispatch.

Because Thunks get the dispatch function, we have control over when and how
often we're going to dispatch actions. We can also use `getState()` to check
the state before dispatching actions.

```javascript
// src/store/reducers.js

// ...
export const fetching = (state = false, action) => {
  switch (action.type) {
    case C.FETCH_RESORT_NAMES:
      return true
    case C.CANCEL_FETCHING:
      return false
    case C.CHANGE_SUGGESTIONS:
      return false
    default:
      return state
  }
}
// ...
```

```javascript
// src/actions.js

export const randomGoals = () => (dispatch, getState) => {
  if (!getState().resortNames.fetching) {
    dispatch({
      type: C.FETCH_RESORT_NAMES
    })

    setTimeout(() => {
      dispatch({
        type: C.CANCEL_FETCHING
      })
    }, 1500)
  }
}
```

So in this case, if we're not currently fetching resort names, then we'll start
the process of fetching them. After a second and a half, it will dispatch the
action to cancel the fetching.

```javascript
// src/index.js

import storeFactory from "./store"
import { randomGoals } from "./actions"

const store = storeFactory()

store.dispatch(randomGoals())
```

Terminal Output:

```
dispatching action => FETCH_RESORT_NAMES
dispatching action => CANCEL_FETCHING
```

What if we dispatched our `randomGoals()` twice?

Terminal Output:

```
dispatching action => FETCH_RESORT_NAMES
dispatching action => CANCEL_FETCHING
```

This is because the state of 'fetching' became true.

## Autocomplete thunk

Let's imagine that we have an API end-point running on an Express back-end,
accessible from `/resorts/{search string}`. For example, a request to
`/resorts/hea` returns `["Heavenly Ski Resort", "Heavens Sonohara"]`.

We want to use this to provide suggestions of resorts to choose from in a
search field.

In order to make an AJAX request to the suggestions server, we'll use a library
called isomorphic-fetch.

```bash
$ npm install isomorphic-fetch -save
```

This library is an implementation of the
[whatwg fetch specification](https://fetch.spec.whatwg.org/) that works in
NodeJS and the browser. This is a standard for fetching resources from APIs.

```javascript
// src/index.js

import storeFactory from "./store"
import { suggestResortNames } from "./actions"

const store = storeFactory()

store.dispatch(suggestResortNames("hea"))
```

```javascript
// src/actions.js
import C from './constants'
import fetch from 'isomorphic-fetch'

export function addDay(resort, date, powder=false, backcountry=false) {
  return {
    type: C.ADD_DAY,
    payload: { resort, date, powder, backcountry }
  }
}

// ...

export const suggestResortNames = value => (dispatch) {
  dispatch({
    type: C.FETCH_RESORT_NAMES
  })

  fetch('http://localhost:3333/resorts/' + value)
    .then(response => response.json())
    .then(suggestions => {
      dispatch({
        type: C.CHANGE_SUGGESTIONS,
        payload: suggestions
      })
    })
    .catch(error => {
      dispatch({
        addError(error.message)
      })
      dispatch({
        type: C.CANCEL_FETCHING
      })
    })
}
```

Our function returned by the thunk `suggestResortNames` could accept both the
`dispatch` and `getState` methods, but it only needs the `dispatch` function.

Console Output:

```
dispatching action => FETCH_RESORT_NAMES
dispatching action => CHANGE_SUGGESTIONS
```

A half second after the first line, the `CHANGE_SUGGESTIONS` shows up after
the suggestions are received from the API and added to the state.

You can stop the server you're running and refresh the page, and this will
result in the errors.

Console Output:

```
dispatching action => FETCH_RESORT_NAMES
dispatching action => ADD_ERROR
dispatching action => CANCEL_FETCHING
```

# Incorporating React

## React app overview

Thus far we've used Redux to construct the client data layer for our
application. It's now time to implement the user interface layer for our new
store.

- src
  - components
    - containers
    - ui
    - index.js
  - store
    - index.js
    - reducers.js
  - stylesheets
    - index.scss
    - Menu.scss
    - ShowErrors.scss
    - SkiDayList.scss
  - actions.js
  - constants.js
  - index.js
  - initialState.json
  - routes.js

React-Redux helps us integrate our store with our React components.

```javascript
// src/index.js
import C from "./constants"
import React from "react"
import { render } from "react-dom"
import routes from "./routes"
import sampleData from "./initialState"

const initialState = localStorage["redux-store"]
  ? JSON.parse(localStorage["redux-store"])
  : sampleData

const saveState = () =>
  (localStorage["redux-store"] = JSON.stringify(store.getState()))

window.React = React

render(routes, document.getElementById("react-container"))
```

Let's bring our store in.

```javascript
// src/index.js
import C from "./constants"
import React from "react"
import { render } from "react-dom"
import routes from "./routes"
import sampleData from "./initialState"
import storeFactory from "./store"

const initialState = localStorage["redux-store"]
  ? JSON.parse(localStorage["redux-store"])
  : sampleData

const saveState = () =>
  (localStorage["redux-store"] = JSON.stringify(store.getState()))

const store = storeFactory(initialState)
store.subscribe(saveState)

// to aid with interacting from JS console
window.React = React
window.store = store

render(routes, document.getElementById("react-container"))
```

We need to be able to pass the store down to our component tree. React Redux
has a compnent we can use called Provider that does this.

```javascript
import { Provider } from "react-redux"
```

You can wrap the Provider component around any component tree, and it will
place the store in Context. Context is a feature that will allow any child
React component to interact with the store if needed.

```javascript
render(
  <Provider store={store}>{routes}</Provider>,
  document.getElementById("react-container")
)
```

This will place the store in context so that it's accessible by any of the child
components listed under routes.

## Map props to React components

We're going to wire up the ski day count.

In the folder structure outlined above, the components in `src/components` are
organized under either the `containers` folder or `ui` folder.

The `ui` folder contains user interface components, which are pure react
components. They communicate solely through properties. They pass data back
up to their parents through two-way data binding, and they receive data from
properties as well.

The `container` folder contains wrappers used to feed data to our components.

For example, the following container component is a stateless functional
component that wraps around the SkiDayCount component. Currently the variables
being passed are hardcoded. We want this map data from our store to the
properties of the SkiDayCount component.

```javascript
// src/components/containers/skiDayCount.js
import SkiDayCount from "../ui/SkiDayCount"

export default () => <SkiDayCount total={100} powder={25} backcountry={10} />
```

To do this we'll use `connect` provided by `react-redux` that creates a
component that grabs the store out of state, and can map state from the store
to properties in a child component.

We need to define a function that receives the state and returns an object
that contains keys for the properties of the `SkiDayCount` component,
and values that represent the values we want passed into the component. This
is defined below in `mapStateToProps`.

The `connect` function is a higher order function. It takes our
`mapStateToProps` function as an argument, and it returns a
function that expects the component we wish to wrap as it's first argument
(SkiDayCount).

```javascript
// src/components/containers/skiDayCount.js
import SkiDayCount from "../ui/SkiDayCount"
import { connect } from "react-redux"

const mapStateToProps = state => {
  return {
    total: state.allSkiDays.length,
    powder: state.allSkiDays.filter(day => day.powder).length,
    backcountry: state.allSkiDays.filter(day => day.backcountry).length
  }
}

const Container = connect(mapStateToProps)(SkiDayCount)
export default Container
```

## Map dispatch to React components

Next we want to work with a component that displays errors.

If the user chooses to close the error, a 'CLEAR_ERROR' action should be
dispatched.

Below we have our ShowErrors component. We need to replace the errors with
our action errors from the data store, and also pass a function to the
component for the `onClearError` property that will dispatch the 'CLEAR_ERROR'
action.

```javascript
// src/components/ShowErrors.js
import ShowErrors from "../ui/ShowErrors"

export default () => (
  <ShowErrors
    errors={["sample error"]}
    onClearError={index => console.log("todo: clear error at", index)}
  />
)
```

```javascript
// src/components/ShowErrors.js
import ShowErrors from "../ui/ShowErrors"
import { clearError } from "../../actions"
import { connect } from "react-redux"

const mapStateToProps = state => {
  return {
    errors: state.errors
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onClearError(index) {
      dispatch(clearError(index))
    }
  }
}

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(ShowErrors)
```

We want to make sure that any errors that occur get recorded in state.
Anytime an error occurs, we want to add this to the state.

```javascript
// src/index.js
import C from "./constants"
import React from "react"
import { render } from "react-dom"
import routes from "./routes"
import sampleData from "./initialState"
import storeFactory from "./store"
import { Provider } from "react-redux"
import { addError } from "./actions"

// ...

const handleError = error => {
  store.dispatch(addError(error))
}

// ...

window.addEventListener("error", handleError)
```

If we add a call at the bottom of our file now, such as `foo = bar`,
we get `Uncaught ReferenceError: bar is not defined` added to our
errors.

Console:

```
dispatching action => ADD_ERROR
Uncaught ReferenceError: bar is not defined(...)
```

Now any errors that occur with our application are displayed in the UI.

## Map router params to React components

In our routes we go to `/list-days/` to view all the ski days. If instead we go
to `/list-days/backcountry` we want a filter applied that only shows the
backcountry days, or if we go to `/list-days/powder` we want to only see the
powder days.

So for our ListSkiDays component we're going to need to pass not only the
list of days, but also the router parameter that represents the filter.

Also, if the user double clicks on any of the days, we should remove that
day from the list.

Here is how our container is configured, with sample list data and a console
log statement when an item is double clicked.

```javascript
// src/components/containers/SkiDayList.js
import SkiDayList from "../ui/SkiDayList"

const sample = [
  {
    resort: "Stowe",
    date: "2017-1-28",
    powder: false,
    backcountry: false
  },
  {
    resort: "Tuckerman's Ravine",
    date: "2017-1-31",
    powder: false,
    backcountry: true
  },
  {
    resort: "Mad River Glen",
    date: "2017-2-12",
    powder: true,
    backcountry: false
  }
]

export default props => (
  <SkiDayList
    days={sample}
    filter={props.params.filter}
    onRemoveDay={date => console.log("remove day on", date)}
  />
)
```

An arrow function will return whatever is on the other side of the arrow, so

```javascript
// src/components/containers/SkiDayList.js
import SkiDayList from "../ui/SkiDayList"
import { connect } from "react-redux"
import { removeDay } from "../../actions"

const mapStateToProps = (state, props) => ({
  days: state.allSkiDays,
  filter: props.params.filter
})

const mapDispatchToProps = dispatch => ({
  onRemoveDay(date) {
    dispatch(removeDay(date))
  }
})

export default connect(mapStateToProps.mapDispatchToProps)(SkiDayList)
```

## Create containers for form components

# Conclusion

## Next steps
