---
layout: post
title: Bundler Definitions
date: '2013-09-01 20:16:38 -0700'
categories:
- Gems
tags:
- bundler
---

I'm currently starting work on a Ruby gem,
[Github profile for Annotate Gemfile], that will grab the title, description,
homepage URL, or source URL for every defined gem, and then add them as an
annotation / commented for each gem definition in the Gemfile.

As part of this exploration, I'm digging into the source for Bundler to try an
understand how it imports details on the gems from the Gemfile, how it queries
for details on each from [RubyGems]. Here are some discoveries I'm making.
<!--more-->

[Github profile for Annotate Gemfile]: https://github.com/redconfetti/annotate_gemfile
[RubyGems]: http://rubygems.org/
# Bundler.definition


This appears to return an initialized object with the Gemfile definitions
loaded. [Bundler.definition] returns an instance of Bundler::Definition for the
[default Gemfile] and default lock file (Gemfile.lock). [Readable attributes] of
this "definition" are platforms, sources, ruby_version, and dependencies.

[Bundler.definition]: https://github.com/bundler/bundler/blob/master/lib/bundler.rb#L146
[default Gemfile]: https://github.com/bundler/bundler/blob/master/lib/bundler.rb#L240
[Readable attributes]: https://github.com/bundler/bundler/blob/master/lib/bundler/definition.rb#L8

```ruby
>> Bundler.definition.platforms
=> ["ruby"]

>> Bundler.definition.sources
=> [source at ., rubygems repository https://rubygems.org/]

>> Bundler.definition.ruby_version
=> nil

>> Bundler.definition.dependencies
=> [<Bundler::Dependency type=:runtime name="annotate_gem" requirements=">= 0">, <Bundler::Dependency type=:development name="bundler" requirements="~> 1.3">, <Bundler::Dependency type=:development name="rake" requirements=">= 0">, <Bundler::Dependency type=:development name="rspec" requirements="~> 2.14.1">]
```

## Platforms

Bundler allows the specification for gems to be installed only on certain
[Ruby platforms]. Options include different minor versions of Ruby (1.8, 1.9,
2.0) as well as differences between types of Ruby (MRI Ruby, Rubinius, jRuby,
various Windows Ruby versions).

[Ruby platforms]: http://bundler.io/man/gemfile.5.html#PLATFORMS-platforms-

## Sources

There appear to be three source types supported by Bundler.

### RubyGems

