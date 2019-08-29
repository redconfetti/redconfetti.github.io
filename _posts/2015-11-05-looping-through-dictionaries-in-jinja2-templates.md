---
layout: post
title: Looping through dictionaries in jinja2 templates
date: "2015-11-05 02:10:32 -0800"
categories:
  - Python
tags:
  - jinja2
  - ansible
  - templates
comments: []
---

I am adding a script to our server using Ansible. The roles are all setup to
support multiple Wordpress websites based on the dictionary defined in
`ansible/group_vars/wordpress_sites.yml`, as my Ansible configuration is based
on [Trellis](https://github.com/roots/trellis/){:target="\_blank"}.

I don't want to use the
[Ansible template module](http://docs.ansible.com/ansible/template_module.html){:target="\_blank"}
to create a script for every website, because really I only have one website
configured. Sure I might have configuration files for each site under [Nginx],
so that makes sense. So I decided that instead of creating multiple scripts,
I'll just have Ansible generate scripting for each of the sites inside of my shell script.

Well it turns out that this isn't do easy for someone not very familiar with
Jinja2 templates or Python objects.

At first I figured that I would simply loop through each element inside of the
'wordpress_sites' dictionary like so:

```
{% raw %}
{% for site in wordpress_sites.values() %}
echo "-------------------------------------------------------"
echo "| Backing Up Assets and Database for {{ site.key }} |"
echo "-------------------------------------------------------"
echo ""
read -s -p "MySQL Password for '{{ site.env.db_user }}': " mysqlpw
# Other script code goes here for each site
{% endfor %}
{% endraw %}

```

Unfortunately when I'd run the script to generate this site I would get this
error:

```shell
TASK: [admin-scripts | Add Backup script] *************************************
fatal: [45.79.167.52] => {'msg': "AnsibleUndefinedVariable: One or more undefined variables: 'str object' has no attribute 'env'", 'failed': True}
fatal: [45.79.167.52] => {'msg': "AnsibleUndefinedVariable: One or more undefined variables: 'str object' has no attribute 'env'", 'failed': True}
```

This was very frustrating, as I just expected each object to have yet another
dictionary object as it's value. Why am I getting a string object?

After much experimentation (trial and error), I realized that I could traverse
through the dictionary manually.

```
{% raw %}
{{ wordpress_sites }}
{{ wordpress_sites['mysite.example.com'].env }}
{{ wordpress_sites['mysite.example.com'].env.db_user }}
{{ wordpress_sites['mysite.example.com'].env.doesnt_exist }}
{% endraw %}
```

This resulted in an error about no attribute 'doesnt_exist', so I knew that the
global variable is there and accessible. So there must be something wrong with
the loop that is converting the value to a string. I found
[an article](http://stackoverflow.com/questions/29065243/jinja2-convert-string-to-dict-object){:target="\_blank"} that used `dict.values()`, implying that you could call
values() on a dictionary, and it would return the values.

I guess that a for loop inside of a jinja2 template expects a list, not a
dictionary.

```
{% raw %}
{% for site in wordpress_sites.values() %}
echo "-------------------------------------------------------"
echo "| Backing Up Assets and Database for {{ site.key }} |"
echo "-------------------------------------------------------"
echo ""
read -s -p "MySQL Password for '{{ site.env.db_user }}': " mysqlpw
# Other script code goes here for each site
{% endfor %}
{% endraw %}

```

This still left me unable to access the key that was used to represent the site,
which is needed by my script. I tested
[a configuration that iterated with key and value](http://blog.mattcrampton.com/post/31254835293/iterating-over-a-dict-in-a-jinja-template){:target="\_blank"} available inside
of the code block.

```
{% raw %}
{% for site_name, site in wordpress_sites.iteritems() %}
echo "-------------------------------------------------------"
echo "| Backing Up Assets and Database for {{ site_name }} |"
echo "-------------------------------------------------------"
echo ""
read -s -p "MySQL Password for '{{ site.env.db_user }}': " mysqlpw
echo ""
{% endfor %}
{% endraw %}
```

This resolved my issue.

I wonder if this is the type of thing that's a no brainer to a Python developer.
Probably.

[nginx]: https://www.nginx.com/
