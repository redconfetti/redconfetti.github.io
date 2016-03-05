---
layout: post
title: Updating a Serialized Object from a Web form
date: '2012-06-06 18:19:01 -0700'
categories:
- Ruby on Rails
tags:
- serialize
comments: []
---
<p>You may run into a situation where you create some sort of standard Ruby class that you want to associate with an ActiveRecord model. The <a href="http://apidock.com/rails/ActiveRecord/AttributeMethods/Serialization/ClassMethods/serialize" target="_blank">serialize method</a> allows you to store an object inside of a text field for an ActiveRecord object.</p>
<p>With Rails 2.3 support for models nested within forms was added, but it's clear that this support isn't compatible with serialized objects. In my example below I have a Post model, which represents a simple blog post. The serialized object is instantiated from a custom class called 'Metadata', which stores metadata for the post like it's type and version.</p>
<pre class="brush:ruby">class Post < ActiveRecord::Base<br />
  serialize(:meta, Metadata)</p>
<p>  accepts_nested_attributes_for :meta<br />
end</pre></p>
<pre class="brush:ruby">class Metadata</p>
<p>  # Type of Post<br />
  attr_accessor :type</p>
<p>  # Version of Post<br />
  attr_accessor :version</p>
<p>  # builds new instance using hash<br />
  def initialize(params = {})<br />
    self.errors     = Array.new<br />
    self.type       = params[:type] unless params[:type] == nil<br />
    self.version    = params[:version] unless params[:version] == nil<br />
  end</p>
<p>end</pre><br />
I tried to setup the Post to accept nested attributes for my serialized field, which resulted in the following error:</p>
<pre class="brush:ruby">> post = Post.new<br />
ArgumentError: No association found for name 'serialized_field'. Has it been defined yet?</pre><br />
I tried to setup a form using the 'simple_fields_for' method used by <a href="https://github.com/plataformatec/simple_form" target="_blank">Simpleform</a> (the equivalent of <a href="http://apidock.com/rails/v3.2.1/ActionView/Helpers/FormBuilder/fields_for" target="_blank">fields_for</a>).</p>
<pre class="brush:ruby"><%= simple_form_for @post do |f| %><br />
  <%= f.input :title %><br />
  <%= f.input :body, :as => 'text' %><br />
  <%= f.simple_fields_for :meta do |m| %><br />
    <%= m.select :type, options_for_select(@post_types, @post.meta.type) %><br />
    <%= m.input :version %><br />
  <% end %><br />
<% end %></pre><br />
This only resulted in an error with the 'update_attributes' method provided by ActiveRecord. A more detailed Gist is available <a href="https://gist.github.com/2871786" target="_blank">here</a>.</p>
<pre class="brush:ruby">> post.update_attributes(:meta => { :type => 'awesome' })<br />
ActiveRecord::SerializationTypeMismatch: meta was supposed to be a Metadata, but was a Hash</pre><br />
This causes me to suspect that Rails is not yet designed to handle the update of serialized objects via a nested hash included with the parameters submitted by the form. I can see that what happens is that the nested parameter hash itself is assigned to the 'meta' field for the object, instead of the values for each hash key being applied to the custom object.</p>
<p>The solution that works is instead of defining the 'simple_fields_for' block as part of the parent object which the form is being built for, simply insert it as its own floating object inside your form. In the above example the 'simple_fields_for' method was called and passed a block within the Post form. This results in the fields being defined as part of the Post in the HTML form.</p>
<pre class="brush:xml"><input id="post_meta_version" name="post[meta][player]" size="50" type="text"></pre><br />
Next in your controller, assign/update the parameters for the custom object separately.</p>
<pre class="brush:ruby"># PUT /posts/1<br />
def update<br />
  @post = Post.find(params[:id])</p>
<p>  respond_to do |format|<br />
    if @post.update_attributes(params[:post])</p>
<p>      # Update Metadata and save again<br />
      @post.meta.update_attributes(params[:metadata])<br />
      @post.save</p>
<p>      format.html { redirect_to @post, notice: 'Post was successfully updated.' }<br />
      format.json { head :no_content }<br />
    else<br />
      format.html { render action: "edit" }<br />
      format.json { render json: @post.errors, status: :unprocessable_entity }<br />
    end<br />
  end<br />
end</pre></p>
