---
layout: page
title: VueJS Intro
---

# The Vue Instance

We're going to build a shopping cart system to introduce new Vue concepts. We'll
start with a product page that allows you to choose different colors of "Vue
Mastery Socks", and informs you if a certain color is or is not in stock.

Later we'll add a cart, tabs, a form to add reviews, etc.

Google Chrome also supports a Vue DevTools plugin to see what's going on.

## Begin

We start off with an HTML file that is linked to the `main.js` using a
`<script>` tag.

_index.html_

```html
<!DOCTYPE html>
<html>
<head>
  <title>Product App</title>
</head>
<body>

  <div id="app">
    <h1>Product goes here</h1>
  </div>

  <script src="main.js"></script>
</body>
</html>
```

_main.js_

```javascript
var product = "Socks"
```

We want to get this data into our HTML.

We start by including the Vue.js CDN hosted library into our page.

_index.html_

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Next we change our variable in `main.js` into a Vue app declaration.

_main.js_

```javascript
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks"
  }
})
```

This will cause the Vue.js app to bind to the DIV with ID "app". Within the H1,
we replace 'Product goes here' with the template syntax for the value we want
inserted into the page.

```html
  <div id="app">
    <h1>{{ product }}</h1>
  </div>
```

Now in the page we should see "Socks".

## Expression

When you use the curly braces within the page, this informs Vue.js that an
expression should be evaluated, and the result inserted into the page.

```html
<h1>{{ product }}</h1>
```

Because it's an expression, and not simply just used for outputting values,
you can use them in many ways.

```html
{{ product + '?' }}

{{ firstName + ' ' + lastName }}

{{ clicked ? true : false }}

{{ message.split('').reverse().join('') }}
```

## Reactive

When the value of 'products' is updated within the 'data', it automatically
updates within the DOM of the webpage. This is because Vue is Reactive.

The instances data is linked to every place that data is being referenced. If we
were to place a reference to 'products' in multiple places, both instances would
be updated once the value of 'products' is modified.

```html
<div id="app">
  <h1>{{ products }}</h1>
  <h2>I love my {{ products }}</h2>
</div>
```

You can test this directly within the console of the browser.

```javascript
> app.product = 'Coat'
< "Coat"
> app.product = 'Compass'
< "Compass"
```

## Recap

The Vue instance (`app`) is the heart of the application. It plugs into an
element in the DOM, and that element uses an expression to display that
instances data.

# Attribute Binding

You can download two assets to make this tutorial feel more polished.

