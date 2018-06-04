---
layout: page
title: VueJS Advanced Components
---

# Build a Reactivity System

Vue's reactivity system can look like magic when you see it working for the
first time.

```html
<div id="app">
  <div>Price: ${{ price }}</div>
  <div>Total: ${{ price * quantity }}</div>
  <div>Tax: ${{ totalPriceWithTax }}</div>
</div>
```

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      price: 5.00,
      quantity: 2
    },
    computed: {
      totalPriceWithTax() {
        return this.price * this.quantity * 1.03
      }
    }
  })
</script>
```

Somehow Vue just knows that if `price` changes, it should do three things:

* Update the `price` value on our webpage.
* Recalculate the expression that multipllies `price * quantity`, and update the page.
* Call the `titalPriceWithTax` function again and update the page.

But how does Vue know what to update when the `price` changes, and how does it
keep track of everything?

JavaScript is procedural, not reactive, so this doesn't work with the following:

```javascript
let price = 5
let quantity = 2
let total = price * quantity
price = 20
console.log("total is ${total}")
```

With VueJS we might expect this to output dynamically

```
>> total is 40
```

This is solved by putting the total calculation in a method so we can re-run it
when the `price` or `quantity` change.

```javascript
let price = 5
let quantity = 2
let total = 0
let target = null

target = function() {
  total = price * quantity
}

record() // remember this in case we want to run it later
target() // Also go ahead and run it
```

We would write the same method using ES6 arrow syntax:

```javascript
target = () => {
  total = price * quantity
}
```

The definition of the `record` is simply:

```javascript
let storage = []

function record() {
  storage.push(target)
}
```

We're storing this function so that we can run it later, rehaps with a `replay`
function that runs all the things we've recorded.

```javascript
function replay() {
  storage.forEach(run => run())
}
```

This goes through all the anonymous functions we have stored inside the storage
array and executes each of them. Then in our code we can just:

```javascript
price = 20
console.log(total) // => 10
replay()
console.log(total) // => 40
```

Here is the entire code:

```javascript
let price = 5
let quantity = 2
let total = 0
let target = null
let storage = []

function record() {
  storage.push(target)
}

function replay() {
  storage.forEach(run => run())
}

target = () => {
  total = price * quantity
}

record()
target()

price = 20
console.log(total) // => 10
replay()
console.log(total) // => 40
```

## Dependency Class

We could go on recording targets as needed, but it would be nice to have a more
robust solution that will scale with the app.

A class that takes care of maintaining a list of targets that get notified when
we need them to get re-run. Here is a **Dependency Class** that implements the
standard programming observer pattern.

```javascript
// "Dep" stands for dependency
class Dep {
  constructor() {
    this.subscribers = [] // The targets that are dependent, and should be
    // run when notify() is called.
  }
  depend() {
    // This replaces our record function
    if (target && !this.subscribers.includes(target)) {
      // Only if there is a target & it's not already subscribed
      this.subscribers.push(target)
    }
  }
  notify() {
    // Replaces our replay function
    this.subscribers.forEach(sub => sub()) // Run our targets, or observers.
  }
}
```

Now instead of `storage`, we're instead storing our anonymous functions in
`subscribers`. We also use `depend` instead of `record`, and `notify` instead
of `replay`.

```javascript
const dep = new Dep()

let price = 5
let quantity = 2
let total = 0
let target = () => {
  total = price * quantity
}
dep.depend() // Add this target to our subscribers
target() // Run it to get the total

