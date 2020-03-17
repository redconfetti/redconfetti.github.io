---
layout: page
title: Intro to Vue.js
---

The [Vue Dev Tools for Chrome] extension is recommended.

[vue dev tools for chrome]: https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=en

## The Vue Instance

You can insert a Vue component inside of an existing front-end, using a CDN to
include the Vue library using a `<script>` tag.

_index.html_

    <!DOCTYPE html>
    <html>
    <head>
      <meta name="viewport" content="width=device-width, initial-scale=" />
      <title>Product App</title>
    </head>
    <body>
      <div id="app">
        <h1>{{ product }}</h1>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue"></script>
      <script src="main.js"></script>
    </body>
    </html>

_main.js_

    var app = new Vue({
      el: '#app',
      data: {
        product: 'Socks'
      }
    })

In the template, anything between `{{` and `}}` can be an expression to output
something into the page. It can call a function, use logic, or just reference
variables.

In the console we can manipulate the value of the data like so:

    app.product = 'Coat'

Doing this will update the product name displayed by the Vue component.

## Attribute Binding

You can bind the value in your `data` object with attributes in the DOM.

    <img v-bind:src="image" />


    var app = new Vue({
      el: '#app',
      data: {
        product: 'Socks',
        image: './assets/duck.jpg'
      }
    })

In this case the image URL is bound to the ‘src’ attribute of the ‘img’ element.
You can do the same with the alt attribute.

    <img v-bind:src="image" v-bind:alt="altText" />

    var app = new Vue({
      el: '#app',
      data: {
        product: 'Socks',
        image: './assets/duck.jpg',
        altText: 'It\'s a duck'
      }
    })

There is also a short-hand method for binding where you just use the colon (`:`)
like so:

    <img :src="image" :alt="altText" />

## Conditional Rendering

Vue also supports directives for conditional rendering.

    <p v-if="inStock">In Stock</p>
    <p v-else>Out of Stock</p>

    data: {
      product: 'Socks',
      inStock: true
    }

You can also use else-if conditionals.

    <p v-if="inventory > 10">In Stock</p>
    <p v-else-if="inventory <= 10 && inventory > 0">Almost sold out!</p>
    <p v-else>Out of Stock</p>

    data: {
      product: 'Socks',
      inventory: 4
    }

Instead of using the if-else directives, you can simply use `v-show`.

    <p v-show="inStock">In Stock</p>

## List Rendering

If you want to render an array of data elements, you can use `v-for`.

    data: {
      product: 'Socks',
      details: ["80% cotton", "20% polyester", "100% masculine"]
    }


    <ul>
      <li v-for="detail in details">{{ detail }}</li>
    </ul>

### Iterating over Objects

You may need to iterate over JSON objects, instead of primitive values such as
integers or strings.

    data: {
      product: 'Socks',
      variants: [
        {
          id: 2234,
          color: "green"
        },
        {
          id: 2235,
          color: "blue"
        },
      ]
    }


    <div v-for="variant in variants">
      <p>{{ variant.color }}</p>
    </div>

Vue needs a unique key for each object to keep track of them, so you’ll want to
add a key attribute to each element.

    <div v-for="variant in variants" :key="variant.id">
      <p>{{ variant.color }}</p>
    </div>

If needed, you can also ensure that the index for each item is present with each
iteration.

    <div class="color-box"
         v-for="(variant, index) in variants"
         :key="variant.id"
         @mouseover="updateProduct(index)">
    </div>

## Event Handling

You can bind function callbacks to events using the `v-on` directive.

    <button v-on:click="cart + 1">Add to Cart</button>

The above example placed the expression in the template, but you can also point
to a method.

    <button v-on:click="addToCart">Add to Cart</button>

    var app = new Vue({
      el: '#app',
      data: {
        product: 'Socks',
      },
      methods: {
        addToCart() {
          this.cart += 1;
        }
      }
    })

Shorthand for `v-on` is available via the `@` character. `v-on:mouseover` is the
same as `@mouseover`.

    <p @mouseover="updateProduct(variant.image)">

## Class & Style Binding

You can apply styles to classes

    <div class="color-box"
         v-for="variant in variants"
         :key="variant.id"
         :style="{ backgroundColor: variant.color }"></div>

You can also disable a button based on a value

    <button v-on:click="addToCart"
            :disabled="!inStock"
            :class="{ disabledButton: !inStock }"
            >Add to Cart
    </button>

## Computed Properties

Computed Properties calculate a value rather than store a value.

    var app = new Vue({
      el: '#app',
      data: {
        product: 'Socks',
        brand: 'Vue Mastery'
      },
      computed: {
        title() {
          return this.brand + ' ' + this.product
        }
      }
    })

You may wonder what the difference is between using a computed property vs a
method, as both can provide the same results. The difference is that
[computed properties are cached based on their reactive dependencies]. A
computed property will only re-evaluate if a dependency has changed...
where-as a method will execute every time.

[computed properties are cached based on their reactive dependencies]: https://vuejs.org/v2/guide/computed.html#Computed-Caching-vs-Methods

## Components