* [vmSocks-green-onWhite.jpg]
* [Stylesheet for Vue Mastery's Intro to Vue course]

```html
<link rel="stylesheet" type="text/css" href="style.css">
```

## Navigation Div

Add a navigation bar DIV above our app DIV. This will add a nice gradient bar at
the top of the page.

```html
<div class="nav-bar"></div>

<div id="app">
  ...
</div>
```

Next add an element for our product image, and we'll also wrap our product info
inside of a `product-info` wrapper DIV.

```html
<div id="app">
  <div class="product">
    <div class="product-image">
      <img src="">
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
    </div>
  </div>
</div>
```

Then we'll add the path to the image inside of our Vue app declaration.

```javascript
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image: "./assets/vmSocks-green.jpg"
  }
})
```

## V-Bind Directive

The v-bind directive establishes a bond between the data and the attribute we
want it to be bound to.

```html
<div class="product-image">
  <img v-bind:src="image" >
</div>
```

v-bind dynamically binds an attribute to an expression. The attribute we're
binding to is the `src` attribute of the image tag (`v-bind:src`). We pass an
expression to v-bind for the attribute.

In our example above our expression is `image`. This is similar to
`{{ image }}`. This expression equals the value of `data.image` within our Vue
app, and thus the image path is applied to the image src in the page.

## V-Bind Shorthand

You'll find yourself using v-bind a lot. Because of this there is a shorthand
version of the directive.

```html
<!-- this -->
<img src="photo.jpg" v-bind:alt="description" />

<!-- is the same as this -->
<img src="photo.jpg" :alt="description" />

<a :href="url">...</a>

<div :class="isActive">...</div>

<div :style="someStyles" />

<button :disabled="isDisabled" />
```

# Conditional Rendering

We want to show that the product is either "In Stock" or "Out of Stock"
dynamically.

```html
<p>In Stock</p>
<p>Out of Stock</p>
```

We can use the v-if directive to control when the 'In Stock' message is shown.

_main.js_

```javascript
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image: "./assets/vmSocks-green.jpg",
    inStock: false
  }
})
```

_index.html_

```html
<p v-if="inStock">In Stock</p>
<p v-else>Out of Stock</p>
```

Alternatively we could keep track of our inventory in the data, and update our
expression to render as true if the inventory is above 0.

_main.js_

```javascript
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image: "./assets/vmSocks-green.jpg",
    inventory: 100
  }
})
```

_index.html_

```html
<p v-if="inventory > 0">In Stock</p>
<p v-else>Out of Stock</p>
```

We could take this further and display that we're almost sold out when inventory
reaches 10 or less.

_index.html_

```html
<p v-if="inventory > 10">In Stock</p>
<p v-else-if="inventory < 10 && inventory > 0">Almost sold out!</p>
<p v-else>Out of Stock</p>
```

## v-show

Instead of inserting or removing content within the DOM by using v-if, we can
instead use v-show, which uses `display:none` and `display:block` styling to
show or hide the element within the page.

```html
<p v-show="inStock">In Stock</p>
```

# List Rendering

In our data we've added product details that we want to show up as a list.

_main.js_

```javascript
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image: "./assets/vmSocks-green.jpg",
    inStock: true,
    details: ["80% cotton", "20% polyester", "Gender-neutral"]
  }
})
```

_index.html_

```html
<ul>
  <li v-for="detail in details">{{ detail }}</li>
</ul>
```

## Variants

Let's say we have variants of our product that we want to include in the page.

_main.js_

```javascript
variants: [
  {
    variantId: 2234,
    variantColor: "green"
  },
  {
    variantId: 2235,
    variantColor: "blue"
  }
]
```

_index.html_

```html
<div v-for="variant in variants" :key="variant.variantId">
  <p>{{ variant.variantColor }}</p>
</div>
```

It's important to use the `:key` attribute to ensure that the `v-for` directive
can keep track of the unique items.

# Event Handling

We've added an 'Add to Cart' button to our page, as well as a "Cart" that's
simply a DIV with a paragraph inside of it.

_index.html_

```html
<button>Add to Cart</button>

<div class="cart">
  <p>Cart({{cart}})</p>
</div>
```

_main.js_

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
    ...
    cart: 0
  }
})
```

## V-on

Next we apply the `v-on` directive to specify that a certain expression is
evaluated when the button is clicked on.

_index.html_

```html
<button v-on:click="cart += 1">Add to Cart</button>

<div class="cart">
  <p>Cart({{cart}})</p>
</div>
```

## Methods

Because this expression is so simple, this works fine, however more complicated
applications may need to wrap much functionality in a function.

_index.html_

```html
<button v-on:click="addToCart">Add to Cart</button>
```

_main.js_

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
    ...
    cart: 0
  },
  methods: {
    addToCart: function() {
      this.cart += 1
    }
  }
})
```

Within our function `this.cart` refers to the `cart` property within the `data`
object.

## Variants

The variants of our product now have their own images.

_main.js_

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
    ...
    variants: [
      {
        variantId: 2234,
        variantColor: 'green',
        variantImage: './assets/vmSocks-green.jpg'
      },
      {
        variantId: 2235,
        variantColor: 'blue',
        variantImage: './assets/vmSocks-blue.jpg'
      }
    ]
  },
  methods: {
    ...
  }
})
```

When I hover my mouse over the elements representing one variant or another, I
want the image of the product to change.

Before we simply listed the two variant names within the page using two DIVs.

```html
<div v-for="variant in variants" :key="variant.variantId">
  <p>{{ variant.variantColor }}</p>
</div>
```

We can add a `v-on` directive to our P tag that outputs the color of the
variant, however there is a shorthand version of `v-on`, the `@` symbol.

```html
<div v-for="variant in variants" :key="variant.variantId">
  <p @mouseover="updateProduct(variant.variantImage)">{{ variant.variantColor }}</p>
</div>
```

To correspond to this we'll have to add another method

_main.js_

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
    ...
  },
  methods: {
    ...
    updateProduct = function(variantImage) {
      this.image = variantImage
    }
  }
})
```

## ES6 Shorthand

Instead of using anonymous functions, we can use the ES6 shorthand like so:

_main.js_

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
    ...
  },
  methods: {
    addToCart() {
      this.cart += 1
    }
    updateProduct(variantImage) {
      this.image = variantImage
    }
  }
})
```

Keep in mind that not all browsers may support ES6.

## Other Events

There are other events we can listen for:

```html
<button @click="addToCart">Add to cart</button>

<div @mouseover="updateProduct">Color</div>

<form @submit="addToCart">...</form>