console.log(total) // => 10 .. The right number
price = 20
console.log(total) // => 10 .. No longer the right number
dep.notify() // Run the subscribers
console.log(total) // => 40  .. Now the right number
```

This still works, and our code feels re-usable. It does seem weird setting and
running the `target`.

## Watcher Function

We're going to eventually have a Dep class for each variable. It would be nice
to encapsulate the behavior of creating anonymous functions that need to be
watched for updates. Perhaps a `watcher` function might be in order to take
care of this behavior.

Instead of making the following call:

```javascript
target = () => {
  total = price * quantity
}
dep.depend()
target()
```

We can instead call:

```javascript
watcher(() => {
  total = price * quantity
})
```

Instead of our `watcher` function, we can do a few simple things:

```javascript
function watcher(myFunc) {
  target = myFunc // set as the active target
  dep.depend() // add the active target as a dependency
  target() // Call the target
  target = null // Reset the target
}
```

So the `watcher` function takes a `myFunc` argument, sets that as our global
`target` property, calls `dep.depend()` to add our target as a subscriber,
calls the `target` function, and then resets the `target` value to null.

```javascript
price = 20
console.log(total) // => 10
dep.notify()
console.log(total) // => 40
```

Why did we implement `target` as a global variable, rather than pass it into
our functions where needed? You'll see why ahead.

## Object.defineProperty()

We have a single `Dep class`, but what we really want is each of our variables
to have its own Dep. Let's move things into properties before we go further.

```javascript
let data = { price: 5, quantity: 2 }
```

Let's imagine that each of our properties (`price` and `quantity`) have their
own internal Dep class.

Now when we run:

```javascript
watcher(() => {
  total = data.price * data.quantity
})
```

Since the `data.price` value is accessed, I want the `price` property's Dep
class to push our anonymous function (stored in `target`) onto its subscriber
array (by calling `dep.depend()`). Since `data.quantity` is accessed I also
want the `quantity` property Dep class to push this anonymous function
(stored in `target`) into its subscriber array.

If I have another anonymous function where just `data.price` is accessed, I
want that pushed just to the `price` property Dep class.

```javascript
watcher(() => {
  salePrice = data.price * 0.9
})
```

This is an example of how additional watchers may only apply to one of our
properties.

When do I want `dep.notify()` to be called on `price`'s subscribers? I want
them to be called when `price` is set.

I want to be able to go into the console and do:

```javascript
>> total
10
>> price = 20 // When this gets run it will need to call notify() on the price
>> total
40
```

We need some way to hook into a data property (like `price` or `quantity`) so
when it's accessed we can save the `target` into our subscribe array, and when
it's changed run the functions stored in our subscribe array.

## Solution

We need to learn about the [Object.defineProperty()] function which is plain
ES5 JavaScript. It allows us to define getter and setter functions for a
property. Here is the basic usage:

```javascript
let data = { price: 5, quantity: 2 }

Object.defineProperty(data, 'price', {

  get() {
    console.log('I was accessed')
  }

  set(newVal) {
    console.log('I was changed')
  }

})

data.price // this calls get()
data.price = 20 // thi calls set()
```

As you can see, it just logs two lines.

```javascript
>> I was accessed
>> I was changed
```

However, it doesn't actually `get` or `set` any values, since we over-rode the
functionality. Let's add it back now. `get()` expected to return a value, and
`set()` still needs to update a value, so let's add an `internalValue` variable
to store our current `price` value.

```javascript
let data = { price: 5, quantity: 2 }

let internalValue = data.price // Our initial value.

Object.defineProperty(data, "price", {
  // For just the price property

  get() {
    // Create a get method
    console.log(`Getting price: ${internalValue}`)
    return internalValue
  },

  set(newVal) {
    // Create a set method
    console.log(`Setting price to: ${newVal}`)
    internalValue = newVal
  }
})
total = data.price * data.quantity // This calls get()
data.price = 20 // This calls set()
```

Now that our get and set are working properly, what do you think will print to
the console?

```javascript
Getting price: 5
Setting price to: 20
```

So we have a way to get notified when we get and set values. And with some
recursion we can run this for all items in our data array, right?

FYI, `Object.keys(data)` returns an array of the keys of the object.

```javascript
let data = { price: 5, quantity: 2 }

Object.keys(data).forEach(key => {
  // We're running this for each item in data now
  let internalValue = data[key]
  Object.defineProperty(data, key, {
    get() {
      console.log(`Getting ${key}: ${internalValue}`)
      return internalValue
    },
    set(newVal) {
      console.log(`Setting ${key} to: ${newVal}`)
      internalValue = newVal
    }
  })
})
total = data.price * data.quantity
data.price = 20
```

Now everything has getters and setters, and we see this in the console.

```
Getting price: 5
Getting quantity: 2
Setting price to: 20
```

## Combining Approaches

```javascript
total = data.price * data.quantity
```

When a piece of code like this gets run and **gets** the value of `price`, we
want `price` to remember this anonymous function (`target`). That way if
`price` gets changed, or is **set** to a new value, it will trigger this
function to get rerun, since it knows this line is dependent upon it. So you
can think of it like this.

* **Get** => Remember this anonymous function, we'll run it again when our value changes.
* **Set** => Run the saved anonymous function, our value just changed.

Or in the case of our Dep class

**Price accessed (get)** => call `dep.depend()` to save the current `target`

**Price set** => call `dep.notify` on price, re-running all the `targets`

Let's combine these two ideas, and walk through our final code.

```javascript
let data = { price: 5, quantity: 2 }
let target = null

