---
layout: post
title: Strong Parameters with Spree Extensions
date: '2014-04-20 01:05:29 -0700'
categories:
- Ruby on Rails
- Gems
tags:
- Spree
---
<p>I'm currently working on an extension for <a href="https://github.com/spree/spree" target="_blank">Spree</a>, an e-commerce solution for Ruby on Rails applications. The <a href="http://guides.spreecommerce.com/developer/" target="_blank">developer documentation</a> for Spree is very helpful, letting developers know that they should use certain Ruby meta-programming methods to extend the functionality of the Spree system. The extension I'm working on was setup under a version of Spree that used Rails 3. Now that Spree v2.2.1 uses Rails 4.0.4, I'm having to refactor some parts of this extension to adapt to new practices.</p>
<h3>From Accessible Attributes to Strong Parameters</h3><br />
Under Rails 3, you have to use <a href="http://apidock.com/rails/ActiveModel/MassAssignmentSecurity/ClassMethods/attr_accessible" target="_blank">#attr_accessible</a> to ensure that attributes of a model can be updated via methods such as <a href="http://apidock.com/rails/ActiveResource/Base/update_attributes" target="_blank">#update_attributes</a>. This was implemented to protect models from mass assignment vulnerability. In Rails 4, this functionality re-implemented as a convention at the controller level, in a feature known as <a href="http://apidock.com/rails/ActionController/StrongParameters" target="_blank">Strong Parameters</a>. This <a href="http://www.sitepoint.com/rails-4-quick-look-strong-parameters/" target="_blank">Rails 4 Quick Look: Strong Parameters</a> article explains this clearly.</p>
<h3>Rails 3 Decorators</h3><br />
Under Rails 3, a Spree extension could introduce new columns to Spree models via a migration, and then simply introduce a decorator like this one to make the new attributes available for mass assignment updates.</p>
<pre class="brush:rails">Spree::Order.class_eval do<br />
  attr_accessible :my_extensions_attribute, :my_extensions_attribute2<br />
end<br />
</pre><br />
But now with Rails 4, I have to create a decorator for Spree::Api::OrdersController instead. I imagined that this decorator will have to somehow apply a call to the <a href="http://apidock.com/rails/ActionController/Parameters/permit" target="_blank">#permit</a> method on the 'params', allowing my extension attributes to be updated as well.</p>
<p>After some searching online I realized that the best solution to this problem is to specify an <a href="http://apidock.com/rails/Module/alias_method_chain" target="_blank">alias_method_chain</a> inside my decorator. We don't have control over the Spree code, and there is no option for using 'super' because inheritance isn't involved here. So this is definitely a situation where we <a href="http://erniemiller.org/2011/02/03/when-to-use-alias_method_chain/" target="_blank">should use an alias method chain</a>.</p>
<h3>Spree Controller Helpers for Strong Parameters</h3><br />
I just noticed however that the <a href="https://github.com/spree/spree/blob/e2bd38d4/api/app/controllers/spree/api/orders_controller.rb#L93" target="_blank">Spree::Api::OrdersController#order_params</a> method has a more complex method for permitting the attributes than I expected. In this case the order attributes are provided by <a href="https://github.com/spree/spree/blob/e2bd38d4/api/app/controllers/spree/api/orders_controller.rb#L107" target="_blank">#permitted_order_attributes</a>, which makes a 'super' call that refers to the parent controller <a href="https://github.com/spree/spree/blob/e2bd38d4/api/app/controllers/spree/api/base_controller.rb" target="_blank">Spree::Api::BaseController</a>. The BaseController doesn't have a #permitted_order_attributes method defined, however it does include <a href="https://github.com/spree/spree/blob/e2bd38d4/core/lib/spree/core/controller_helpers/strong_parameters.rb" target="_blank">Spree::Core::ControllerHelpers::StrongParameters</a>. which defines <a href="https://github.com/spree/spree/blob/e2bd38d4/core/lib/spree/core/controller_helpers/strong_parameters.rb#L28" target="_blank">#permitted_order_attributes</a>. If you follow the dependencies further, you'll see that all these methods in Spree::Core::ControllerHelpers::StrongParameters rely on <a href="https://github.com/spree/spree/blob/e2bd38d4/core/lib/spree/permitted_attributes.rb" target="_blank">Spree:PermittedAttributes</a>.</p>
<p>So all that is necessary to define a new Spree::Order attribute is to define a Spree::PermittedAttributes decorator like so:</p>
<pre class="brush:rails"># lib/spree/permitted_attributes_decorator.rb<br />
Spree::PermittedAttributes.class_eval do<br />
  @@checkout_attributes.push(:my_extensions_attribute, :my_extensions_attribute2)<br />
end<br />
</pre><br />
I'll have to test this out, but it seems like the plausible approach. I hope this helps any other developers.</p>
<h3>Update</h3><br />
I just went back to a StackOverflow article on this subject that I had seen before - <a href="http://stackoverflow.com/questions/19924702/rails-4-strong-parameters-concept-involvement-in-spree-2-1" target="_blank">Rails 4 - strong parameters concept involvement in spree-2.1</a>. It turns out that they referenced a simpler approach by simply placing the following into an initializer.</p>
<pre class="brush:rails">
Spree::PermittedAttributes.user_attributes.push :first_name, :last_name<br />
</pre></p>
<p>I don't think this is the best approach however, because an initializer has to be installed into the application via a generator. A generated initializer cannot be maintained either.</p>
