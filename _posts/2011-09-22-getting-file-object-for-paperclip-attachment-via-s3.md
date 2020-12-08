---
layout: post
title: Getting File object for Paperclip Attachment via S3
date: '2011-09-22 15:34:10 -0700'
categories:
- Ruby on Rails
tags:
- rails
- csv
- paperclip
- s3
---

I'm working on a project where we are using the Paperclip plugin for Ruby on
Rails for file handling and associations with other models.

I'm working on a CSV import option right now, using [this tutorial][1] to help
me get a head start on how to break the contents of the file up into rows and
columns. I'm not passing the file directly from a form to the controller
method, but I'm opening the file that has already been saved after being
uploaded via AJAX.
<!--more-->

I couldn't find out how I would get the file itself into an object that I can
pass to the parser like so:

``` ruby
@parsed_file=CSV::Reader.parse(params[:dump][:file])
```

It turns out that you simply need to refer to the Paperclip attachment and
then use 'to_file' like so:

``` ruby
require 'csv'
@upload = Upload.find(params[:upload_id])
@parsed_file = CSV::Reader.parse(@upload.attachment.to_file)
```

[1]: http://satishonrails.wordpress.com/2007/07/18/how-to-import-csv-file-in-rails/