<input @keyup.enter="send">
```

Refer to the [Vue Essentials Cheatsheet] for other modifiers.

# Class & Style Binding

## Style Binding

Instead of printing out the variants color when hovering over the P tag, we want
to use that color in a DIVs background color.

First we add the `color-box` class to our DIV, which gives it a width, height,
and margin. Then we bind the variant color to the style of the DIV.

```html
<div class="color-box"
  v-for="variant in variants"
  :key="variant.variantId"
  :style="{ backgroundColor: variant.variantColor }"
  >
  <p @mouseover="updateProduct(variant.variantImage)">
    {{ variant.variantColor }}
  </p>
</div>
```

Now that our variants are being shown, we no longer need to print out the
variant color text. We can remove the P tag and move the `@mouseover` into the
DIV itself.

```html
<div class="color-box"
  v-for="variant in variants"
  :key="variant.variantId"
  :style="{ backgroundColor: variant.variantColor }"
  @mouseover="updateProduct(variant.variantImage)"
  >
</div>
```

Now when we hover over the blue box, the blue socks appear. If we hover over
the green box, the green socks appear.

## Class Binding

Now that we've learned how to do style binding, let's explore class binding.

When we're out of stock, we should disable the "Add to Cart" button, since there
is no product in stock to add to the cart. We can use the build-in HTML
attribute `disabled` to accomplish this.

```html
<button v-on:click="addToCart" :disabled="!inStock">
  Add to Cart
</button>
```

This disables the functionality of the button, but it doesn't change the
appearance to let the user know that it's disabled.

We can solve this problem by binding a `disabledButton` class to our button
when `inStock` is false.

```html
<button v-on:click="addToCart"
  :class="{ disabledButton: !inStock }"
  :disabled="!inStock">Add to Cart
</button>
```

So here we are using the `v-bind` directive's shorthand `:` to bind to our
button's `class`.

It's also possible to bind an entire class object or array of classes to an
element.

```html
<div :class="classObject"></div>

<div :class="[activeClass, errorClass]"></div>
```

# Computed Properties

Computed properties are properties on the Vue instance that calculate a value
rather than store a value.

Our goal is to display our `brand` and our `product` as one string.

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    brand: 'Vue Mastery',
    ...
    ...
  }
}}
```

So we want to take our new `brand` variable and combine it with our product
name: "Socks", inside of the `h1`.

Since computed properties _calculate_ a value rather than store a static value,
we can use this option in our instance to create a computed property called
`title`.

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    brand: 'Vue Mastery',
    ...
    ...
  },
  computed: {
    title() {
      return this.brand + ' ' + this.product
    }
  }
}}
```

When `title` is called, it will concatenate `brand` and `product` into a new
string, then return it.

```html
  <h1>{{ title }}</h1>
```

Now, if our product was to change, it would say "Vue Mastery Socks" at first,
and then change to "Vue Mastery {{product}}" now.

## Complex Example

Currently, the way we're updating our image is with the `updateProduct` method,
with our `variantImage` passed into the method as an argument. If we want to
change more than just the image, we'll need to refactor this code.

Instead of having `image` in our data, let's replace it with `selectedVariant`,
which we'll initialize as 0.

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    brand: 'Vue Mastery',
    ...
    selectedVariant: 0,
    ...
  },
  computed: {
    title() {
      return this.brand + ' ' + this.product
    }
  }
}}
```

We choose 0 as the default value because it will represent the index of products
in the array.

We can introduce the index value into our `v-for` clause by using
`v-for="(variant, index) in variants"`, and then we'll pass the index into
`updateProduct` instead.

```html
<div class="color-box"
  v-for="(variant, index) in variants"
  :key="variant.variantId"
  :style="{ backgroundColor: variant.variantColor }"
  @mouseover="updateProduct(index)"
  >
</div>
```

Next we'll update our `updateProduct` method to use the index value.

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
  },
  methods: {
    ...
    updateProduct(index) {
      this.selectedVariant = index
    }
  }
})
```

We can insert a `console.log` command into this method to see it at work.

We'll notice that we're getting a warning though:

```
[Vue warn]: Property or method "image" is not defined on the instance but
referenced during render.
```

This is because we deleted `image` and replaced it with `selectedVariant`.
Let's turn `image` into a computed property.

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
  },
  methods: {
    ...
    updateProduct(index) {
      this.selectedVariant = index
    }
  },
  computed: {
    title() {
      return this.brand + ' ' + this.product
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    }
  }
})
```

Now that we have a selected variant, we can access all of the properties. Let's
remove `inStock` variable from our data and turn it into a computed property
that uses the variants quantities.