A [Bundler::Source::RubyGems] object represents a RubyGems server. The default
one is RubyGems.org. If you check your Gemfile, you'll see a 'source' directive
that points to the URL for the gem server with [http://www.rubygems.org/] as the
URL.

[http://www.rubygems.org/]: http://www.rubygems.org/
[Bundler::Source::RubyGems]: https://github.com/bundler/bundler/blob/master/lib/bundler/source/rubygems.rb

``` ruby
1.9.3p448 :044 > Bundler.definition.sources[1].class
 => Bundler::Source::Rubygems

1.9.3p448 :038 > Bundler.definition.sources[1]
 => rubygems repository https://rubygems.org/

1.9.3p448 :039 > Bundler.definition.sources[1].name
 => "rubygems repository https://rubygems.org/"

1.9.3p448 :040 > Bundler.definition.sources[1].options
 => {"remotes"=>["https://rubygems.org/"]}

1.9.3p448 :041 > Bundler.definition.sources[1].remotes
 => [#<URI::HTTPS:0x007fb59138ea20 URL:https://rubygems.org/>]

1.9.3p448 :042 > Bundler.definition.sources[1].caches
 => [#<Pathname:/Users/redconfetti/Sites/annotate_gemfile/vendor/cache>, "/Users/redconfetti/.rvm/gems/ruby-1.9.3-p448@annotate_gemfile/cache", "/Users/redconfetti/.rvm/gems/ruby-1.9.3-p448@global/cache"]

1.9.3p448 :043 > Bundler.definition.sources[1].dependency_names
 => ["rake", "annotate_gem", "bundler", "diff-lcs", "rspec-core", "rspec-expectations", "rspec-mocks", "rspec"]
```

The 'dependency_names' are the names of gems which rely on this RubyGem source server.

### Path

A [Bundler::Source::Path]object represents a local gem path. This source is from
the local path of the Gemfile I'm developing. The 'name' appears to be the name
of the dependency that relies on the "path" source.

[Bundler::Source::Path]: https://github.com/bundler/bundler/blob/master/lib/bundler/source/path.rb

``` ruby
1.9.3p448 :045 > Bundler.definition.sources[0].class
 => Bundler::Source::Path

1.9.3p448 :030 > Bundler.definition.sources[0].name
 => "annotate_gemfile"

1.9.3p448 :031 > Bundler.definition.sources[0].path
 => #<Pathname:.>

1.9.3p448 :032 > Bundler.definition.sources[0].options
 => {"path"=>"."}

1.9.3p448 :033 > Bundler.definition.sources[0]
 => source at .
```

### Git

A [Bundler::Source::Git] object represents a Git repository that provides the
source code for a defined gem dependency.

[Bundler::Source::Git]: https://github.com/bundler/bundler/blob/master/lib/bundler/source/git.rb

``` ruby
2.0.0p247 :018 > Bundler.definition.sources[0].class
 => Bundler::Source::Git

2.0.0p247 :019 > Bundler.definition.sources[0].name
 => "bootstrap-sass"

2.0.0p247 :020 > Bundler.definition.sources[0].uri
 => "https://github.com/thomas-mcdonald/bootstrap-sass.git"

2.0.0p247 :021 > Bundler.definition.sources[0].ref
 => "master"

2.0.0p247 :022 > Bundler.definition.sources[0].branch
 => nil

2.0.0p247 :023 > Bundler.definition.sources[0].options
 => {"revision"=>"16da2ffb1fb98672f498b482c318db0cfb20a054", "uri"=>"https://github.com/thomas-mcdonald/bootstrap-sass.git"}

2.0.0p247 :024 > Bundler.definition.sources[0].submodules
 => nil

2.0.0p247 :025 > Bundler.definition.sources[0].version
 => nil
```

As you can see, the 'name' attribute defines the gem that is dependent on this
specific Git repository.

## Ruby Version

Your Gemfile may include a specific Ruby version, which is expressed as a
[Bundler::RubyVersion] object in the definition. Heroku
[requires the Ruby version] for projects you are deploying to their platform.

[Bundler::RubyVersion]: https://github.com/bundler/bundler/blob/master/lib/bundler/ruby_version.rb
[requires the Ruby version]: https://devcenter.heroku.com/articles/ruby-versions

``` ruby
# Gemfile
ruby '2.0.0'

# Console
2.0.0p247 :001 > Bundler.definition.ruby_version
 => #<Bundler::RubyVersion:0x007fe8c890a908 @version="2.0.0", @engine="ruby", @input_engine=nil, @engine_version="2.0.0">
```

## Dependencies

This is the meat of the Gemfile, the dependencies which are represented as
[Bundler::Dependency] objects.

[Bundler::Dependency]: https://github.com/bundler/bundler/blob/master/lib/bundler/dependency.rb

``` ruby
1.9.3p448 :003 > Bundler.definition.dependencies[0]
 => <Bundler::Dependency type=:runtime name="annotate_gem" requirements=">= 0">

1.9.3p448 :004 > Bundler.definition.dependencies[1]
 => <Bundler::Dependency type=:development name="bundler" requirements="~> 1.3">

1.9.3p448 :005 > Bundler.definition.dependencies[2]
 => <Bundler::Dependency type=:development name="rake" requirements=">= 0">

1.9.3p448 :006 > Bundler.definition.dependencies[3]
 => <Bundler::Dependency type=:development name="rspec" requirements="~> 2.14.1">
```

It appears that dependencies do not specify a source if they are provided from a
Ruby Gems server. Only if they are sourced from a Path or Git repository.


``` ruby
# Dependencies from my gem in development
1.9.3p448 :021 > Bundler.definition.dependencies[0]
 => <Bundler::Dependency type=:runtime name="annotate_gem" requirements=">= 0">

1.9.3p448 :022 > Bundler.definition.dependencies[0].source
 => source at .

1.9.3p448 :023 > Bundler.definition.dependencies[1]
 => <Bundler::Dependency type=:development name="bundler" requirements="~> 1.3">

1.9.3p448 :024 > Bundler.definition.dependencies[1].source
 => nil

# Bootstrap-Sass dependency from another project
2.0.0p247 :022 > Bundler.definition.dependencies[10].class
 => Bundler::Dependency

2.0.0p247 :023 > Bundler.definition.dependencies[10]
 => <Bundler::Dependency type=:runtime name="bootstrap-sass" requirements=">= 0">

2.0.0p247 :024 > Bundler.definition.dependencies[10].groups
 => [:default]

2.0.0p247 :025 > Bundler.definition.dependencies[10].source.class
 => Bundler::Source::Git
```
