---
layout: post
title: Paperclip URL and Path
date: '2013-08-28 20:04:13 -0700'
categories:
- Ruby on Rails
tags:
- paperclip
- s3
---
<p>I'm trying to configure my application so that it stores files in S3 by default when my application is running in the Production Rails environment, with local file storage and a customized file path for development and test environments. Here is my configuration.</p>
<h2>Paperclip Defaults</h2><br />
You can view the default options by opening the Rails console and inspecting Paperclip::Attachment.default_options. I'm using Paperclip 3.5.1.</p>
<pre class="brush:ruby">pry(main)> Paperclip::Attachment.default_options<br />
=> {:convert_options=>{},<br />
 :default_style=>:original,<br />
 :default_url=>"/:attachment/:style/missing.png",<br />
 :escape_url=>true,<br />
 :restricted_characters=>/[&amp;$+,\/:;=?@<>\[\]\{\}\|\\\^~%# ]/,<br />
 :filename_cleaner=>nil,<br />
 :hash_data=>":class/:attachment/:id/:style/:updated_at",<br />
 :hash_digest=>"SHA1",<br />
 :interpolator=>Paperclip::Interpolations,<br />
 :only_process=>[],<br />
 :path=>":rails_root/public:url",<br />
 :preserve_files=>false,<br />
 :processors=>[:thumbnail],<br />
 :source_file_options=>{},<br />
 :storage=>:filesystem,<br />
 :styles=>{},<br />
 :url=>"/system/:class/:attachment/:id_partition/:style/:filename",<br />
 :url_generator=>Paperclip::UrlGenerator,<br />
 :use_default_time_zone=>true,<br />
 :use_timestamp=>true,<br />
 :whiny=>true,<br />
 :check_validity_before_processing=>true}</pre></p>
<h2>Overriding Defaults</h2><br />
You can override the defaults in config/initializers/paperclip.rb using the following example which <a href="http://rubydoc.info/gems/paperclip/Paperclip/Storage/S3" target="_blank">configures Paperclip to use S3</a> in production, with the ImageMagick path we use on the production server (running <a href="http://www.ubuntu.com/" target="_blank">Ubuntu</a> instead of MacOSX):</p>
<pre class="brush:ruby">if Rails.env.production?<br />
  # See http://rubydoc.info/gems/paperclip/Paperclip/Storage/S3<br />
  Paperclip.options[:image_magick_path] = "/usr/bin/"<br />
  Paperclip::Attachment.default_options.merge!({<br />
    :storage => :s3,<br />
    :s3_credentials => "#{Rails.root}/config/s3.yml",<br />
    :bucket => YAML.load_file("#{Rails.root}/config/s3.yml")[Rails.env]['bucket'],<br />
    :url => ":s3_domain_url"<br />
  })<br />
end</pre><br />
Here is my configuration for the local dev/test environments:</p>
<pre class="brush:ruby"># Config for Non-production Environments<br />
unless Rails.env.production?<br />
  Paperclip::Attachment.default_options.merge!({<br />
    :url => "/system/:rails_env/:class/:attachment/:id_partition/:style/:filename",<br />
    :path => ":rails_root/public:url"<br />
  })<br />
end</pre><br />
<span style="line-height: 21px;">Remember that the :path and :url have to align so that the file path and URL path match and the file is served by your web server. It's best to modify the path only if the local filesystem prefix is different than the 'public' folder in the root of your Rails application directory.</span></p>
<p> </p>