```javascript
var app = new Vue({
  el: '#app',
  data: {
    product: 'Socks',
    ...
  },
  methods: {
    ...
    updateProduct(index) {
      this.selectedVariant = index
    }
  },
  computed: {
    title() {
      return this.brand + ' ' + this.product
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    }
  }
})
```

Our button is still conditionally turning gray whenever `inStock` is false, just
like before. This is because we're still using `inStock` to bind the
`disabledButton` class to the 'Add to Cart' button.

## Cached Computed Properties

Computed Properties are cached, meaning the result is saved until it's
dependencies change. So when `quantity` changes, the cache will be cleared and
the return value that is calculated will be served from the cache.

Because of this, it's more efficient to use a computed property rather than a
method for an expensive operation.

You also should not be mutating your data model from within a computed property.
You are only computing values based on other values. Keep computed property
functions pure.

# Components

Components are reusable blocks of code that can have both structure and
functionality. They help create a more modular codebase.

_index.html_

```html
    <div class="product">
      <div class="product-image">
        <img :src="image" />
      </div>

      <div class="product-info">
        <h1>{{ product }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{ shipping }}</p>

        <ul>
          <li v-for="detail in details">{{ details  }}</li>
        </ul>

        <div class="color-box"
          v-for="(variant, index) in variants"
          :key="variant.variantId"
          :style="{ backgroundColor: variant.variantColor }"
          @mouseover="updateProduct(index)"
        >
        </div>

        <button v-on:click="addToCart"
          :disabled="!inStock"
          :class="{ disabledButton: !inStock }"
        >
          Add to cart
        </button>

        <div class="cart">
          <p>Cart({{ cart }})</p>
        </div>
      </div>
    </div>
```

_main.js_

```javascript
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    brand: "Vue Mastery",
    selectedVariant: 0,
    details: ["80% cotton", "20% polyester", "Gender-neutral"],
    variants: [
      {
        variantId: 2234,
        variantColor: "green",
        variantImage: "./assets/vmSocks-green.jpg",
        variantQuantity: 10
      },
      {
        variantId: 2235,
        variantColor: "blue",
        variantImage: "./assets/vmSocks-blue.jpg",
        variantQuantity: 0
      }
    ],
    cart: 0
  },
  methods: {
    addToCart() {
      this.cart += 1
    },
    updateProduct(index) {
      this.selectedVariant = index
      console.log(index)
    }
  },
  computed: {
    title() {
      return this.brand + " " + this.product
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    }
  }
})
```

## Problem

In a Vue application, we don't want all of our data, methods, computed
properties, etc. living on the root instance. Over time, that would become
unmanageable. We want to break up our code into modular pieces so that it's
easier and more flexible to work with.

## Solution

We'll take the bulk of our code and move it over into a new component for our
product.

We register the component like this:

```javascript
Vue.component("product", {})
```

The first argument is the name we choose for the component, and the second is
an options object.

In the Vue instance we used the `el` property to plug into an element in the
DOM. For a component we use the `template` property to specify its HTML. Inside
that options object, we'll add our template.

```javascript
Vue.component("product", {
  template: `
    <div class="product">
      ... // Our product HTML all goes in here
    </div>
  `
})
```

There are other ways to create a template in Vue, but right now we'll use a
template literal, with back ticks.

If all of our template code was not nested within one element, the div with the
class of 'product', we would have received this error:

`Component template should contain exactly one root element`

It won't work if there are more than one elements in the template.

```javascript
template: `
    <h1>I'm a single element!</h1>
    <h2>Not anymore.</h2>
  `
```

Now that our template is complete with the product HTML, we'll add our data,
methods, and computed properties from the root instance into our new component.

```javascript
Vue.component("product", {
  template: `
    <div class="product">
      ... // Our product HTML all goes in here
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
```

This looks identical to our app structure. The only difference is that `data` is
now a function that returns an object.

Because we want to reuse components, we need to ensure that a separate instnace
of our data is created for each component. Returning an object like this ensures
that the object is returned by value instead of by reference.

After moving our product-related code into it's own `product` component, our
root instance looks like this:

_main.js_

```javascript
var app = new Vue({
  el: "#app"
})
```

Now we just simply tuck our `product` component within our index.html.

_index.html_

```html
  <div id="app">
    <product></product>
  </div>
```

In the Vue dev tools we'll see that we have the `Root` and then below that the
`Product` component.

If we add more `Product` components, we'll see our interface show up on the page
repeated that many more times.

```html
  <div id="app">
    <product></product>
    <product></product>
    <product></product>
  </div>
