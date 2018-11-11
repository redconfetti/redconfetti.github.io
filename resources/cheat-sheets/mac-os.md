---
layout: page
title: Mac OS
---

[Back to Cheat Sheets](/resources/cheat-sheets/)

Many other modifications can be found in [Mathias Bynens Dotfiles](https://github.com/mathiasbynens/dotfiles/blob/master/.macos)

## Open Current or Other Folder in Finder

``` shell
open .
open ~/Projects
```

## Disable the Dock's Bounce to Alert behavior

If you prefer to not have application icons in your dock bounce to alert you, you can run the following:

``` shell
defaults write com.apple.dock no-bouncing -bool TRUE
killall Dock
```

## Changing the location of screenshots

By defaults the screenshots you take using __&#8984; + 3__, or __&#8984; + 4__ show up on your desktop. You can reconfigure
these to show up in a different folder you specify.

``` shell
# See the location where screenshots are placed
defaults read com.apple.screencapture location

# Create a screenshots folder and set it as the new location
mkdir -p ~/Screenshots
defaults write com.apple.screencapture location ~/Screenshots
killall Dock
```

## Automatically quit printer app once the print jobs complete

``` shell
defaults write com.apple.print.PrintingPrefs "Quit When Finished" -bool true
```

## Disable Writing Behavior in Applications

``` shell
# Disable automatic capitalization as it’s annoying when typing code
defaults write NSGlobalDomain NSAutomaticCapitalizationEnabled -bool false

## Disable smart dashes as they’re annoying when typing code
defaults write NSGlobalDomain NSAutomaticDashSubstitutionEnabled -bool false

## Disable automatic period substitution as it’s annoying when typing code
defaults write NSGlobalDomain NSAutomaticPeriodSubstitutionEnabled -bool false

# Disable smart quotes as they’re annoying when typing code
defaults write NSGlobalDomain NSAutomaticQuoteSubstitutionEnabled -bool false

# Disable auto-correct
defaults write NSGlobalDomain NSAutomaticSpellingCorrectionEnabled -bool false
```
