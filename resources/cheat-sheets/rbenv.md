---
layout: page
title: RBenv
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

``` shell
# list all available versions
rbenv install -l
rbenv install --list-all

# display ruby versions installed (* indicating in use)
rbenv versions

# display current ruby version in use
rbenv version

# display version of rbenv itself
rbenv --version

# install a ruby version
rbenv install 2.7.2

# set the global ruby version
rbenv global 2.7.2

# set a shell specific version of ruby (as opposed to application / global version)
rbenv shell jruby-1.7.1

# set the application specific ruby version (written to `.ruby-version`)
rbenv local 2.7.2

# install shims for all Ruby executables known to rbenv
rbenv rehash

# display full path to executable that rbenv will invoke for a command
rbenv which irb

# list all Ruby versions with the given command installed
rbenv whence rackup
```

## Installation / Update with Homebrew

```bash
# install with homebrew
brew install rbenv ruby-build

# initialize / setup to run in your shell 
rbenv init

# update with homebrew
brew upgrade rbenv ruby-build
```
