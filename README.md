# RubyColoredGlasses

Source code for [rubycoloredglasses](http://www.rubycoloredglasses.com/)

## Installation

### Install RVM

```
\curl -sSL https://get.rvm.io | bash -s stable --ruby
```

You will have to change into the directory to initialize the gemset. If the version of Ruby required is not installed, you'll be prompted to install it (e.g. `rvm install ruby-2.2.1`)

### Install Gems

This blog requires that you install the Bundler gem, and then run `bundle install` to install all needed gem dependencies.

```
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

The configuration for the site are stored in [config.yml](./_config.yml). See [Jekyll Docs - Configuration](https://jekyllrb.com/docs/configuration/)

### Server

You can run `jekyll serve` in the terminal, and then visit [http://127.0.0.1:4000/](http://127.0.0.1:4000/) to preview the site as you work on it.

### Plugins

#### Simple-Jekyll-Search

Search page is powered by [Simple-Jekyll-Search](https://github.com/christian-fei/Simple-Jekyll-Search)

Due to lack of Jekyll plugin support with Github pages, [search.json](./search.json) had to be configured with supported [Liquid Template Filters](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#standard-filters) to ensure that the generated JSON used for the search is valid.

## Style Guide

### Markdown

* [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [HTML to Markdown Converter](https://domchristie.github.io/to-markdown/)

### Code Highlighting

Code highlighting is supported by the [Rouge gem](https://github.com/jneen/rouge).

It's recommended that you use [Markdown](https://guides.github.com/features/mastering-markdown/) syntax to wrap code examples.

```
\`\`\`ruby
def some_method
  puts "this is some method"
end
\`\`\`
```

See [Rouge Wiki - List of supported languages and lexers](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers) for reference.

## Presentations

I intend to contribute presentations powered by [Reveal.js](https://github.com/hakimel/reveal.js)

[Reveal JS Demo](http://lab.hakim.se/reveal-js/)
