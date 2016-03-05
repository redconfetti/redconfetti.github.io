---
layout: post
title: Setup Environment for Django Development
date: '2015-02-02 00:26:28 -0800'
categories:
- Python
tags:
- python
- django
- virtualenv
- pip
---

<p>Although this website is primarily devoted to Ruby / Rails development, I've found it necessary to learn Python for a new position I might take in the upcoming year. Here is my guide for setting up your local workstation for Python / Django development on a Mac OS X workstation.</p>

<h2>Homebrew</h2>

<p>
  The first step is to ensure that you have Homebrew installed, which is a package manager for Mac OS X that 
  installs various software packages that are ported for Mac OS X.</p>
<p>To install Homebrew run the following from your Terminals command line:</p>

{% highlight ruby %}
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

Once this is installed you should run 'brew doctor' and make sure that it's setup properly. Usually I find that I have to make sure that '/usr/local/bin' is the first path shown in /etc/paths. You can edit this using nano from the command line.</p>

{% highlight bash %}
  $ sudo nano /etc/paths
{% endhighlight %}

You'll likely also have to run 'brew update'. Once 'brew doctor' reports 'Your system is ready to brew', you can move forward.</p>
<p>For development it's important to install software packages that are provided by Homebrew, so that all the executables and libraries you are using are provided by Homebrew, and thus not conflicting with system libraries. Homebrew installs executables in /usr/local/bin, which is configured to be your primary path. This ensures that when you try to run a command it uses the Homebrew executable and libraries rather than the default executables and libraries provided by Mac OS X.</p>
<h2>Python</h2>
<p>The next step is to install Python. By default Python v2.7.6 is already available for Mac OS X (Yosemite), however certain programs may rely on this version of Python to run on your system. By installing Python via Homebrew, it will depend on other dependencies installed by Homebrew.</p>
<p>This command will install both python version 2 and 3.</p>

{% highlight bash %}
$ brew install python python3
{% endhighlight %}

<p>After this is finished you can use 'which' to see which Python executables are present in your environment by default.</p>

{% highlight bash %}
$ brew install python python3
$ which python
/usr/local/bin/python
$ which python3
/usr/local/bin/python3
{% endhighlight %}

