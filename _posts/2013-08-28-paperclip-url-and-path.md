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
I'm trying to configure my application so that it stores files in S3 by default
when my application is running in the Production Rails environment, with local
file storage and a customized file path for development and test environments.
Here is my configuration.

<!--more-->
## Paperclip Defaults

You can view the default options by opening the Rails console and inspecting
Paperclip::Attachment.default_options. I'm using Paperclip 3.5.1.

``` ruby
pry(main)> Paperclip::Attachment.default_options

=> {:convert_options=>{},
 :default_style=>:original,
 :default_url=>"/:attachment/:style/missing.png",
 :escape_url=>true,
 :restricted_characters=>/[&amp;$+,\/:;=?@<>\[\]\{\}\|\\\^~%# ]/,
 :filename_cleaner=>nil,
 :hash_data=>":class/:attachment/:id/:style/:updated_at",
 :hash_digest=>"SHA1",
 :interpolator=>Paperclip::Interpolations,
 :only_process=>[],
 :path=>":rails_root/public:url",
 :preserve_files=>false,
 :processors=>[:thumbnail],
 :source_file_options=>{},
 :storage=>:filesystem,
 :styles=>{},
 :url=>"/system/:class/:attachment/:id_partition/:style/:filename",
 :url_generator=>Paperclip::UrlGenerator,
 :use_default_time_zone=>true,
 :use_timestamp=>true,
 :whiny=>true,
 :check_validity_before_processing=>true}
```

## Overriding Defaults

You can override the defaults in config/initializers/paperclip.rb using the
following example which [configures Paperclip to use S3] in production, with the
ImageMagick path we use on the production server (running [Ubuntu] instead of
MacOSX):

``` ruby
if Rails.env.production?
  # See http://rubydoc.info/gems/paperclip/Paperclip/Storage/S3
  Paperclip.options[:image_magick_path] = "/usr/bin/"

  Paperclip::Attachment.default_options.merge!({
    :storage => :s3,
    :s3_credentials => "#{Rails.root}/config/s3.yml",
    :bucket => YAML.load_file("#{Rails.root}/config/s3.yml")[Rails.env]['bucket'],
    :url => ":s3_domain_url"
  })
end
```

Here is my configuration for the local dev/test environments:

``` ruby
# Config for Non-production Environments
unless Rails.env.production?
  Paperclip::Attachment.default_options.merge!({
    :url => "/system/:rails_env/:class/:attachment/:id_partition/:style/:filename",
    :path => ":rails_root/public:url"
  })
end
```

*Remember that the :path and :url have to align so that the file path and URL
path match and the file is served by your web server. It's best to modify the
path only if the local filesystem prefix is different than the 'public' folder
in the root of your Rails application directory.*

[configures Paperclip to use S3]: http://rubydoc.info/gems/paperclip/Paperclip/Storage/S3
[Ubuntu]: http://www.ubuntu.com/
