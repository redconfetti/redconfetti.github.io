---
layout: post
status: publish
published: true
title: Bundler Definitions
author:
  display_name: redconfetti
  login: redconfetti
  email: jason@redconfetti.com
  url: http://www.redconfetti.com/
author_login: redconfetti
author_email: jason@redconfetti.com
author_url: http://www.redconfetti.com/
wordpress_id: 1662
wordpress_url: http://www.rubycoloredglasses.com/?p=1662
date: '2013-09-01 20:16:38 -0700'
date_gmt: '2013-09-01 20:16:38 -0700'
categories:
- Gems
tags:
- bundler
---
<p>I'm currently starting work on a Ruby gem, '<a title="Github profile for Annotate Gemfile" href="https://github.com/redconfetti/annotate_gemfile" target="_blank">annotate_gemfile</a>', that will grab the title, description, homepage URL, or source URL for every defined gem, and then add them as an annotation / commented for each gem definition in the Gemfile.</p>
<p>As part of this exploration, I'm digging into the source for Bundler to try an understand how it imports details on the gems from the Gemfile, how it queries for details on each from <a href="http://rubygems.org/" target="_blank">RubyGems</a>. Here are some discoveries I'm making.</p>
<h1>Bundler.definition</h1></p>
<p>This appears to return an initialized object with the Gemfile definitions loaded. <a href="https://github.com/bundler/bundler/blob/master/lib/bundler.rb#L146" target="_blank">Bundler.definition</a> returns an instance of Bundler::Definition for the <a href="https://github.com/bundler/bundler/blob/master/lib/bundler.rb#L240" target="_blank">default Gemfile</a> and default lock file (Gemfile.lock). <a href="https://github.com/bundler/bundler/blob/master/lib/bundler/definition.rb#L8" target="_blank">Readable attributes</a> of this "definition" are platforms, sources, ruby_version, and dependencies.</p>
<pre class="brush:ruby">
1.9.3p448 :012 > Bundler.definition.platforms<br />
 => ["ruby"]<br />