<p>As you can see, the Homebrew versions of Python will be used when you use these commands.</p>
<h2>VirtualEnv and VirtualEnvWrapper</h2></p>
<p>Python comes with a package manager called Pip that installs Python libraries from the <a href="https://pypi.python.org/pypi" target="_blank">PyPI (Python Package Index)</a> </p>
<p>By default, this library installs packages globally for the version of Python you are using. For instance for Python v2, you would use 'pip', and for Python v3, you would use 'pip3' to install Python packages.</p>
<pre class="brush:shell">
$ which pip<br />
/usr/local/bin/pip<br />
$ which pip3<br />
/usr/local/bin/pip3<br />
</pre></p>
<p>These packages are installed globally, and available across all your  projects. This can be convenient, but it can also become a problem. For instance, one project might require one version of Django, while another project requires another one be installed as the primary version.</p>
<p>In the Ruby community this is where RVM or rbenv have been used to isolate the environment in use when you're running a specific Ruby application, with an isolated RubyGem gem set.</p>
<p>In the Python community the preferred tool is VirtualEnv and VirtualEnvWrapper. These are both Python tools that will need to be installed globally.</p>
<pre class="brush:shell">
$ pip install virtualenv virtualenvwrapper<br />
</pre></p>
<p>Next you'll want to make a directory to store your virtual environments under. To keep these hidden we'll create a hidden directory under your home directory.</p>
<pre class="brush:shell">
mkdir ~/.virtualenvs<br />
</pre></p>
<p>Next add the following to your .profile file in your home directory.</p>
<pre class="brush:shell">
export WORKON_HOME=$HOME/.virtualenvs<br />
source /usr/local/bin/virtualenvwrapper.sh<br />
export PIP_VIRTUALENV_BASE=$WORKON_HOME<br />
alias workoff='deactivate'<br />
</pre></p>
<p>You can now create a new project to work in using the following command:</p>
<pre class="brush:shell">
$ mkvirtualenv mydjangoapp<br />
New python executable in mydjangoapp/bin/python2.7<br />
Also creating executable in mydjangoapp/bin/python<br />
Installing setuptools, pip...done.<br />
</pre></p>
<p>Next, to work in this virtual environment, use the 'workon' command like so:</p>
<pre class="brush:shell">
$ workon mydjangoapp<br />
</pre></p>
<p>To exit the virtual environment you are in, simply use the 'deactivate' or 'workoff' command. You can remove virtual environments using the 'rmvirtualenv' command.</p>
<p>To create a virtual environment using the Homebrew version of Python 3, use this command:</p>
<pre class="brush:shell">
$ mkvirtualenv -p /usr/local/bin/python3 mydjangoapp<br />
</pre></p>
<h2>Postgres</h2></p>
<p>Typically I'd use MySQL, but it looks like the open source community is recommending adoption of Postgres. For instance, Heroku doesn't support MySQL by default, as they found it more portable than MySQL databases.</p>
<p>Run the following to install Postgres, set it up to be started automatically by the system daemon launcher (launchd), and then start the service immediately.</p>
<pre class="brush:shell">
$ brew install postgres<br />
$ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents<br />
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist<br />
</pre></p>
<p>Now that the Postgres server is up and running, we need to establish an empty database and a user with all permissions for that database.</p>
<p>The Postgres config file is located in /usr/local/var/postgres/pg_hba.conf</p>
<h2>Psycopg2</h2></p>
<p>Django requires the Psycopg2 library to connect with Postgres databases. Go into your virtual environment and install this package as well as the Django package.</p>
<pre class="brush:shell">
$ workon mydjangoapp<br />
$ pip install psycopg2 django<br />
</pre></p>
<h2>Django</h2></p>
<p>Now we're ready to create our first Django application.</p>
<pre class="brush:shell">
$ django-admin.py startproject mydjangoapp<br />
</pre></p>
<p>Inside of the project folder you just created will be another folder with the same name (mydjangoapp). To Python, this simply looks like a Python module. Django doesn't care what the name of the outer folder is, just as long as the app folder within it holds the correct name and configuration files.</p>
<p>After you've created the new application folder, change into it's directory and run the following to start the Django development server.</p>
<pre class="brush:shell">
$ cd mydjangoapp<br />
$ python manage.py runserver<br />
</pre></p>
<p>If you want to run this server on another IP address or port this is possible. See <a href="https://docs.djangoproject.com/en/1.6/ref/django-admin/#django-admin-runserver" target="_blank">runserver reference</a>.</p>
<pre class="brush:shell">
$ python manage.py runserver 8080<br />
$ python manage.py runserver 0.0.0.0:8000<br />
</pre></p>
<h2>Database Setup</h2></p>
<p>We need to create a database, and then a user with permissions to use the database in PostgreSQL. Typically you would run sudo as the 'postgres', but Postgres was installed by Homebrew to run as you, so you're the Postgres admin user.</p>
<pre class="brush:shell">
$ createdb mydjangoapp<br />
$ createuser -SDR mydjangoapp<br />
$ psql -d postgres -c "ALTER USER mydjangoapp WITH PASSWORD 'devpass';"<br />
ALTER ROLE<br />
$ psql -d postgres -c "GRANT ALL PRIVILEGES ON DATABASE mydjangoapp to mydjangoapp;"<br />
GRANT<br />
</pre></p>
<p>Inside of your application folder you'll see a 'settings.py' file. This file holds various settings for your Django application, which technically is a Python module. By default Django uses SQLite for the database, however we're going to use PostgreSQL.</p>
<p>This requires that we change the keys in the DATABASES 'default' item inside of settings.py.</p>
<pre class="brush:python">
DATABASES = {<br />
    'default': {<br />
        'ENGINE': 'django.db.backends.sqlite3',<br />
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),<br />
    }<br />
}<br />
</pre></p>
<p>For our needs change this entry to reflect the following:</p>
<pre class="brush:python">
DATABASES = {<br />
    'default': {<br />
        'ENGINE': 'django.db.backends.postgresql_psycopg2',<br />
        'NAME': 'mydjangoapp',<br />
        'USER': 'mydjangoapp',<br />
        'PASSWORD': 'devpass',<br />
        'HOST': '',<br />
        'PORT': '',<br />
    }<br />
}<br />
</pre></p>
<p>After saving these changes, run the following command to have Python create the needed tables inside of the database:</p>
<pre class="brush:shell">
$ python manage.py syncdb<br />
Operations to perform:<br />
  Apply all migrations: admin, auth, sessions, contenttypes<br />
Running migrations:<br />
  Applying contenttypes.0001_initial... OK<br />
  Applying auth.0001_initial... OK<br />
  Applying admin.0001_initial... OK<br />
  Applying sessions.0001_initial... OK</p>
<p>You have installed Django's auth system, and don't have any superusers defined.<br />
Would you like to create one now? (yes/no): yes<br />
Username (leave blank to use 'myusername'):<br />
Email address: myusername@example.com<br />
Password: ******<br />
Password (again): ******<br />
Superuser created successfully.<br />
</pre></p>
<p>Now you're ready to start building your application. You can start by generating a model using the following command.</p>
<pre class="brush:shell">
$ python manage.py mydjangoapp modelname<br />
</pre></p>
<p>This is where this guide leaves off. You can continue your experimentation with building a Django application by following the <a href="https://docs.djangoproject.com/en/1.6/intro/tutorial01/" target="_blank">Writing your first Django app, part 1</a> tutorial from the <a href="https://docs.djangoproject.com/en/1.6/intro/tutorial01/#creating-models" target="_blank">Creating Models</a> section.</p>
