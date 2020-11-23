---
layout: page
title: Regular Expressions
---

## Regular Expressions

Breaking the Ice with Regular Expressions

## Section 1 - The String Story

### Why Regular Expressions

Let’s say you want to validate that a phone number for someone in Orlando is
entered into a form. You’d want to validate that the area code is 407 or 321,
and optionally ensure that dashes are placed after the area code and exchange
number.

It would require a lot of normal code to accomplish this, but only one call to
do so with a regular expression.

### Understanding a Regular Express

A regular expression involves a subject string (subject) where we are looking
for a match, and a regular expression (regex) itself, which is a set of
characters that represents rules for searching or matching text in a string in
a concise, predictable way.

A regular expression engine walks through the pattern and the subject looking
for a match.

```ruby
chars = '407-555-1212'
if (chars.match(/407/)) # returns true
chars = '321-555-1212'
if (chars.match(/407/)) # returns false
```

In this last case, the engine would have parsed through the entire string hoping
to find a series of characters with '407'. We can introduce an 'OR' operator
into the regular expression.

```ruby
if (chars.match(/407|321/)) # returns true
```

Regular expressions start after a forward slash, and ends before the ending
forward slash. What is in-between is known as a regular expression pattern.
Writing regex is really like asking the question, "Does a group of characters
match a specific pattern?".

### Not Just Numbers

Regular expressions are not just meant to match numbers.

```ruby
    chars = ‘boat’
    if (chars.match(/boat|ship/)) # returns true
    chars = ‘ship’
    if (chars.match(/boat|ship/)) # returns true
```

### **What are Regular Expressions Used For?**

Validations on:

- Phone Numbers
- Email
- Password
- Domain Names

Searching:

- Words in a sentence
- Unwanted characters
- Extracting sections
- Replacing, cleaning, or formatting text

### **Our First Problem: Collecting Names**

We are assembling a crew.

```ruby
/smitty|james/ # gets a match on both names we’ve been given
```

Some shipmates are reporting their name as 'ar', 'arr', 'arrr', etc. We could
use the OR operator to add every variant of this as we can. Instead we can use
the + character to match the previous character any number of times.

```ruby
/smitty|james|ar+/ # will match 'arrrrrr'
```

Let’s say we have someone who will give the name 'james' or 'jameson'. Our name
matcher is getting out of hand, requiring a very long regex. If we look at
things we see that names use alphabetical characters, a-z. We can specify a
character set by placing it in square brackets. /[a-z]/ will match any
alphabetical character.

```ruby
/\[a-z\][a-z]\[a-z\][a-z]\[a-z\][a-z]/  # will match 'smitty', but not 'james',
                                        # so this won't work.
```

/[a-z]+/ # this will match any number of characters that are alphabetical. So
this is better, and is very short. Now we have a new name, 'Blackbeard'. This
includes a capital letter, which our pattern ignores.

```ruby
/[a-zA-Z]+/
```

There is another way we can do this by adding a modifier after our the ending
slash in our regular expression.

```ruby
/[a-z]+/i
```

The 'i' modifier means 'case insensitive', which will match uppercase and
lowercase characters. Keep in mind that modifiers are language specific, so
check the documentation on your regular expression methods.

```ruby
/[a-z]+/i
```

Now we see that 'Captain hook' would not be matched because it includes a space.
We could use an actual space in our regex string, but it's very literal. Instead
we can use a '\s' to match a "whitespace character" which could be a space, tab,
or newline character.

```ruby
/[a-z\s]+/i
```

As you can see we've added the \s into the character set. It doesn't matter
where we place it.

```ruby
/[\sa-z]+/i # the same thing
```

Our final shipmate signs up, his name is 'Long John the 3rd'. We're not matching
on numbers, so this won't work. Let’s add a number range.

```ruby
/[a-z0-9\s]+/i
```

We can refactor this to use the word meta character - '\w'. This is the same
as [a-zA-Z0-9].

```ruby
/[\w\s]+/
```

Since this word meta character takes care of matching uppercase letters, we can
remove our 'i' modifier. You can use this meta character outside of a character
set even.

```ruby
/\w+/
```