```

## Problem

Often in an application, a component will need to receive data from its parent.
In this case, the parent of our `product` component is the root instance itself.

Let's say our root instance has some user data on it, indicating that the user
is a premium account holder.

_main.js_

```javascript
var app = new Vue({
  el: "#app",
  data: {
    premium: true
  }
})
```

Let's say this provides the user free shipping, which means that our `product`
component needs to operate differently for premium users. How do we send this
to the child `product` component?

## Solution

In Vue, we use **props** to handle this kind of downward data sharing. Props
are essentially variables that are waiting to be filled with the data its
parent sends down into it.

_main.js_

```javascript
Vue.component("product", {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  }
  /// ... Template, Data, Methods,
  /// ... Computed Properties below
})
```

You'll notice there is validation built into the structure of defining the
props, making it required and specifying that it should be a Boolean value.

In our template, let's make an element to display our prop to make sure it's
being passed down correctly.

```javascript
  template: `
    <div class="product">
      ...
      <p>User is premium: {{ premium }}</p>
      ...
    </div>
  `,
```

To pass the `premium` variable into our `product` component, we add it using a
directive like so:

```html
  <div id="app">
    <product :premium="premium"></product>
  </div>
```

Our `product` component was given a prop, or _custom attribute_, called
`premium`. We are binding that custom attribute `:` to the `premium` that lives
in our instance's data.

Now that we're succesfully passing data into our component, let's use that data
to affect what we show for shipping.

Let's use our `premium` prop within a computed property called shipping.

_main.js_

```javascript
    shipping() {
      if (this.premium) {
        return "Free"
      } else {
        return 2.99
      }
    }
```

Further up in our template, we can simply output the return value of the
shipping method.

_main.js_

```javascript
template: `
    ...
    <p>Shipping: {{ shipping }}</p>
    ...
  `
```

## Recap

* Components are blocks of code, grouped together within a custom element
* Components make applications more manageable by breaking up the whole into reusable parts that have their own structure and behavior
* Data on a component must be a function
* Props are used to pass data from parent to child
* We can specify requirements for the props a component is receiving
* Props are fed into a component through a custom attribute
* Props can be dynamically bound to the parent's data
* Vue Dev tools provide helpful insight about your components

# Communicating Events

We know how to create components and pass data to them via props, but what about
passing information back up? Let's learn to communicate from a child component
up to its parent.

Our goal is to have our `product` component tell it's parent, the root instance,
that an event has occurred, and send data along with that event notification.

```html
<div id="app">
  <product :premium="premium"></product>
</div>
```

```javascript
Vue.component("product", {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  template: `
  <div id="product">
  
    <div class="product-image">
    <img :src="image" />      
    </div>
    
    <div class="product-info">
      <div class="cart">
        <p>Cart({{ cart }})</p>
      </div>
    
      <h1>{{ title }}</h1>
      <p>Shipping: {{ shipping }}</p>
      
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>
      
      <h2>Details</h2>
      <ul>
        <li v-for="detail in details">{{ detail }}</li>
      </ul>
      <h3>Colors:</h3>
      <div v-for="variant in variants" :key="variant.variantId">
        <div class="color-box" :style="{ backgroundColor: variant.variantColor }" @mouseover="updateProduct(index)"></div>
      </div>
      <button :class="{ disabledButton: !inStock }" v-on:click="addToCart" :disabled="!inStock">Add to Cart</button>
    </div>
  </div>
  `,
  data() {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      selectedVariant: 0,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          varaintId: 1,
          variantQuantity: 15,
          variantColor: "green",
          variantImage: "./assets/vmSocks-green.jpg"
        },
        {
          variantId: 2,
          variantQuantity: 0,
          variantColor: "blue",
          variantImage: "./assets/vmSocks-blue.jpg"
        }
      ],
      cart: 0
    }
  },
  methods: {
    addToCart() {
      this.cart += 1
    },
    updateProduct(index) {
      this.selectedVariant = index
    }
  },
  computed: {
    title() {
      return this.brand + " " + this.product
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      if (this.quantity > 0) {
        return true
      } else {
        return false
      }
    },
    shipping() {
      if (this.premium) {
        return "Free"
      } else {
        return 2.99
      }
    }
  }
})

var app = new Vue({
  el: "#app",
  data: {
    premium: true
  }
})
```

Now that `product` is its own component, it doesn't make sense for our cart to
live within `product`. It would get messy if each product had it's own cart to
keep track of. We want it to live in theroot instance, and have `product`
communicate up to the cart when its "Add to Cart" button is pressed.

## Moving Cart to Root

Let's move the cart back to our root instance.

```javascript
var app = new Vue({
  el: "#app",
  data: {
    premium: true,
    cart: 0
  }
})
```

```html
<div id="app">
  <div class="cart">
    <p>Cart({{ cart }})</p>
  </div>

  <product :premium="premium"></product>