// This is exactly the same Dep class
class Dep {
  constructor() {
    this.subscribers = []
  }
  depend() {
    if (target && !this.subscribers.includes(target)) {
      // Only if there is a target & it's not already subscribed
      this.subscribers.push(target)
    }
  }
  notify() {
    this.subscribers.forEach(sub => sub())
  }
}

// Go through each of our data properties
Object.keys(data).forEach(key => {
  let internalValue = data[key]

  // Each property gets a dependency instance
  const dep = new Dep()

  Object.defineProperty(data, key, {
    get() {
      dep.depend() // <-- Remember the target we're running
      return internalValue
    },
    set(newVal) {
      internalValue = newVal
      dep.notify() // <-- Re-run stored functions
    }
  })
})

// My watcher no longer calls dep.depend,
// since that gets called from inside our get method.
function watcher(myFunc) {
  target = myFunc
  target()
  target = null
}

watcher(() => {
  data.total = data.price * data.quantity
})
```

And now look at what happens in our console when we play around.

```
> data.total
< 10
> data.price = 20
< 20
> data.total
> 40
> data.quantity = 3
< 3
> data.total
< 60
```

Exactly what we were hoping for! Both `price` and `quantity` are indeed
reactive! Our total code gets re-run whenever the value of `price` or
`quantity` gets updated.

### Jumping to Vue

This illustration from the Vue docs should start to make sense now.

![image](/images/resources/notes/vuejs/vuejs-watcher-render-diagram.png)

Do you see that beautiful purple Data circle with the getts and setters? It
should look familiar! Every component instance has a `watcher` instance (in
blue) which collects dependencies from the getters (red line). When a setter is
called later, it **notifies** the watcher which causes the component to
re-render.

Obviously how Vue does this under the hood is more complex, but you now know the
basics. In the next lesson we'll dive under the hood with Vue, and see if we can
find this pattern inside the source code.

### Overview

* How to create a **Dep class** which collects all dependencies (depend) and re-runs all dependencies (notify).
* How to create a **watcher** to manage the code we're running, that may need to be added (target) as a dependency.
* How to use **Object.defineProperty()** to create getters and setters.

# Reactivity in Vue.js

Using the knowledge we learned building a reactivity system, we'll dive into the
Vue.js source code to find where the reactivity lives. Doing so will help us:

* Get comfortable parsing through the Vue.js source
* Learn Vue.js design patterns and architecture
* Improve your debugging skills
* Learn caveats to the reactivity system

We'll start with a simple Vue application.

```html
<div id="app">
  <h1>{{ product }}</h1>
</div>

<script src="vue.js"></script>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      product: "Socks"
    }
  })
</script>
```

At some point the `product` property inside the `data` object gets reactive
super powers. It'd be nice to know where and how this happens.

If we look into [the 'data' documentation] we find:

> #data
> Type: `Object | Function`
>
> Restriction: Only accepts `Function` when used in a component definition
>
> Details:
>
> The data object for the Vue instance. Vue will recursively convert its
> properties into getter/setters to make it "reactive". **The object must be
> plain:** native objects such as browser API objects and prototype properties
> are ignored. A rule of thumb is that data should just be data - it is not
> recommended to observe objects with their own stateful behavior.
>
> Once observed, you can no longer add reactive properties to the root data
> object. It is therefore recommended to declare all root-level reactive
> properties upfront, before creating the instance.

See how it talks about getters/setters to make it reactive? It also mentions
that the object must be plain, just data. So somewhere in the source it's making
the properties in data reactive, and adding the ability to remember what needs
to get updated when the property values change. Remember, when data is GET we
**add** a dependency, and when it's SET we **notify** all the dependencies.

## Tracing Code Execution

I started by downloading the [Vue.js source code] so I could generate a local
version of Vue. I wrote up an `index.html` file with the contents that you see
above, and used the Chrome DevTools Debugger to watch the code execute one line
at a time. It's easier than you might think.

When our Vue application starts up, we'll begin here:

_[/src/core/instance/index.js]_

```javascript
import { initMixin } from './init'
import { stateMixin } from './state'
import { renderMixin } from './render'
import { eventsMixin } from './events'
import { lifecycleMixin } from './lifecycle'
import { warn } from '../util/index'

