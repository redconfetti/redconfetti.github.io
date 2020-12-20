---
layout: post
title: Edit Devise User without Password
date: '2012-03-20 16:38:43 -0700'
comments: true
categories:
- Controller
tags:
- rails
- devise
---

I recently setup a custom controller to edit/update my Admin accounts, which are
authenticated using [Plataformatec's Devise gem][1].

I found [an article in the Devise Wiki][2] that mentions using some sort of
'update_without_password' method to update the model without requiring the
password. In this case I'm not requiring the user to provide their own
password to edit their info. I'm allowing them to do it straight out.

The solution that I found was to simply remove the 'password' and
'password_confirmation' from the parameter set if both are blank.

``` ruby
# remove password parameters if blank
if params[:admin]['password'].blank? &amp;&amp; params[:admin]['confirmation'].blank?
  params[:admin].delete('password')
  params[:admin].delete('password_confirmation')
end
```

[1]: https://github.com/plataformatec/devise
[2]: https://github.com/plataformatec/devise/wiki/How-To:-Allow-users-to-edit-their-account-without-providing-a-password
