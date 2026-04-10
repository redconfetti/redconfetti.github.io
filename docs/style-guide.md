# Style Guide

## Markdown

* [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [HTML to Markdown Converter](https://domchristie.github.io/to-markdown/)
* [MarkdownLint Rules](https://github.com/markdownlint/markdownlint/blob/1e78c892/docs/RULES.md)
  (see config in .markdownlint.json)

## Code Highlighting

Code highlighting is supported by the [Rouge gem].

It's recommended that you use [Markdown] syntax to wrap code examples.

````ruby
```ruby
def some_method
  puts "this is some method"
end
```
````

See [Rouge Wiki - List of supported languages and lexers] for reference.

[Rouge gem]: https://github.com/jneen/rouge
[Rouge Wiki - List of supported languages and lexers]: https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers
[Markdown]: https://guides.github.com/features/mastering-markdown/
