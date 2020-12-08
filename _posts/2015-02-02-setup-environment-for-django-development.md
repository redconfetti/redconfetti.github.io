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

Although this website is primarily devoted to Ruby / Rails development, I've
found it necessary to learn Python for a new position I might take in the
upcoming year. Here is my guide for setting up your local workstation for Python
/ Django development on a Mac OS X workstation.
<!--more-->

## Homebrew

The first step is to ensure that you have Homebrew installed, which is a package
manager for Mac OS X that installs various software packages that are ported for
Mac OS X.

To install Homebrew run the following from your Terminals command line:

```ruby
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Once this is installed you should run 'brew doctor' and make sure that it's
setup properly. Usually I find that I have to make sure that `/usr/local/bin` is
the first path shown in `/etc/paths`. You can edit this using nano from the
command line.

```bash
sudo nano /etc/paths
```

You'll likely also have to run `brew update`. Once `brew doctor` reports 'Your
system is ready to brew', you can move forward.

For development it's important to install software packages that are provided
by Homebrew, so that all the executables and libraries you are using are
provided by Homebrew, and thus not conflicting with system libraries. Homebrew
installs executables in /usr/local/bin, which is configured to be your primary
path. This ensures that when you try to run a command it uses the Homebrew
executable and libraries rather than the default executables and libraries
provided by Mac OS X.

## Python

The next step is to install Python. By default Python v2.7.6 is already
available for Mac OS X (Yosemite), however certain programs may rely on this
version of Python to run on your system. By installing Python via Homebrew, it
will depend on other dependencies installed by Homebrew.

This command will install both python version 2 and 3.

``` bash
brew install python python3
```

After this is finished you can use 'which' to see which Python executables are
present in your environment by default.

``` bash
$ brew install python python3

$ which python
/usr/local/bin/python

$ which python3
/usr/local/bin/python3
```

As you can see, the Homebrew versions of Python will be used when you use these commands.

## VirtualEnv and VirtualEnvWrapper

Python comes with a package manager called Pip that installs Python libraries
from the [PyPI (Python Package Index)](https://pypi.python.org/pypi). By
default, this library installs packages globally for the version of Python you
are using. For instance for Python v2, you would use 'pip', and for Python v3,
you would use 'pip3' to install Python packages.

``` bash
$ which pip
/usr/local/bin/pip

$ which pip3
/usr/local/bin/pip3
```

These packages are installed globally, and available across all your projects.
This can be convenient, but it can also become a problem. For instance, one
project might require one version of Django, while another project requires
another one be installed as the primary version.

In the Ruby community this is where RVM or rbenv have been used to isolate the
environment in use when you're running a specific Ruby application, with an
isolated RubyGem gem set.

In the Python community the preferred tool is VirtualEnv and VirtualEnvWrapper.
These are both Python tools that will need to be installed globally.

``` bash
pip install virtualenv virtualenvwrapper
```

Next you'll want to make a directory to store your virtual environments under.
To keep these hidden we'll create a hidden directory under your home directory.

``` bash
mkdir ~/.virtualenvs
```

Next add the following to your .profile file in your home directory.

``` bash
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
export PIP_VIRTUALENV_BASE=$WORKON_HOME
alias workoff='deactivate'
```

You can now create a new project to work in using the following command:

``` bash
$ mkvirtualenv mydjangoapp
New python executable in mydjangoapp/bin/python2.7

Also creating executable in mydjangoapp/bin/python
Installing setuptools, pip...done.
```

Next, to work in this virtual environment, use the 'workon' command like so:

```bash
workon mydjangoapp
```

To exit the virtual environment you are in, simply use the 'deactivate' or
'workoff' command. You can remove virtual environments using the 'rmvirtualenv'
command.

To create a virtual environment using the Homebrew version of Python 3, use
this command:

```shell
mkvirtualenv -p /usr/local/bin/python3 mydjangoapp
```

## Postgres

Typically I'd use MySQL, but it looks like the open source community is
recommending adoption of Postgres. For instance, Heroku doesn't support MySQL
by default, as they found it more portable than MySQL databases.

Run the following to install Postgres, set it up to be started automatically by
the system daemon launcher (launchd), and then start the service immediately.

```shell
brew install postgres
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

Now that the Postgres server is up and running, we need to establish an empty
database and a user with all permissions for that database.

The Postgres config file is located in `/usr/local/var/postgres/pg_hba.conf`.

## Psycopg2

Django requires the Psycopg2 library to connect with Postgres databases. Go into
your virtual environment and install this package as well as the Django package.

``` shell
workon mydjangoapp
pip install psycopg2 django
```

## Django

Now we're ready to create our first Django application.

``` shell
django-admin.py startproject mydjangoapp
```

Inside of the project folder you just created will be another folder with the
same name (mydjangoapp). To Python, this simply looks like a Python module.
Django doesn't care what the name of the outer folder is, just as long as the
app folder within it holds the correct name and configuration files.

After you've created the new application folder, change into it's directory
and run the following to start the Django development server.

```shell
cd mydjangoapp
python manage.py runserver
```

If you want to run this server on another IP address or port this is possible.
See [runserver reference].

```shell
python manage.py runserver 8080
python manage.py runserver 0.0.0.0:8000
```

[runserver reference]: https://docs.djangoproject.com/en/1.6/ref/django-admin/#django-admin-runserver

## Database Setup

We need to create a database, and then a user with permissions to use the
database in PostgreSQL. Typically you would run sudo as the 'postgres', but
Postgres was installed by Homebrew to run as you, so you're the Postgres admin
user.

```bash
$ createdb mydjangoapp
$ createuser -SDR mydjangoapp

$ psql -d postgres -c "ALTER USER mydjangoapp WITH PASSWORD 'devpass';"
ALTER ROLE

$ psql -d postgres -c "GRANT ALL PRIVILEGES ON DATABASE mydjangoapp to mydjangoapp;"
GRANT
```

Inside of your application folder you'll see a 'settings.py' file. This file
holds various settings for your Django application, which technically is a
Python module. By default Django uses SQLite for the database, however we're
going to use PostgreSQL.

This requires that we change the keys in the DATABASES 'default' item inside
of settings.py.

``` python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

For our needs change this entry to reflect the following:

``` python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'mydjangoapp',
        'USER': 'mydjangoapp',
        'PASSWORD': 'devpass',
        'HOST': '',
        'PORT': '',
    }
}

```

After saving these changes, run the following command to have Python create the
needed tables inside of the database:

``` bash
$ python manage.py syncdb

Operations to perform:

  Apply all migrations: admin, auth, sessions, contenttypes

Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying sessions.0001_initial... OK

You have installed Django's auth system, and don't have any superusers defined.

Would you like to create one now? (yes/no): yes
Username (leave blank to use 'myusername'):
Email address: myusername@example.com
Password: ******
Password (again): ******
Superuser created successfully.
```

Now you're ready to start building your application. You can start by generating
a model using the following command.

``` bash
python manage.py mydjangoapp modelname
```

This is where this guide leaves off. You can continue your experimentation with
building a Django application by following the
[Writing your first Django app, part 1] tutorial from the [Creating Models]
section.

[Writing your first Django app, part 1]: https://docs.djangoproject.com/en/1.6/intro/tutorial01/
[Creating Models]: https://docs.djangoproject.com/en/1.6/intro/tutorial01/#creating-models
