# RubyColoredGlasses

Source code for [blog.redconfetti.com](http://blog.redconfetti.com/)

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

This blog is powered by [Jekyll](https://jekyllrb.com/docs/home/){:target="_blank"}

* [Documentation](https://jekyllrb.com/docs/home/){:target="_blank"}
* [Github](https://github.com/jekyll/jekyll){:target="_blank"}
* [Jekyll-Uno theme](https://github.com/joshgerdes/jekyll-uno){:target="_blank"}
* [Github Pages](https://help.github.com/categories/github-pages-basics/){:target="_blank"}
* [About Github Pages and Jekyll](https://help.github.com/articles/about-github-pages-and-jekyll/){:target="_blank"}

The configuration for the site are stored in [config.yml](./_config.yml). See [Jekyll Docs - Configuration](https://jekyllrb.com/docs/configuration/){:target="_blank"}

### Server

You can run `jekyll serve` in the terminal, and then visit [http://127.0.0.1:4000/](http://127.0.0.1:4000/){:target="_blank"} to preview the site as you work on it.

## Style Guide

### Markdown

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

### Code Highlighting

Code highlighting is supported by the [Rouge gem](https://github.com/jneen/rouge){:target="_blank"}.

### Example

```
{% highlight ruby %}
class Kitten
  def meow
    puts "meow!"
  end
end
{% endhighlight %}
```

See [Rouge Wiki - List of supported languages and lexers](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers){:target="_blank"} for reference.

## Presentations

I intend to contribute presentations powered by [Reveal.js](https://github.com/hakimel/reveal.js){:target="_blank"}

[Reveal JS Demo](http://lab.hakim.se/reveal-js/){:target="_blank"}
