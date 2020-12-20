---
layout: post
published: true
title: How to Insert Special Entities in React
description: Generating UTF Characters with ES6
date: 2020-12-09 14:14:10 -0700
comments: true
categories:
  - javascript
tags:
  - special entities
  - es6
  - react
---

## Generating Characters

You can generate UTF-16 characters in ES6 using [String.fromCharCode()][1],
which takes one or more UTF-16 codes as input and returns the unicode
character(s) specified.

```javascript
// hex code input
let copyright = String.fromCodePoint(0x00A9);
// "©"

// decimal code input
let copyright = String.fromCodePoint(0169);
// "©"
```

<!--more-->

Here is a list of the codes for common characters I've seen used in websites:

| Label                  | Character     | Decimal | Hex      | HTML           |
|------------------------|---------------|---------|----------|----------------|
| Copyright              | &copy;        | 169     | 0x000A9  | `&copy;`       |
| Registered             | &reg;         | 174     | 0x000AE  | `&reg;`        |
| Trademark              | &trade;       | 8482    | 0x02122  | `&trade;`      |
| Euro                   | &euro;        | 8364    | 0x020AC  | `&euro;`       |
| Dollar                 | $      | 36      | 0x00024  | `&dollar;`     |
| Ohm                    | Ω         | 8486    | 0x02126  | `&ohm;`        |
| Degree                 | &deg;         | 176     | 0x000B0  | `&deg;`        |
| Micro                  | &micro;       | 181     | 0x000B5  | `&micro;`      |
| Em Dash                | &mdash;       | 8212    | 0x201    | `&mdash;`      |
| Non Breaking Space     | &nbsp;        | 160     | 0x000A0  | `&nbsp;`       |

If you need a symbol not listed above, see the [HTML5 Character Reference][2],
or the [List of Unicode Characters][3].

## Using with React Components

After generating the character as a constant, just insert it into your JSX as
needed.

```jsx
import React from 'react';

const copyrightMessage = () => {

  const copyright = String.fromCodePoint(0x00A9);
  const year = '2020';
  const companyName = 'Company Name';

  return (
    <span>
     {`${copyright} ${year} ${companyName}`}
    </span>
  );
};

export default copyrightFooter;
```

See also [JSX Gotchas][4] for more approaches.

[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode
[2]: https://dev.w3.org/html5/html-author/charref
[3]: https://en.wikipedia.org/wiki/List_of_Unicode_characters
[4]: https://shripadk.github.io/react/docs/jsx-gotchas.html