1.9.3p448 :013 > Bundler.definition.sources<br />
 => [source at ., rubygems repository https://rubygems.org/]<br />
1.9.3p448 :014 > Bundler.definition.ruby_version<br />
 => nil<br />
1.9.3p448 :015 > Bundler.definition.dependencies<br />
 => [<Bundler::Dependency type=:runtime name="annotate_gem" requirements=">= 0">, <Bundler::Dependency type=:development name="bundler" requirements="~> 1.3">, <Bundler::Dependency type=:development name="rake" requirements=">= 0">, <Bundler::Dependency type=:development name="rspec" requirements="~> 2.14.1">]<br />
</pre></p>
<h2>Platforms</h2></p>
<p>Bundler allows the specification for gems to be installed only on certain <a href="http://bundler.io/man/gemfile.5.html#PLATFORMS-platforms-" target="_blank">Ruby platforms</a>. Options include different minor versions of Ruby (1.8, 1.9, 2.0) as well as differences between types of Ruby (MRI Ruby, Rubinius, jRuby, various Windows Ruby versions).</p>
<h2>Sources</h2></p>
<p>There appear to be three source types supported by Bundler.</p>
<h3>RubyGems</h3></p>
<p>A <a style="font-style: normal;" href="https://github.com/bundler/bundler/blob/master/lib/bundler/source/rubygems.rb" target="_blank">Bundler::Source::RubyGems</a> object represents a RubyGems server. The default one is RubyGems.org. If you check your Gemfile, you'll see a 'source' directive that points to the URL for the gem server with <a href="http://www.rubygems.org/" target="_blank">http://rubygems.org/</a> as the URL.</p>
<pre class="brush:ruby">1.9.3p448 :044 > Bundler.definition.sources[1].class<br />
 => Bundler::Source::Rubygems<br />
1.9.3p448 :038 > Bundler.definition.sources[1]<br />
 => rubygems repository https://rubygems.org/<br />
1.9.3p448 :039 > Bundler.definition.sources[1].name<br />
 => "rubygems repository https://rubygems.org/"<br />
1.9.3p448 :040 > Bundler.definition.sources[1].options<br />
 => {"remotes"=>["https://rubygems.org/"]}<br />
1.9.3p448 :041 > Bundler.definition.sources[1].remotes<br />
 => [#<URI::HTTPS:0x007fb59138ea20 URL:https://rubygems.org/>]<br />
1.9.3p448 :042 > Bundler.definition.sources[1].caches<br />
 => [#<Pathname:/Users/redconfetti/Sites/annotate_gemfile/vendor/cache>, "/Users/redconfetti/.rvm/gems/ruby-1.9.3-p448@annotate_gemfile/cache", "/Users/redconfetti/.rvm/gems/ruby-1.9.3-p448@global/cache"]<br />
1.9.3p448 :043 > Bundler.definition.sources[1].dependency_names<br />
 => ["rake", "annotate_gem", "bundler", "diff-lcs", "rspec-core", "rspec-expectations", "rspec-mocks", "rspec"]</pre><br />
The 'dependency_names' are the names of gems which rely on this RubyGem source server.</p>
<h3>Path</h3></p>
<p>A <a style="font-style: normal;" href="https://github.com/bundler/bundler/blob/master/lib/bundler/source/path.rb" target="_blank">Bundler::Source::Path</a> object represents a local gem path. This source is from the local path of the Gemfile I'm developing. The 'name' appears to be the name of the dependency that relies on the "path" source.</p>
<pre class="brush:ruby">1.9.3p448 :045 > Bundler.definition.sources[0].class<br />
 => Bundler::Source::Path<br />
1.9.3p448 :030 > Bundler.definition.sources[0].name<br />
 => "annotate_gemfile"<br />
1.9.3p448 :031 > Bundler.definition.sources[0].path<br />
 => #<Pathname:.><br />
1.9.3p448 :032 > Bundler.definition.sources[0].options<br />
 => {"path"=>"."}<br />
1.9.3p448 :033 > Bundler.definition.sources[0]<br />
 => source at .</pre></p>
<h3>Git</h3></p>
<p>A <a style="font-style: normal;" href="https://github.com/bundler/bundler/blob/master/lib/bundler/source/git.rb" target="_blank">Bundler::Source::Git</a> object represents a Git repository that provides the source code for a defined gem dependency.</p>
<pre class="brush:ruby">2.0.0p247 :018 > Bundler.definition.sources[0].class<br />
 => Bundler::Source::Git<br />
2.0.0p247 :019 > Bundler.definition.sources[0].name<br />
 => "bootstrap-sass"<br />
2.0.0p247 :020 > Bundler.definition.sources[0].uri<br />
 => "https://github.com/thomas-mcdonald/bootstrap-sass.git"<br />
2.0.0p247 :021 > Bundler.definition.sources[0].ref<br />
 => "master"<br />
2.0.0p247 :022 > Bundler.definition.sources[0].branch<br />
 => nil<br />
2.0.0p247 :023 > Bundler.definition.sources[0].options<br />
 => {"revision"=>"16da2ffb1fb98672f498b482c318db0cfb20a054", "uri"=>"https://github.com/thomas-mcdonald/bootstrap-sass.git"}<br />
2.0.0p247 :024 > Bundler.definition.sources[0].submodules<br />
 => nil<br />
2.0.0p247 :025 > Bundler.definition.sources[0].version<br />
 => nil</pre><br />
As you can see, the 'name' attribute defines the gem that is dependent on this specific Git repository.</p>
<h2>Ruby Version</h2></p>
<p>Your Gemfile may include a specific Ruby version, which is expressed as a <a href="https://github.com/bundler/bundler/blob/master/lib/bundler/ruby_version.rb">Bundler::RubyVersion</a> object in the definition. Heroku <a href="https://devcenter.heroku.com/articles/ruby-versions" target="_blank">requires the Ruby version</a> for projects you are deploying to their platform.</p>
<pre class="brush:ruby"># Gemfile<br />
ruby '2.0.0'</p>
<p># Console<br />
2.0.0p247 :001 > Bundler.definition.ruby_version<br />
 => #<Bundler::RubyVersion:0x007fe8c890a908 @version="2.0.0", @engine="ruby", @input_engine=nil, @engine_version="2.0.0"></pre></p>
<h2>Dependencies</h2></p>
<p>This is the meat of the Gemfile, the dependencies which are represented as <a href="https://github.com/bundler/bundler/blob/master/lib/bundler/dependency.rb" target="_blank">Bundler::Dependency</a> objects.</p>
<pre class="brush:ruby">1.9.3p448 :003 > Bundler.definition.dependencies[0]<br />
 => <Bundler::Dependency type=:runtime name="annotate_gem" requirements=">= 0"><br />
1.9.3p448 :004 > Bundler.definition.dependencies[1]<br />
 => <Bundler::Dependency type=:development name="bundler" requirements="~> 1.3"><br />
1.9.3p448 :005 > Bundler.definition.dependencies[2]<br />
 => <Bundler::Dependency type=:development name="rake" requirements=">= 0"><br />
1.9.3p448 :006 > Bundler.definition.dependencies[3]<br />
 => <Bundler::Dependency type=:development name="rspec" requirements="~> 2.14.1"></pre><br />
It appears that dependencies do not specify a source if they are provided from a Ruby Gems server. Only if they are sourced from a Path or Git repository.</p>
<pre class="brush:ruby"># Dependencies from my gem in development<br />
1.9.3p448 :021 > Bundler.definition.dependencies[0]<br />
 => <Bundler::Dependency type=:runtime name="annotate_gem" requirements=">= 0"><br />
1.9.3p448 :022 > Bundler.definition.dependencies[0].source<br />
 => source at .<br />
1.9.3p448 :023 > Bundler.definition.dependencies[1]<br />
 => <Bundler::Dependency type=:development name="bundler" requirements="~> 1.3"><br />
1.9.3p448 :024 > Bundler.definition.dependencies[1].source<br />
 => nil</p>
<p># Bootstrap-Sass dependency from another project<br />
2.0.0p247 :022 > Bundler.definition.dependencies[10].class<br />
 => Bundler::Dependency<br />
2.0.0p247 :023 > Bundler.definition.dependencies[10]<br />
 => <Bundler::Dependency type=:runtime name="bootstrap-sass" requirements=">= 0"><br />
2.0.0p247 :024 > Bundler.definition.dependencies[10].groups<br />
 => [:default]<br />
2.0.0p247 :025 > Bundler.definition.dependencies[10].source.class<br />
 => Bundler::Source::Git</pre><br />
 </p>