</div>
```

Obviously after making this adjustment, the "Add to Cart" button becomes broken.

To fix this, let's update our component's `addToCart` method.

Currently our method does the following:

```javascript
addToCart() {
  this.cart += 1
},
```

Let's update it to do this instead:

```javascript
addtoCart() {
  this.$emit('add-to-cart')
},
```

`this.$emit` informs triggers an event by the name of 'add-to-cart'. Right now
we don't have a listener for the event.

Let's add it to our component as an attribute:

```html
<product :premium="premium" @add-to-cart="updateCart"></product>
```

Previously we used `:premium` to funnel data into `product`, passing data down
into it. `@add-to-cart` is like a radio that can receive the event emission from
when the "Add to Cart" button is clicked, and blast the announcement when the
click event happens, triggering the `updateCart` method that lives in the root
instance.

```javascript
var app = new Vue({
  el: "#app",
  data: {
    premium: true,
    cart: 0
  },
  methods: {
    updateCart() {
      this.cart += 1
    }
  }
})
```

In a real application it's not helpful to know that a product was aded to the
cart. We would need to know _which_ product was just added.

Let's update our notification so that it passes this data up along with the
event announcement.

Within our component, let's update `addToCart`.

```javascript
addToCart() {
  this.$emit('add-to-cart', this.variants[this.selectedVariant].variantId)
},
```

In our root instance, let's push the ID into our cart array.

```javascript
var app = new Vue({
  el: "#app",
  data: {
    premium: true,
    cart: 0
  },
  methods: {
    updateCart(id) {
      this.cart.push(id)
    }
  }
})
```

This results in the following being shown on our page: `Cart([ 2234 ])`

We can fix this by updating our template for the display of the cart.

```html
  <p>Cart({{ cart.length }})</p>
```

## Recap

* A component can let its parent know that an event has happened with `$emit`
* A component can use an event handler with the `v-on` directive (`@` for short) to listen for an event emission, which can trigger a metho on the parent
* A component can `$emit` data along with the announcement that an event has occurred
* A parent can use data emitted from its child

# Forms

Let's create a form that allows users to submit a review of a product, but only
if they have filled out the required fields.

Let's create a new component for our form, which will be called `product-review`
since it's the component that collects product reviews. It will be nested within
our `product` component.

Let's register our new component.

```javascript
Vue.component("product-review", {
  template: `
      <input>
    `,
  data() {
    return {
      name: null
    }
  }
})
```

So far our component has an input field, and returns a `name` in our data.

Earlier we learned about binding with `v-bind`, but that was only for one-way
binding _**from the data to the template**_. Now we want whatever the user
inputs to be bound to `name` in our data, in effect
_**from the template to the data**_.

## V-Model directive

Vue's `v-model` directive gives us this two-way binding. When something new is
entered into the input, the data changes. Whenever the data changes, anywhere
using that data will also update.

Let's add `v-model` to our input, and bind it to `name` in our component's data.

```html
<input v-model="name">
```

So far so good. Let's add a complete form to our template.

```html
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
```

You can see above that we've added `v-model` binding for our 3 form fields
(`input`, `textarea`, and `select`. We're also using the `.number` modifier on
the `select` (more explained below).

At the top of our form you can see that our `onSubmit` method will be triggered
when the form is submitted. You may notice that it's using `.prevent`. This is
an event modifier used to prevent the submit event from reloading our page.

```javascript
Vue.component("product-review", {
  template: `
      <input>
    `,
  data() {
    return {
      name: null,
      review: null,
      rating: null
    }
  },
  methods: {
    onSubmit() {
      let productReview = {
        name: this.name,
        review: this.review,
        rating: this.rating
      }
      this.$emit("review-submitted", productReview)
      this.name = null
      this.review = null
      this.rating = null
    }
  }
})
```

When our method is submitted, it emits our event with the review object
included, then clears our form fields.

We now need to listen for that announcement on `product-review`.

```html
  <product-review @review-submitted="addReview"></product-review>
```

This will ensure that when the "review-submitted" event happens, the product's
`addReview` method will run.

```javascript
  addReview(productReview) {
    this.reviews.push(productReview)
  }
