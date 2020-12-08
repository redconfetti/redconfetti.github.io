---
layout: post
title: Strong Parameters with Spree Extensions
date: '2014-04-20 01:05:29 -0700'
comments: true
categories:
- Ruby on Rails
- Gems
tags:
- Spree
---
I'm currently working on an extension for [Spree], an e-commerce solution for
Ruby on Rails applications. The [developer documentation] for Spree is very
helpful, letting developers know that they should use certain Ruby
meta-programming methods to extend the functionality of the Spree system. The
extension I'm working on was setup under a version of Spree that used Rails 3.

Now that Spree v2.2.1 uses Rails 4.0.4, I'm having to refactor some parts of
this extension to adapt to new practices.

[Spree]: https://github.com/spree/spree
[developer documentation]: http://guides.spreecommerce.com/developer/
<!--more-->

### From Accessible Attributes to Strong Parameters

Under Rails 3, you have to use [#attr_accessible] to ensure that attributes of a
model can be updated via methods such as [#update_attributes]. This was
implemented to protect models from mass assignment vulnerability. In Rails 4,
this functionality re-implemented as a convention at the controller level, in a
feature known as [Strong Parameters]. This
[Rails 4 Quick Look: Strong Parameters] article explains this clearly.

[#attr_accessible]: http://apidock.com/rails/ActiveModel/MassAssignmentSecurity/ClassMethods/attr_accessible
[#update_attributes]: http://apidock.com/rails/ActiveResource/Base/update_attributes
[Strong Parameters]: http://apidock.com/rails/ActionController/StrongParameters
[Rails 4 Quick Look: Strong Parameters]: http://www.sitepoint.com/rails-4-quick-look-strong-parameters/

### Rails 3 Decorators

Under Rails 3, a Spree extension could introduce new columns to Spree models via
a migration, and then simply introduce a decorator like this one to make the new
attributes available for mass assignment updates.

``` ruby
Spree::Order.class_eval do
  attr_accessible :my_extensions_attribute, :my_extensions_attribute2
end
```

But now with Rails 4, I have to create a decorator for
Spree::Api::OrdersController instead. I imagined that this decorator will have
to somehow apply a call to the [#permit] method on the 'params', allowing my
extension attributes to be updated as well.

After some searching online I realized that the best solution to this problem is
to specify an [alias_method_chain] inside my decorator. We don't have control
over the Spree code, and there is no option for using 'super' because
inheritance isn't involved here. So this is definitely a situation where we
[should use an alias method chain].

[#permit]: http://apidock.com/rails/ActionController/Parameters/permit
[alias_method_chain]: http://apidock.com/rails/Module/alias_method_chain
[should use an alias method chain]: http://erniemiller.org/2011/02/03/when-to-use-alias_method_chain/

### Spree Controller Helpers for Strong Parameters

I just noticed however that the [Spree::Api::OrdersController#order_params]
method has a more complex method for permitting the attributes than I expected.
In this case the order attributes are provided by [#permitted_order_attributes],
which makes a 'super' call that refers to the parent controller
[Spree::Api::BaseController]. The BaseController doesn't have a
\#permitted_order_attributes method defined, however it does include
[Spree::Core::ControllerHelpers::StrongParameters]. which defines
[#permitted_order_attributes]. If you follow the dependencies further, you'll
see that all these methods in Spree::Core::ControllerHelpers::StrongParameters
rely on [Spree:PermittedAttributes].

So all that is necessary to define a new Spree::Order attribute is to define a
Spree::PermittedAttributes decorator like so:

```ruby
# lib/spree/permitted_attributes_decorator.rb
Spree::PermittedAttributes.class_eval do
  @@checkout_attributes.push(:my_extensions_attribute, :my_extensions_attribute2)
end
```

I'll have to test this out, but it seems like the plausible approach. I hope
this helps any other developers.

[Spree::Api::OrdersController#order_params]: https://github.com/spree/spree/blob/e2bd38d4/api/app/controllers/spree/api/orders_controller.rb#L93
[#permitted_order_attributes]: https://github.com/spree/spree/blob/e2bd38d4/api/app/controllers/spree/api/orders_controller.rb#L107
[Spree::Api::BaseController]: https://github.com/spree/spree/blob/e2bd38d4/api/app/controllers/spree/api/base_controller.rb
[Spree::Core::ControllerHelpers::StrongParameters]: https://github.com/spree/spree/blob/e2bd38d4/core/lib/spree/core/controller_helpers/strong_parameters.rb
[#permitted_order_attributes]: https://github.com/spree/spree/blob/e2bd38d4/core/lib/spree/core/controller_helpers/strong_parameters.rb#L28
[Spree:PermittedAttributes]: https://github.com/spree/spree/blob/e2bd38d4/core/lib/spree/permitted_attributes.rb

### Update

I just went back to a StackOverflow article on this subject that I had seen
before - [Rails 4 - strong parameters concept involvement in spree-2.1]. It
turns out that they referenced a simpler approach by simply placing the
following into an initializer.

``` ruby
Spree::PermittedAttributes.user_attributes.push :first_name, :last_name
```

I don't think this is the best approach however, because an initializer has to
be installed into the application via a generator. A generated initializer
cannot be maintained either.

[Rails 4 - strong parameters concept involvement in spree-2.1]: http://stackoverflow.com/questions/19924702/rails-4-strong-parameters-concept-involvement-in-spree-2-1
