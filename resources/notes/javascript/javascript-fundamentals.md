---
layout: page
title: JavaScript Fundamentals
---

## JavaScript

### Basic Operators

JavaScript is designed for interactive webpages, however this tutorial will be
primarily focused on the fundamental building blocks of the JavaScript language
itself. This will better serve as a prerequisite to working with frameworks
such as jQuery.

## Common Operators used in JavaScript syntax

Addition

```javascript
>> 6 + 4
-> 10
```

Subtraction

```javascript
>> 9 - 5
-> 4
```

Multiplication

```javascript
>> 3 * 4
-> 12
```

Division

```javascript
>> 12 / 4
-> 3
```

Modulus - Remainder after division

```javascript
>> 43 % 10
-> 3
```

## Order of Operations: PEMDAS

JavaScript recognizes the standard order of operations in mathematics. The acronym PEMDAS.

- **P**arenthesis
- **E**xponents
- **M**ultiplication
- **D**ivision
- **A**ddition
- **S**ubtraction

```javascript
// The parenthesis will be evaluated first
>> (5 + 7) * 3

// Then multiplied
>> 12 * 3
-> 36

>> (3 * 4) + 3 - 12 / 2

// Parenthesis first
>> 12 + 3 - 12 / 2

// Next Division
>> 12 + 3 - 6

// Next Addition
>> 15 - 6

// Finally Subtraction
-> 9
```

## Modulus is contained within the Multiplication (M of PEMDAS) hierarchical level

```javascript
>> 4 + (8 % (3 + 1))
```

### Inner parenthesis first

```javascript
>> 4 + (8 % 4)
>> 4 + (0)
-> 4
```

## Comparators

Comparators allow us to compare values, returning a boolean value (true or false)

```javascript
# Greater Than
>> 6 > 4
-> true

# Less Than
>> 9 < 5
-> false
```

To compare equality in JavaScript, it requires a special syntax. We use a double equal sign to receive a boolean value.

```javascript
// Is equal
>> 3 == 4
-> false

// Not equal
>> 12 != 4
-> true

// Greater or equal
>> 8 >= -2
-> true

// Less or Equal
>> 10 <= 10
-> true
```

## Strings

JavaScripts way of handling, storing, and processing flat text.

```javascript
>> "Raindrops On Roses"
-> "Raindrops On Roses"

>> "Whiskers on Kittens"
-> "Whiskers on Kittens"
```

### Concatenation

```javascript
>> "Raindrops On Roses" + "And" + "Whiskers on Kittens"
-> "Raindrops On Roses And Whiskers On Kittens"
```

Concatenation works with numbers and expressions too:

```javascript
>> "The meaning of life is" + 42
-> "The meaning of life is 42"

>> "Platform " + 9 + " and " + 3/4
-> "Platform 9 and 0.75"
```

### Special Characters

Some characters need backslash notation to “escape” the characters.

```javascript
>> "Flight #:\t921\t\tSeat:\t21c"
-> "Flight #:   921   Seat:      21c
```

Use backslash to escape double quotes

```javascript
>> "Login Password:\t\t\"C3P0R2D2\""
-> "Login Password:        "C3P0R2D2"
```

Double backslash for the backslash itself

```javascript
>> "Origin\\Destination:\tOrlando (MCO) \\ London(LHR)"
-> "Origin\Destinaton:    Orlando (MCO) \ London(LHR)"
```

Newline character - \n

### String Comparisons

Check for matching strings and alphabetical ordering.

```javascript
>> "The Wright Brothers" == "The Wright Brothers"
-> true

>> "The Wright Brothers" == "Super Mario Brothers"
-> false

# Javascript is case sensitive
>> "Jason" != "jason"
-> true
```

### String Length

```javascript
>> "antidisestablishmentarianism".length
-> 28

# Spaces are contained also
>> "Hello Govna!".length
-> 12
```

## Variables

JavaScript uses variables to store and manage data.

```javascript
>> var trainWhistles = 3
>> trainWhistles
-> 3
```

### Naming Variables

**Rules and Regulations**

- No spaces in variable names - `var my name = 'jason';`
- No digits prefixing the names - `var 3blindmice = [];`
- Underscores are okay - `var scored_is_fine =` ` '``3``'``; `
- Dollar signs are okay - `var get$ =` ` '``123``'``; ` (why would you though?)

**CamelCase**
Begins with lowercase, words capitalized

    var goodNameBro = 'my name';
    var mortalKombat2 = []; // uses number at end

### Changing Variable Contents

The `var` keyword isn’t needed after the variable has already been declared in memory

    >> var trainWhistles = 3
    >> trainWhistles = 9

    >> trainWhistles = trainWhistles + 3
    >> trainWhistles
    -> 12

    # similar operation
    >> trainWhistles += 3
    -> 15

    >> trainWhistles = trainWhistles * 2
    -> 30

    # again, similar yet shorter operation
    >> trainWhistles *= 2
    -> 60