```

In our `product` component we'll need to add a `reviews` array.

```javascript
reviews: []
```

Once this is done, our form elements are bound to the `product-review`
components data, which is used to create a `productReview` object that is sent
up to our `product` when the form is submitted, and associated with our
product's `reviews` array.

## Displaying Reviews

Now we just need to display our reviews. We'll add these in our product
component, just above where the `product-review` component is nested.

```html
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
```

Here, we are creating a list of our reviews with `v-for` and printing them out
using dot notation, since each review is an object.

## Form Validation

Often with forms we have required fields. We won't want our user to be able to
submit the review if the review body field is empty.

HTML5 provides a `required` attribute:

```html
<input required />
```

This provides form validation handled natively in the browser, instead of your
code. This might not always be the best for your application though, because
you don't have control over how it works.

## Custom Form Validation

In our `product-review` component's data we'll add an array for errors:

```javascript
  data() {
    return {
      name: null,
      review: null,
      rating: null,
      errors: []
    }
  }
```

We want to add an error into that array whenever one of our fields is empty.

```javascript
  onSubmit() {
    if (this.name && this.review && this.rating) {
      let productReview = {
        name: this.name,
        review: this.review,
        rating: this.rating
      }
      this.$emit('review-submitted', productReview)
      this.name = null
      this.review = null
      this.rating = null
    } else {
      if (!this.name) this errors.push("Name required.")
      if (!this.review) this errors.push("Review required.")
      if (!this.rating) this errors.push("Rating required.")
    }
  }
```

Now if we have all our data, the review is submitted, otherwise the errors
are populated.

To display these errors, we do the following in our template:

```html
  <p v-if="errors.length">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
  </p>
```

## Using `.number`

Using the [.number](https://vuejs.org/v2/guide/forms.html#number) modifier on
`v-model` is a helpful feature, but please be aware there is a known bug with
it. If the value is blank, it will turn back into a string. The
[Vue.js Cookbook] offers the solution to wrap that data in the `Number` method,
like so:

```javascript
Number(this.myNumber)
```

## Recap

* We can use the `v-model` directive to create two-way bindings on form elements
* We can use the `.number` modifier to tell Vue to cast that value as a number (even though there is a bug with it)
* We can use the `.prevent` event modifier to stop the page from reloading when the form is submitted
* We can use Vue to do fairly simple custom form validation

# Tabs

Now we'll add tabs to display our reviews and the review form separately. This
will be nicer than displaying the reviews and form on top of each other.

We'll start by adding a `product-tabs` component, which will be nested at the
bottom of our `product` component.

```javascript
Vue.component("product-tabs", {
  template: `
        <div>
          <span class="tab" v-for="(tab, index) in tabs" :key="index">{{ tab }}</span>
        </div>
      `,
  data() {
    return {
      tabs: ["Reviews", "Make a Review"]
    }
  }
})
```

In our data the `tabs` array contains strings that we're using as the titles of
each tab. We're creating a `span` for each string in our `tabs` array.

We want to know which tab is currently selected, so in our data we'll add
`selectedTab` and dynamically set that value with an event handler, setting it
equal to the tab that was just clicked.

```javascript
Vue.component("product-tabs", {
  template: `
        <div>
          <span class="tab"
            v-for="(tab, index) in tabs"
            @click="selectedTab = tab"
            :key="index">{{ tab }}</span>
        </div>
      `,
  data() {
    return {
      tabs: ["Reviews", "Make a Review"],
      selectedTab: "Reviews"
    }
  }
})
```

## Class Binding for Active Tab

We should give our users some visual feedback, so that they know which tab is
selected. We can do this by adding a class binding to our span.

```javascript
template: `
    <div>
      <span class="tab"
        :class="{ activeTab: selectedTab === tab}"
        v-for="(tab, index) in tabs"
        @click="selectedTab = tab"
        :key="index">{{ tab }}</span>
    </div>
    `
```

When the current tab is the selected tab, this class will be applied to the
`span`.

Let's build out our template now, and add the content we want to display when
either tab is clicked.

Let's first move our reviews from the `product` component into the template of
our `product-tabs` component.

```javascript
template: `
      <div>
      
        <div>
          <span class="tab" 
                v-for="(tab, index) in tabs"
                @click="selectedTab = tab"
          >{{ tab }}</span>
        </div>
        
        <div> // moved here from where it was on product component
            <p v-if="!reviews.length">There are no reviews yet.</p>
            <ul v-else>
                <li v-for="(review, index) in reviews" :key="index">
                  <p>{{ review.name }}</p>
                  <p>Rating:{{ review.rating }}</p>
                  <p>{{ review.review }}</p>
                </li>
            </ul>
        </div>
        
      </div>
    `
```

The `reviews` lives in our `product` component, so we'll need to send that data
into our `product-tabs` component via props.

```javascript
  props: {
    reviews: {
      type: Array,
      required: false
    }
  }
