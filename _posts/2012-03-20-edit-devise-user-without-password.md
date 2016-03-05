---
layout: post
title: Edit Devise User without Password
date: '2012-03-20 16:38:43 -0700'
categories:
- Controller
tags:
- rails
- devise
comments: []
---
<p>I recently setup a custom controller to edit/update my Admin accounts, which are authenticated using <a href="https://github.com/plataformatec/devise" target="_blank">Plataformatec's Devise gem</a>.</p>
<p>I found <a href="https://github.com/plataformatec/devise/wiki/How-To:-Allow-users-to-edit-their-account-without-providing-a-password" target="_blank">an article in the Devise Wiki</a> that mentions using some sort of 'update_without_password' method to update the model without requiring the password. In this case I'm not requiring the user to provide their own password to edit their info. I'm allowing them to do it straight out.</p>
<p>The solution that I found was to simply remove the 'password' and 'password_confirmation' from the parameter set if both are blank.</p>
<pre class="brush:rails">
# remove password parameters if blank<br />
if params[:admin]['password'].blank? &amp;&amp; params[:admin]['confirmation'].blank?<br />
  params[:admin].delete('password')<br />
  params[:admin].delete('password_confirmation')<br />
end<br />
</pre></p>
