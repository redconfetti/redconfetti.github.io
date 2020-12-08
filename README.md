# RubyColoredGlasses

Source code for [rubycoloredglasses](http://www.rubycoloredglasses.com/)

## Installation

### Install RBenv

Install [RBenv](https://github.com/rbenv/rbenv#installation)

After you've installed RBenv, run the command `rbenv install`.

### Install Gems

This blog requires that you install the Bundler gem, and then run
`bundle install` to install all needed gem dependencies.

```bash
gem install bundler
bundle install
```

## Jekyll

This blog is powered by [Jekyll](https://jekyllrb.com/docs/home/)

* [Documentation](https://jekyllrb.com/docs/home/)
* [Github](https://github.com/jekyll/jekyll)
* [Jekyll-Uno theme](https://github.com/joshgerdes/jekyll-uno)
* [Github Pages](https://help.github.com/categories/github-pages-basics/)
* [About Github Pages and Jekyll](https://help.github.com/articles/about-github-pages-and-jekyll/)
* [Liquid Templates](https://shopify.github.io/liquid/)

The configuration for the site are stored in [config.yml](./_config.yml).
See [Jekyll Docs - Configuration](https://jekyllrb.com/docs/configuration/)

### Server

You can run `jekyll serve` in the terminal, and then visit
[http://127.0.0.1:4000/](http://127.0.0.1:4000/) to preview the site as you
work on it.

### Plugins

#### Simple-Jekyll-Search

Search page is powered by [Simple-Jekyll-Search]

Due to lack of Jekyll plugin support with Github pages,
[search.json](./search.json) had to be configured with supported
[Liquid Template Filters] to ensure that the generated JSON used for the search
is valid.

[Liquid Template Filters]: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#standard-filters
[Simple-Jekyll-Search]: https://github.com/christian-fei/Simple-Jekyll-Search

## Style Guide

### Markdown

* [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [HTML to Markdown Converter](https://domchristie.github.io/to-markdown/)
* [MarkdownLint Rules](https://github.com/markdownlint/markdownlint/blob/1e78c892/docs/RULES.md)
  (see config in .markdownlint.json)

### Code Highlighting

Code highlighting is supported by the [Rouge gem].

It's recommended that you use [Markdown] syntax to wrap code examples.

```ruby
\`\`\`ruby
def some_method
  puts "this is some method"
end
\`\`\`
```

See [Rouge Wiki - List of supported languages and lexers] for reference.

[Rouge gem]: https://github.com/jneen/rouge
[Rouge Wiki - List of supported languages and lexers]: https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers
[Markdown]: https://guides.github.com/features/mastering-markdown/

## Presentations

I intend to contribute presentations powered by [Reveal.js](https://github.com/hakimel/reveal.js)

[Reveal JS Demo](http://lab.hakim.se/reveal-js/)