You don’t want everything living within a root instance of your Vue app. You’ll
instead want to put all your logic for each element in your front-end in a
self-contained instance called a Component.

    Vue.component('product', {
      template: `
        <div class="product">
           <!-- ... Our Product HTML all goes in here ... -->
        </div>
      `,
      data() {
        return {
          // data goes here
        }
      },
      methods: {
        // methods go here
      },
      computed: {
        // computed properties go here
      }
    })

There are many ways to create a template in Vue, but in this example above we’re
using backticks to define a template literal.

You’ll also notice that our data is now a function. This is to ensure that each
component is not sharing the same ‘data’ element, as the function will return a
unique data object.

Our main app can look like this now:

    var app = new Vue({
      el: '#app'
    })

And our main HTML can be presented like so:

    <div id="app">
      <product></product>
    <div>

### Props

To pass data to components, you define ‘props’ that the component is expecting
to receive.

    Vue.component('product', {
      props: {
        premium: {
          type: Boolean,
          required: true
        }
      }
    })

We can display this ‘premium’ value in our template like so:

    <p>User is premium: {{ premium }}</p>

We can pass the value to our component from the Root instance like so:

    <div id="app">
      <product :premium="premium"></product>
    </div>

If we needed to let this prop affect the outcome of a computed value, we can
just refer to it via `this`.

    shipping() {
      if (this.premium) {
        return "Free"
      } else {
        return 2.99
      }
    }

**Note:** Don’t modify the value of props you pass into a component, as props
are overwritten when re-rendering the component. It’s best to use a computed
value to serve a mutated form of a prop if needed.

## Communicating Events

Sometimes we need to communicate events from one component to our root app. For
example, if a shopping cart item is added with a button click in one component,
and should update a value that is rendered elsewhere in the page. In such cases
you need a
[custom event](https://vuejs.org/v2/guide/components-custom-events.html).

    addToCart() {
      this.$emit('add-to-cart')
    }

For this to be accessible outside of the component, we have to apply a binding
to the component from the root app.

    <product :premium="premium" @add-to-cart="updateCart"></product>

    <!-- equivalent -->
    <product :premium="premium" v-on:add-to-cart="updateCart"></product>

In our root app we can have this method defined:

    var app = new Vue({
      el: '#app',
      methods: {
        updateCart() {
          this.cart += 1;
        }
      }
    })

## Forms

    Vue.component('product-review', {
      template: `
        <form class="review-form" @submit.prevent="onSubmit">
          <p>
            <label for="name">Name:</label>
            <input id="name" v-model="name" placeholder="name">
          </p>

          <p>
            <label for="review">Review:</label>
            <textarea id="review" v-model="review"></textarea>
          </p>

          <p>
            <label for="rating">Rating:</label>
            <select id="rating" v-model.number="rating">
              <option>5</option>
              <option>4</option>
              <option>3</option>
              <option>2</option>
              <option>1</option>
            </select>
          </p>

          <p>
            <input type="submit" value="Submit">
          </p>
          </form>
     `,
      data() {
        return {
          name: null,
          review: null,
          rating: null
        }
      }
    });

We’re using the `v-model` directive to associate form elements with values in
our data. You can see we used the `.number` modifier on the rating to ensure
that the value is interpreted as an integer.

You see that on the form itself we declare a `@submit` event handler that uses
the `.prevent`
[event modifier](https://vuejs.org/v2/guide/events.html#Event-Modifiers) to
prevent the page from reloading (the default browser behavior for submit
buttons) when the button is clicked.

Next we’ll want to add our `onSubmit()` method.

    methods: {
      onSubmit() {
        let productReview = {
          name: this.name,
          review: this.review,
          rating: this.rating
        }
        this.$emit('review-submitted', productReview)
        this.name = null
        this.review = null
        this.rating = null
      }
    }

Our method is packing up the current value into the `productReview` object,
emitting the event to any subscribers outside of the component, then resetting
the form to default values.

We can subscribe to the emitted event on our component like so:

    <product-review @review-submitted="addReview"></product-review>

We could register this in the parent component like so:

    data() {
      return {
        reviews: []
      }
    },
    methods: {
      addReview(productReview) {
        this.reviews.push(productReview)
      }
    }

Now directly above our `product-review` component we can display our reviews:

    <div>
      <h2>Reviews</h2>
      <p v-if="!reviews.length">There are no reviews yet.</p>
      <ul>
        <li v-for="review in reviews">
          <p>{{ review.name }}</p>
          <p>Rating: {{ review.rating }}</p>
          <p>{{ review.review }}</p>
        </li>
      </ul>
    </div>

## Global Event Bus

A common solution for communicating between components (especially when they are
very far apart like a grandparent-grandchild component relationship) is to use
a global event bus.

    var eventBus = new Vue()

In our component, instead of registering an emitter using `this.$emit`, we’ll
instead use `eventBus` like so:

    eventBus.$emit('reviews-submitted', productReview)

Now we’ll setup a listener for this event in the `mounted()` function of the
product component.

    mounted() {
      eventBus.$on('review-submitted', productReview => {
        this.reviews.push(productReview)
      })
    }

`mounted()` is a lifecycle hook that is called once the component has mounted
to the DOM.

This can be a simple solution, using a global event bus, but you’re probably
better off using Vuex.
