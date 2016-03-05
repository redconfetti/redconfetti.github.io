---
layout: post
title: Paperclip error with non-image file
date: '2011-12-05 16:43:25 -0800'
categories:
- Ruby on Rails
tags:
- paperclip
- identify
- imagemagick
- non-image
comments: []
---
<p>Recently I updated to Rails 3.1 from 2.3.8 for a project I'm working on. Paperclip version 2.4.5 was working perfectly well for me locally on my Mac OS X 10.7.2 laptop, with ImageMagick version 6.7.3-1.</p>
<p>We had just launched the upgraded Rails 3.1 application to our production server, which went smoothly, but upon my checklist of pages to test (we don't have test suite in use yet) I noticed that the file upload was failing. Further investigation showed that the object using paperclip was failing to save:</p>
<pre class="brush:shell">upload.errors.inspect: </p>
<p>#["/tmp/stream20111205-28441-10oj2ck-0.csv is not recognized by the 'identify' command.", "/tmp/stream20111205-28441-10oj2ck-0.csv is not recognized by the 'identify' command."]}>, @base=#></pre><br />
Further investigation helped me identify this 'identify' command as an executable provided by ImageMagick. I ran this command on the temporary file which was located in /tmp, and I got an error such as the one that follows on both my local machine and in production. The odd thing was that the file upload worked locally, but was failing remotely.</p>
<pre class="brush:shell">$ identify test-import20111205-1909-1xsbr19-0.csv </p>
<p>identify: no decode delegate for this image format `test-import20111205-1909-1xsbr19-0.csv' @ error/constitute.c/ReadImage/532.</pre><br />
This didn't make sense. I was uploading a CSV file, not an image. I wasn't even using the ':styles' hash value in my model with the 'has_attached_file' expression. Why was it checking to see what type of image the file is?!?!</p>
<p>I tried to upgrade and reinstall ImageMagick on the production server, but this had no effect. This made me suspect that paperclip was the cause of the issue. I also created an initalizer for paperclip under /config/initalizers/paperclip.rb pointing out the image_magick_path, which I had seen resolve other issues with 'identify' errors.</p>
<pre class="brush:rails">if Rails.env == 'production'<br />
  Paperclip.options[:image_magick_path] = "/usr/bin/"<br />
end</pre><br />
This didn't help at all.</p>
<p>I tried to reconfigure the application to use Paperclip version 2.4.4 and 2.4.3, but that didn't resolve the issue.</p>
<p>Finally I updated my Gemfile to use the latest Github version of Paperclip. I deployed this update, tested, and finally the issue was resolved.</p>
<pre class="brush:rails">gem 'paperclip', "~> 2.4.5", :git => 'git://github.com/thoughtbot/paperclip.git'</pre></p>
