---
layout: page
title: ES2015
---

[Javascript Fundamentals for ES6](https://app.pluralsight.com/library/courses/javascript-fundamentals-es6/table-of-contents)

## Table of Contents

- [Overview](#overview)
- [Variables and Parameters](#variables-and-parameters)
- [Classes](#classes)
- [Functional Programming](#functional-programming)
- [Built-In Objects](#built-in-objects)
- [Asynchronous Development in ES6](#asynchronous-development-in-es6)
- [Objects in ES6](#objects-in-es6)
- [Modules](#modules)
- [Using ES6 Today](#using-es6-today)

## Overview

Javascript is an implementation of ECMAScript. The ES6 specification
that defines the language was in draft status, with release in June 2015.

ES6 introduces an extensive amount of new syntax to the language.
The last time the specification tried to add this broadly to the language
was with ECMAScript 4, but that project was abandoned.

Some modifications were made with [ECMAScript 5 / ES2009](https://www.w3schools.com/js/js_es5.asp).

### Try It Yourself

You can use [Plunker](http://plnkr.co/) to test ES6 code. You can also do the
same directly in the Chrome JavaScript console using COMMAND + OPTION + J.

You can also go to [jsconsole](https://jsconsole.com/)

### Compatibility

- [ES6 Compatibility Table](http://kangax.github.io/compat-table/es6/)

### The Repository

- [ES6FundamentalsCourseFiles](https://github.com/joeeames/ES6FundamentalsCourseFiles)

### Get Started

## Variables and Parameters

### Let

Let gives us true block-scoped variables in JavaScript. Let is the new `var`.

```javascript
let x = 2
let y = ((3)[(x, y)] = [y, x])

expect(x).toBe(3)
expect(y).toBe(2)
```

### Rest Parameters

### Default Parameters

### Destructuring Assignments

## Classes

We've always had the ability to create objects in JavaScript with properties
and methods, however the new class syntax allows us to do that in familiar way,
especially for those that have used Python, Java, C++, or Ruby.

A class defines a blueprint for constructing objects, includes the pieces that
define the state, construction logic, and even setup inheritance relationships
between objects.

## Functional Programming

There are some new functional programming capabilities with JavaScript. It has
always been a very functional language.

### Arrow Function

Arrow functions allow you to use a terse syntax to define functions. In many
cases you do not need a return statement or curly braces.

```javascript
let add = (x, y) => x + y

expect(add(3, 5)).toBe(8)
```

```javascript
let numbers = function*(start, end) {
  for (let i = start; i <= end; i++) {
    console.log(i)
    yield i
  }
}
```

### Iterators

#### Generator Functions

Generator functions can create iterators.

### List Comprehension Syntax

## Built-In Objects

### Set and Map collections

New data structures we can use in JavaScript, have memory friendly counterparts:
the WeakMap and the WeakSet.

### New APIs

Objects, arrays, numbers, and more.

## Asynchronous Development in ES6

Promises, an object that will promise to give you the result of some asynchronous
operation in the future. There have been unofficial standards for the Promise
API, but now it has been standardized.

## Objects in ES6

New APIs, new methods. New Metaprogramming capabilities.

## Modules

JavaScript has lacked a modules system, where unofficial community standards
have been in place such as CommonJs modules, or the Asynchronous Module
Definition (AMD) standard.

## Using ES6 Today

How people are using ES6 today. Many teams are using tools like Traceur to
transpile ES6 code to ES5 code.