## Section 2 - Crew Emails

### Verifying email addresses

Subject: [sara@example.com](mailto:sara@example.com)
Starts with a word, followed by an @ character, followed by another word

```ruby
/\w@\w/
```

This would match the a@e in the middle of
[sar](mailto:sara@example.com)[**a@e**](mailto:sara@example.com)[xample.com](mailto:sara@example.com)

We need to add the plus operator to make sure that it continues to match characters.

```ruby
/\w+@\w+/
```

This would match sara@example, but not the .com, because of the period / dot/

```ruby
/\w+@\w+.\w+/
```

This will match 1 word character repeated 1 or more times, then the @ symbol,
then 1 word character repeated 1 or more times, the dot literal, and then
another word character 1 or more times.

This happens to match `sara@example!com` though, because the . character in a
regular expression represents any character except for a newline. It’s like a
wildcard.

```ruby
/\w+@\w+\.\w+/
```

### Escaping Special Characters

| .   | Maches any character except newline |
| --- | ----------------------------------- |
| .   | Matches literal ‘.’ character       |
| +   | Matches 1 or more times             |
| +   | Matches literal ‘+’ character       |
| ?   | Make proceeding pattern optional    |
| \?  | Matches literal ‘?’ character       |

**Being More Specific About the Top-Level Domain (TLD)**

We want to support only certain domains, such as COM, NET, ORG, and EDU

```ruby
/\w+@\w+\.com|net|org|edu/i
```

This will not work because it expects ‘sara@example.com’ OR ‘net’ OR ‘org’ or
‘edu’ (by itself). To make this match ‘sara@example.com’ or ‘sara@example.net’
or ‘sara@example.edu’, we need to use parenthesis.

```ruby
/\w+@\w+\.(com|net|org|edu)/i
```

Parenthesis can create groups, to help with evaluation of the pattern.
This is almost complete, however this pattern will still match if other
characters are added to our string, such as ‘sara@example.comlksh’

**Introducing Anchors**

We can use the ‘^’ at the beginning of a regular expression to signal that the
string must begin with whatever comes after the ‘^’.

```ruby
/^learnbydoing$/
```

The ‘\$’ means that the pattern matching must stop at the end of the pattern
being matched.

```ruby
/^\w+@\w+\.(com|net|org|edu)$/i
```

These will ensure that no text comes before or after the email address. This
means that if we use this regular expression again and email address such as
‘sara@example.comdsfjklwe’ it will not pass the check. Neither would
‘@sara@example.com’.

```ruby
/^new[\s]?\w+$/i
```

This can match ‘New Zealand’, ‘New Guinea’, or ‘Newfoundland’. The ‘?’ after
the newline character signals that it can be optional.

```ruby
/^\$\[0-9]+\.[0-9\][0-9]$/
```

Used to pattern match a dollar amount. Begins with literal \$, followed by any
number of digits 0-9, followed by literal period, followed by two digits 0-9,
with nothing after.

## Section 3 - Confirmative

Find each shipmate’s answer as to whether they are willing to voyage.
Subject Strings:

- ok, i will do it
- okie dokie
- Ahooy, Okay!!!
- why sure, i can go
- arrr, yes matey
- My answer me mate, is yes
- y

Accepted Answer Keywords:

- ok
- Okay
- sure
- yes
- y

### Starting the Expression by Matching the First Confirmation

Using ok literal to directly match “ok”

```ruby
/ok/
```

This would match ‘ok, i will do it’ and ‘okie dokie’. We want the answer to be
by itself, and not part of another word.

\b - Boundary metacharacter - “whole words only”

```ruby
/\b\w+\b/g
```

We put the \b before and after the word matcher.

```ruby
/\bok\b/
```

Will match ‘ok’ but not ‘okie’.

### Inefficient Matching with Multiple OR Statements

We can use the ‘|’ (OR operator) to match other words, like ‘okay’.

```ruby
/\bok\b|\bokay\b/
```

This isn’t very efficient. If we can make the ‘ay’ optional after ‘ok’.

```ruby
/\bok(ay)?\b/i
```

When grouping using parentheses, the question mark makes the entire group optional.

