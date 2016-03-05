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

This blog is powered by [Jekyll](https://jekyllrb.com/docs/home/) - [Documentation](https://jekyllrb.com/docs/home/) - [Github](https://github.com/jekyll/jekyll) - [Jekyll-Uno theme](https://github.com/joshgerdes/jekyll-uno)

The configuration for the site are stored in [config.yml](./_config.yml). See [Jekyll Docs - Configuration](https://jekyllrb.com/docs/configuration/)

### Server

You can run `jekyll serve` in the terminal, and then visit [http://127.0.0.1:4000/](http://127.0.0.1:4000/) to preview the site as you work on it.
