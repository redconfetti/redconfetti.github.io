---
layout: page
title: RubyGems
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

``` shell
# List installed Gems
gem list

# Display Gem Filesystem Path
gem which rake

# List all Gem Commands
gem help commands

# Display all gems that need updates
gem outdated

# Install a gem
gem install rake

# Uninstall a gem
gem uninstall rake

# Uninstall all gems (does not remove gems in 'global' RVM gemset)
gem uninstall -a -d -x -I
```