```ruby
/pirate\s(ship)?/
```

So far we match ‘ok’ or ‘Okay’, or even ‘okay’ (the i modifier at the end
matches upper or lowercase). Now lets make it so it matches for ‘sure’ as well.

```ruby
/\bok(ay)?|sure\b/i
```

This will match the word ‘ensure’. We can use the parentheses to group the
pattern, ensuring that a boundary is applied to all answers.

```ruby
/\b(ok(ay)?|sure)\b/i
```

We can add ‘yes’ to the expression easily now.

```ruby
/\b(ok(ay)?|sure|yes)\b/i
```

This still doesn’t match for ‘y’.

```ruby
/\b(ok(ay)?|sure|y(es)?)\b/i
```

All sailors have entered their taglines.

- Work like a captain, play like a pirate.
- Keep calm and say Arr.
- Shiver me timbers matey
- Why are pirates pirates? cuz they arr.

Taglines do not contain numbers, 40 characters or less, 20 characters or more

```ruby
/[a-z]+/i
```

This will match a single word, but not white space.

```ruby
/[a-z\s]+/i
```

This will match a word followed by a space several times.
Work like a captain, play like a pirate.
As you can see this matches up until it hits the comma.

```ruby
/[a-z\s,]+/i
```

This will match the comma, but as you can see we have many characters that we
have to include, which is inefficient.

**Writing a Shorter Pattern with the NOT Operator**

The ‘^’ means ‘not’ when placed within a character set.

```ruby
/[^\d]+/i
```

\d - Any number

So the above pattern matches anything that is not a number.

```ruby
/^[^\d]+$/
```

Outside of a character set, the ‘^’ at the beginning of the expression is used
to anchor the beginning of the subject.

### Even Shorter with Negated Shorthand

| Example | Negated Shorthand | Note                                    |
| ------- | ----------------- | --------------------------------------- |
| [^\d]   | \D                | match every character except numbers    |
| [^\s]   | \S                | match every character except whitespace |
| [^\w]   | \W                | match every character except words      |

You do not need a character set when using negated shorthand characters.

### Problem: We Want to Set Min and Max Characters

```ruby
/^\D+$/
```

This will match any string that doesn’t contain numbers, however it still
doesn’t meet our requirement of having 40 characters or less, or 20 characters
or more.

**Matching a Specific Number of Times with Interval Expressions**

```ruby
/[a-z]{2}/
```

This will match exactly 2 characters.

```ruby
/[a-z]{1,3}/
```

This will match 1 to 3 characters. At least 1, at most 3 characters.

```ruby
/^\D{20,40}$/
```

This will match a minimum of 20 characters, with a max of 40.

```ruby
\b[^aeiouy\s]+\b
```

This will match words that do not contain a, e, i, o, u, or y.

```ruby
/\D+[!]{3,10}$/
```

Match a bunch of words followed by 3 to 10 exclamation marks

## Section 4 - Multi-line Strings

Want to look for all the birds in a block of text

- King penguin
- Emperor penguin
- Wandering albatross
- Arctic Tern
- Rockhopper Penguin
- Weddell seal
- Narwhal
  King penguin\nEmperor penguin\nWandering albatross\nArctic Tern\nNarwhal\n
  Rockhopper Penguin\nWeddell seal
- Has multiple lines separated by newline characters
- Each animal name is 1-2 words
- All animal names have mixed casing
- /penguin/i

Matches the first occurrence of “penguin”

```ruby
/penguin/ig
```

The ‘g’ at the end is a global modifier. This will match “penguin” as many
times as possible with global modifier (find all matches rather than stopping
after the first match).

```ruby
/\w+\spenguin/ig
```

This will match words followed by a space and the word ‘penguin’.

```ruby
/^\w+\spenguin$/ig
```

Adding anchors to our regex causes us to no longer match. This is because it
checks from the very beginning to the end of the subject, which is the entire
multi-line string. We need it to check on each individual line. We can add an
‘m’ modifier (multi-line modifier) to match string on individual lines.

```ruby
/^\w+\spenguin$/mig
```

This causes the anchors to apply to the beginning or end of every single line.

```ruby
/^\w+\s(penguin|albatross)$/mig
```

