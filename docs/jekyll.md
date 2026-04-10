# Jekyll

This blog is powered by [Jekyll](https://jekyllrb.com/docs/home/)

* [Documentation](https://jekyllrb.com/docs/home/)
* [Github](https://github.com/jekyll/jekyll)
* [Jekyll-Uno theme](https://github.com/joshgerdes/jekyll-uno)
* [Github Pages](https://help.github.com/categories/github-pages-basics/)
* [About Github Pages and Jekyll](https://help.github.com/articles/about-github-pages-and-jekyll/)
* [Liquid Templates](https://shopify.github.io/liquid/)

The configuration for the site are stored in [config.yml](../_config.yml).
See [Jekyll Docs - Configuration](https://jekyllrb.com/docs/configuration/)

## Server

You can run `jekyll serve` in the terminal, and then visit
[http://127.0.0.1:4000/](http://127.0.0.1:4000/) to preview the site as you
work on it.

## Plugins

### Simple-Jekyll-Search

Search page is powered by [Simple-Jekyll-Search]

Due to lack of Jekyll plugin support with Github pages,
[search.json](../search.json) had to be configured with supported
[Liquid Template Filters] to ensure that the generated JSON used for the search
is valid.

[Liquid Template Filters]: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#standard-filters
[Simple-Jekyll-Search]: https://github.com/christian-fei/Simple-Jekyll-Search