### Using Variables

    >> trainWhistles = 3
    >> "All of our trains have " + trainWhistles + " whistles!"
    -> "All of our trains have 3 whistles!"

    >> "But the Pollack 9000 has " + (trainWhistles * 3) + "!"
    -> "But the Pollack 9000 has 9!"

### Incrementing and Decrementing

    >> trainWhistles = 3
    >> trainWhistles++
    >> trainWhistles
    -> 4

    >> trainWhistles = 3
    >> trainWhistles--
    >> trainWhistles
    -> 2

### Variable Exploration

JavaScript can store anything in variables like strings

    >> var welcome = "Welcome to the JavaScript Express Line"
    >> var safetyTip = "Look both ways before crossing the tracks"
    >> welcome + "\n" + safetyTip
    -> "Welcome to the JavaScript Express Line
    Look both ways before crossing the tracks"

**Using Variable Names with Strings**

    >> var longString = "I wouldn't want to retype this String every time."
    >> longString.length
    -> 49

**Compare using the length operator**

    longWordOne.length > longWordTwo.length

## Finding Specific Characters Within Strings

    >> var sentence = "Antidisestablishmentarianism"
    >> sentence.length
    -> 43

    >> sentence.charAt(11)
    -> "b"

**Variables Help Organize Data**

    >> var trainsOperational = 8
    >> var totalTrains = 12
    >> var operatingStatus = " trains are operational today."
    >> trainsOperational + " of " + totalTrains + operationalStatus
    -> "8 of 12 trains are operational today."

## Files

### Script Tags

To run JavaScript within an HTML file

    <!-- index.html ->
    <html>
    <head>
      <script src="trains.js"></script>
    </head>
    <body>
      <h1>JavaScript Express!</h1>
    </body>
    </html>


    // trains.js
    "Train #" + 1 + " is running."
    "Train #" 2 1 + " is running."
    "Train #" 3 1 + " is running."

This will result in an error. The compiler doesn’t understand what we’ve placed in our file the same way that the console does.

We need to use semi-colons at the end of each statement.

    var totalTrains = 12;
    var trainsOperational = 8;

    console.log("There are " + trainOperational + " running trains.";
    >> "There are 8 running trains."

## Loops

### While Loop

Runs the code as long as the boolean expression evaluates to true.

    while (1 == 1) {
      // do code
    }

    var number = 1;
    while (number <= 5) {
      console.log(number);
      number++;
    }

    var trainNumber = 1;
    while (trainNumber <= 8) {
      console.log("Train #" + trainNumber + " is running");
      trainNumber++;
    }

### For Loop

    for (initialize; condition; do after each loop) {
      // code
    }

    for (var trainNumber = 1; trainNumber <= trainsOperational; trainNumber++) {
      console.log("Train #" + trainNumber + "is running");
    }

    var totalTrains = 12;
    var trainsOperational = 8;

    var trainNumber = 1;
    while (trainNumber <= trainsOperational) {
     // ...
    }
    for (var stoppedTrain = trainsOperational + 1; stoppedTrain <= totalTrains; stoppedTrains++) {
      // ...
    }

## Conditional Statements

### If-Else

    var totalTrains = 12;
    var trainsOperational = 8;

    for (var trainNumber = 1; trainNumber <= totalTrains; trainNumber++) {
      if (trainNumber <= trainsOperational) {
        console.log("Train #" + trainNumber + "is running");
      } else {
        console.log("Train #" + trainNumber + "is not operational");
      }
    }

### Else-If

    if (*some condition is true) {
      // do this code
    } else if (*some other condition is true*) {
      // do something else
    } else {
      // in all other cases, do this instead
    }

### Checking Multiple Conditions

    if (trainNumber <= operationalTrains) {
      console.log("Train #" + trainNumber + "is running.");
    } else if (trainNumber == 10) {
      console.log("Train 10 begins running at noon.");
    } else {
      console.log("Train #" + trainNumber + "is not operational.");
    }

### Nested Conditionals

    if (*is a square*) {
      if (*it's big*) {
        // make it red
      } else {
        // make it blue, because it must be a small square
      }
    } else {
      // since it's not a square, it must be a circle
      // make it purpose
    }

### Complex Conditionals

`&&` - Binary AND - Returns `true` if both values are true
`||` - Binary OR - Returns `true` if either value is true

    # Both are true
    >> (11 >= 11) && (-7 < 6)
    -> true

    # 9 is not less than 4
    >> (2 >= 0) && (9 < 4)
    -> false

## References

- [CanIUse.com](https://caniuse.com/) - JavaScript browser support