function Vue (options) {
  ... // ommitted code
  this._init(options)
}
initMixin(Vue)  // goes here next
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)
export default Vue
```

There's our root Vue function that creates our instance when we call
`var app = new Vue({ ... })`. Notice it calls \_init, which is a prototype
function that gets added to our Vue object.

**Note:** In the code above and the code that follows I've omitted code to
simplify what you have to read.

_[/src/core/instance/init.js]_

```javascript
export function initMixin (Vue: Class<Component>) {
  Vue.prototype._init = function (options?: Object) {
    const vm: Component = this

    ... Normalizing options ...
    vm._self = vm
    initLifecycle(vm)
    initEvents(vm)
    initRender(vm) // defineReactive attrs and listeners
    callHook(vm, 'beforeCreate') // Notice this lifecycle call hook
    initInjections(vm) // resolve injections before data/props
    initState(vm)  // <---
    initProvide(vm) // resolve provide after data/props
    callHook(vm, 'created') // Notice this lifecycle call hook
    ...
    vm.$mount(vm.$options.el)
  }
}
```

Inside init we do many things, but we're interested in InitState.

_[src/core/instance/state.js]_

```javascript
export function initState (vm: Component) {
  vm._watchers = []
  const opts = vm.$options
  if (opts.props) initProps(vm, opts.props)
  if (opts.methods) initMethods(vm, opts.methods)
  if (opts.data) {
    initData(vm)  // <---  We do have data
  } else {
    observe(vm._data = {}, true /* asRootData */)
  }
  if (opts.computed) initComputed(vm, opts.computed)
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch)
  }
}
...
```

As you can see, we initialize Props, Methods, and then initData, which continues
in the same file below.

```javascript
...
  function initData (vm: Component) {
    let data = vm.$options.data // The value here is { product: "Socks" }
    ...

    observe(data, true /* asRootData */)  // <--- Here we go
  }
```

See how we call "observe" on our data? We're getting closer.

_[/src/core/observer/index.js#109]_

```javascript
  export function observe (value: any, asRootData: ?boolean): Observer | void {
    ... // Our value is still { product: "Socks" }
    ob = new Observer(value)
    return ob
  }
```

**Note:** The above code uses [Facebook Flow](https://github.com/facebook/flow),
a tool used to add static typing to JavaScript to improve code quality.

So the observer does something with our data. Let's dive deeper.

_[/src/core/observer/index.js#37]_

```javascript
  export class Observer {
    value: any;

    constructor (value: any) {
      this.value = value  // { product: "Socks" }
      ...
      if (Array.isArray(value)) {  // We don't have an array but if we did
        ...
        this.observeArray(value) // We would call observeArray (shown below)
      } else {
        this.walk(value)  // Walk through each property
      }
    }
    /**
      * Walk through each property and convert them into
      * getter/setters. This method should only be called when
      * value type is Object.
      */
    walk (obj: Object) {
      const keys = Object.keys(obj)  // keys = ["product"]
      for (let i = 0; i < keys.length; i++) {  // Go through each key
        defineReactive(obj, keys[i], obj[keys[i]])  // <--- Going here next
                    // Calling defineReactive(obj, 'product', 'Socks')
      }
    }
    /**
      * Observe a list of Array items.
      */
    observeArray (items: Array<any>) {
      for (let i = 0, l = items.length; i < l; i++) {
        observe(items[i])  // Just call observe on each item
      }
    }
  }
```

See how if our data is an array we call `observe` on each of the items? If it's
not an array we go to the `walk` function, get all our keys, and then we call
`defineReactive`.

That's where we go next.

_[/src/core/observer/index.js#134]_

```javascript
  /**
    * Define a reactive property on an Object.
    */
  export function defineReactive (
    obj: Object,
    key: string,  // <--- "product"
    val: any, // <--- "Socks" this will be our internalValue
    customSetter?: ?Function,
    shallow?: boolean
  ) {
    const dep = new Dep() // <--- There's our dependency class like from the last lesson.
    const property = Object.getOwnPropertyDescriptor(obj, key)
    if (property && property.configurable === false) {
      return // if property is not set as configurable, then return
    }
    // cater for pre-defined getter/setters
    const getter = property && property.get
    const setter = property && property.set

    Object.defineProperty(obj, key, {  // <--- There's our defineProperty
      enumerable: true,
      configurable: true,
      get: function reactiveGetter () { // <--- There's our Get
        const value = getter ? getter.call(obj) : val // <--- If we have a defined getter, then use that; otherwise return value, like we did with our internalVal.

        if (Dep.target) {
          dep.depend() // <-- There's our depend function
          ...
        }
        return value // A Getter returns a value.
      },
      set: function reactiveSetter (newVal) {
        const value = getter ? getter.call(obj) : val // <-- if custom getter

        // If the value is the same don't do anything.
        if (newVal === value || (newVal !== newVal && value !== value)) {
          return
        }

        if (setter) {
          setter.call(obj, newVal)
        } else {
          val = newVal // <--- Set the new value.
        }
        dep.notify() // <-- There's our notify
      }
    })
  }