This now causes the pattern to match the entries that end with ‘albatross’.

```ruby
/^\w+\s(penguin|albatross|tern)$/mig
```

Now it matches strings that end with ‘tern’.

```ruby
/^\-?\d{1,3}\.\d+$/mg
```

This matches an optional ‘-’ characters, 1-3 digits, followed by a literal ‘.’
character, followed by one or more digits. The ‘m’ modifier makes it support
multi-line, and the ‘g’ modifier makes it apply globally.

## Section 5 - Capture Groups

Each shipmate has entered their address.

- 1 Reindeer Lane, North Pole, AK 99705
- 120 East 4th Street, Juneau, AK 99705

It would be great to break apart each part of the address to store separately.

### Number and Street Name

```ruby
\d+\s[\w\s]+\w{4,6},\s
```

- For street name we match any amount of numbers
- A space
- Then any word of any length including spaces (e.g. “Cherry Tree”)
- Then a word of 4-6 characters in length (e.g. “Lane, Ave, Road, etc)
- A comma
- And ending with a space

### City

```ruby
[\w\s]+,\s
```

Any number of characters followed by a comma and a space

### State

```ruby
\w{2}\s
```

2-letter state followed by a space

### Zip Code

```ruby
\d{5}
```

5-digit ZIP code

### Full Matching Pattern

```ruby
/^\d+\s[\w\s]+\w{4,6},\s[\w\s]+,\s\w{2}\s\d{5}$/ig
```

Added anchors on each end, case insensitive (i modifier), and matching globally
(more than once using ‘g’ modifier).

### Matching Groups

Groups are used to return values to you.

```ruby
/(learnbydoing)/
```

This matches the literal ‘learnbydoing’. By putting parentheses around literal,
we are matching a group. We can also modify this to match ‘bydoing’ in
addition to ‘learnbydoing’.

```ruby
/(learn(bydoing))/
```

1. learnbydoing
2. bydoing
3. /(learn((by)doing))/
4. learnbydoing
5. bydoing
6. by

We can match the house number and street by surrounding them with parentheses.

```ruby
/^(\d+\s[\w\s]+\w{4,6}),\s[\w\s]+,\s\w{2}\s\d{5}$/ig
```

This will make the first match group provide the house number and street name.

We can then also match the city by adding parentheses around the city name.

```ruby
/^(\d+\s[\w\s]+\w{4,6}),\s([\w\s]+),\s\w{2}\s\d{5}$/ig
```

Then we can also capture the state

```ruby
/^(\d+\s[\w\s]+\w{4,6}),\s([\w\s]+),\s(\w{2})\s\d{5}$/ig
```

And finally the 5 digit ZIP code

```ruby
/^(\d+\s[\w\s]+\w{4,6}),\s([\w\s]+),\s(\w{2})\s(\d{5})$/ig
```

We now have all 4 groups returned to us. Now we need to restrict street names
to ‘Street’ or ‘Lane’.

```ruby
/^(\d+\s[\w\s]+(street|lane)),\s([\w\s]+),\s(\w{2})\s(\d{5})$/ig
```

But now we’re getting ‘street’ or ‘lane’ as a match group? How can we use this
group, but not have it be a capturing group?

```ruby
/^(\d+\s[\w\s]+(?:street|lane)),\s([\w\s]+),\s(\w{2})\s(\d{5})$/ig
```

By placing a ‘?:’ inside of the beginning of the parentheses, it excludes this
from the matching groups.

## Soup to Bits: Breaking the Ice With Regular Expressions

HTML5 Validation of Form Fields

```html
<form>
  <input type="”text”" pattern="’\(\d{3}\)\s\d{3}-\d{4}’" />
  <input type="”submit”" />
</form>
```

- [https://regex101.com/](https://regex101.com/)
- [http://rubular.com/](http://rubular.com/)

```ruby
/\(\d{3}\)\s\d{3}-\d{4}/ - Phone number pattern
```

Regex can be used with gsub method on Strings in Ruby. Can use #match to extract
data from a string.

In Atom you can use COMMAND+F to find, enable the ‘.\*’ mode to use a regex
search in files.