```

Next we need to pass in `reviews` on our `product-tabs` component itself, so
it's always receiving the latest `reviews`.

```html
  <product-tabs :reviews="reviews"></product-tabs>
```

Now we'll add our review form to the "Make a Review" tab.

We'll move the `product-review` component from the `product` component and place
it into the template of our `tabs` component, below the div for reviews.

```html
  <div>
    <product-review @review-submitted="addReview"></product-review>
  </div>
```

## Conditionally Displaying with Tabs

Now that we have our template setup, we want to conditionally display either of
the two tabs, depending on which is clicked.

Since we're using the `selectedTab` to track which is seleted, we can use this
with `v-show` to display the selected content.

```javascript
template: `
    <div>
    
      <div>
        <span class="tab" 
          v-for="(tab, index) in tabs"
          @click="selectedTab = index"
        >{{ tab }}</span>
      </div>
      
      <div v-show="selectedTab === 'Review'"> // displays when "Reviews" is clicked
        <p v-if="!reviews.length">There are no reviews yet.</p>
        <ul>
          <li v-for="review in reviews">
            <p>{{ review.name }}</p>
            <p>Rating:{{ review.rating }}</p>
            <p>{{ review.review }}</p>
          </li>
        </ul>
      </div>
      
      <div v-show="selectedTab === 'Make a Review'"> // displays when "Make a Review" is clicked
        <product-review @review-submitted="addReview"></product-review>        
      </div>
  
    </div>
  `
```

Now our tabs work to display the content we want, but there is a warning in our
console.

`[Vue warn]: Property or method "addReview" is not defined on the instance but referenced during render.`

This is a method that lives on our `product` component, and it's supposed to be
triggered when our `product-review` component (which is a child of `product`)
emits the `review-submitted` event.

```html
  <product-review @review-subimtted="addReview"></product-review>
```

Our `product-review` component is a child of our `tabs` component, which is a
child of the `product` component. This makes `product-review` a _grandchild_ of
`product`.

## Refactoring Our Code

A common solution for communicating from a grandchild up to a grandparent, or
for communicating between components, is to use what's called a global event
bus.

This is essentially a channel through which you can send information amongst
your components, and it's just a Vue instance, without any options passed into
it. Let's create our event bus now.

```javascript
var eventBus = new Vue()
```

Just imagine this as a literal bus, and it's passengers are whatever you're
sending at the time. In our case, we want to send an event emission. We'll use
this bus to communicate from our `product-review` component and let our
`product` component know the form was submitted, and pass the form's data up
to `product`.

In our `product-review` component, instead of saying:

```javascript
this.$emit("review-submitted", productReview)
```

We will instead use the `eventBus` to emit the event.

```javascript
eventBus.$emit("review-submitted", productReview)
```

Now we don't need to listen for the `review-submitted` event on our product
component.

```html
  <product-review></product-review>
```

Now, inside of our `product` component, we can delete our `addReview` method,
and instead listen for that event using this code:

```javascript
eventBus.$on("review-submitted", productReview => {
  this.reviews.push(productReview)
})
```

## Why the ES6 Syntax

We're using the ES6 arrow function syntax here because an arrow function is
bound to its parent's context. When we say `this` inside the function, it
refers to `this` component/instance. You can write this code without an arrow
function, you'll just have to manually bind the component's `this` to that
function like so:

```javascript
eventBus.$on(
  "review-submitted",
  function(productReview) {
    this.reviews.push(productReview)
  }.bind(this)
)
```

We're almost done. We just need to put this code somewhere, like in the
`mounted` method. This is a lifecycle hook, a function called once the component
has mounted to the DOM. Now, once `product` has mounted, it will be listening
for the `review-submitted` event. When it hears it, it will add the new
`productReview` to its data.

## A Better Solution

Using an event bus is a common solution that you'll see in others' Vue code.
This isn't the best solution for communicating amongst components within your
app.

As your app grows, you'll want to implement Vue's own state management solution:
[Vuex](https://vuex.vuejs.org/)

This is a state-management pattern and library. You'll learn more in the
[Real World Vue](https://www.vuemastery.com/courses) course.

[vmsocks-green-onwhite.jpg]: https://www.dropbox.com/s/9zccs3f0pimj0wj/vmSocks-green-onWhite.jpg?dl=0
[stylesheet for vue mastery's intro to vue course]: https://gist.github.com/atomjar/67db5aa9b7b9013dcf0d91c91f54e1a9
[vue essentials cheatsheet]: https://www.vuemastery.com/pdf/Vue-Essentials-Cheat-Sheet.pdf
[vue.js cookbook]: https://vuejs.org/v2/cookbook/form-validation.html#Another-Example-of-Custom-Validation