```

## What's inside of Dep?

Surely you're curious what is inside the Dep class, but we need to understand
more about Vue's `Watcher class`. You might remember our watcher function.

```javascript
function watcher(myFunc) {
  target = myFunc
  target()
  target = null
}
```

Vue has a Watcher class which:

* Receives as a parameter the code to watch (like above)
* Stores the code inside a `getter` property
* Has a `get` function (called directly in instantiation, or by the scheduler) which:
  * Runs `pushTarget(this)` to set `Dep.target` to this watcher object
  * Calls `this.getter.call` to run this code
  * Runs `popTarget()` to remove current `Dep.target`
* Has an `update` function to queue this `watcher` to run (using a scheduler)

_[/src/core/observer/watcher.js]_

```javascript
  export default class Watcher {
    ...
    get() {
      pushTarget(this) // Set Dep.target to this watcher object
      ...
      value = this.getter.call(vm, vm) // run the code
      ...
      popTarget() // remove current Dep.target
      return value
    }
    update() {
      if (this.lazy) {
        this.dirty = true
      } else if (this.sync) {
        this.run() // shown below
      } else {
        queueWatcher(this) // queue this to run later
      }
    }
    run() {
      if (this.active) {
        const value = this.get()
      }
    }
    addDep(dep: Dep) { // This is called to start tracking a dep
      ...
      this.newDeps.push(dep) // The watcher also track's deps
      dep.addSub(this) // Calls back to the dep (below)
    }
  }
```

Now that you know what a `Watcher` looks like, the dep class should make a
little more sense:

_[/src/core/observer/dep.js#L12]_

```javascript
  export default class Dep {
    ...
    subs: Array<Watcher>;  // Notice our susbcribers are of class Watcher.
    constructor () {
      this.subs = []  // Our subscribers we need to keep track of
    }
    addSub(sub: Watcher) {  // You can think of this sub Watcher as our target
      this.subs.push(sub)
    }
    ...
    depend() { // <-- There's our depend function
      if (Dep.target) {  // If target exists
        Dep.target.addDep(this)  // Add this as a dependency, which ends up calling addSub function above.  Pushing this watcher.
      }
    }
    notify() { // <--- There's our notify function
      const subs = this.subs.slice()
      for (let i = 0, l = subs.length; i < l; i++) {
        subs[i].update()  // <-- Queue and run each subscriber
      }
    }
  }
```

You can see how this is very similar to the reactivity engine we built in the
previous level. If we look at our diagram again, it should make a little more
sense.

![image](/images/resources/notes/vuejs/vuejs-watcher-render-diagram.png)

Some day you might need to dive into the source, and now perhaps it will be
a little less scary.

## Caveats

There are limitations to the reactivity system that you should get a little familiar
with, and the Vue documentation is extremely well written on it. Some of
these could cause some of those hair-pulling bugs someday, so trust me
on taking a quick look.

The first caveat involves [addition or deletion of reactive properties], like
inside the data object. The second involves [the way you might make changes to an array that is reactive].

[object.defineproperty()]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty
[the 'data' documentation]: https://vuejs.org/v2/api/#Options-Data
[vue.js source code]: https://github.com/vuejs/vue/tree/5e3823a5
[/src/core/instance/index.js]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/instance/index.js
[/src/core/instance/init.js]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/instance/init.js
[/src/core/instance/state.js]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/instance/state.js
[/src/core/observer/index.js#109]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/observer/index.js#L109,L129
[/src/core/observer/index.js#37]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/observer/index.js#L37,L78
[facebook flow]: https://github.com/facebook/flow
[/src/core/observer/index.js#134]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/observer/index.js#L134,L191
[/src/core/observer/watcher.js]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/observer/watcher.js#L104,L126
[/src/core/observer/dep.js#l12]: https://github.com/vuejs/vue/blob/5e3823a5/src/core/observer/dep.js#L12,L43
[addition or deletion of reactive properties]: https://vuejs.org/v2/guide/list.html#Array-Change-Detection
[the way you might make changes to an array that is reactive]: https://vuejs.org/v2/guide/list.html#Array-Change-Detection
