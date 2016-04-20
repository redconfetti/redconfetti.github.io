---
layout: page
title: RVM
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

``` shell
# list all available Ruby versions
rvm list known

# list installed ruby versions
rvm list

# view requirements for installing certain ruby versions
rvm requirements

# install ruby 1.9.2
rvm install 1.9.2

# use version of ruby
rvm use 1.9.2

# List Gem Sets
rvm gemset list

# Create New Gem Set
rvm gemset create gemset_name

# Use gemset
rvm gemset use gemset_name

# Delete gemset
rvm gemset delete gemset_name

# Create .rvmrc file for project
cd ~/projects/my_project/
rvm --rvmrc --create 1.9.2@my_project
```
