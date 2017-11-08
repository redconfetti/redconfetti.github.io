---
layout: page
title: Cheat Sheets
---

Back to [Resources](/resources/)

Here are my own personal cheat sheets. I've organized some into their own pages, or simply added a few commands below.

* [Git](/resources/cheat-sheets/git/)
* [RVM](/resources/cheat-sheets/rvm/)
* [RubyGems](/resources/cheat-sheets/rubygems/)
* [Rails Tests](/resources/cheat-sheets/rails-tests/)
* [MySQL](/resources/cheat-sheets/mysql/)
* [PostgreSQL](/resources/cheat-sheets/postgresql/)
* [Vagrant](/resources/cheat-sheets/vagrant/)
* [Docker](/resources/cheat-sheets/docker/)
* [Node Package Manager (NPM)](/resources/cheat-sheets/npm/)
* [Linux](/resources/cheat-sheets/linux/)
* [Mac OS](/resources/cheat-sheets/mac-os/)

## ActiveRecord

```
# Get name of table associated with model
Model.table_name
# Get field/column names from database table
Model.column_names
```

## Capistrano

```
# View available Capistrano tasks
bundle exec cap -vT
```

## Postgres
```
# ---------------------------
# Shell Commands
# ---------------------------

# Open Console (database name required, use 'postgres' database first time)
$ psql database_name
$ psql myapp_development

# ---------------------------
# Console Commands
# ---------------------------

# List Databases
\list

# Switch to another database
\c database_name

# List tables
\dt

# Quit Postgres Console
\q
```

## PGP Encryption / Decryption
See [Introduction to GnuPG](http://www.ianatkinson.net/computing/gnupg.htm){:target="_blank"} for more detail.

``` shell
# install using homebrew
brew install gpg

# generate your personal key
gpg --gen-key

# list keys
gpg --list-keys

# list secret keys
gpg --list-secret-keys

# encrypt a file (requires specifying recipient)
gpg -e -r jsmith@example.com secret.txt

# decrypt and view encrypted file contents
gpg -d secret.txt.gpg

# decrypt and save file contents to a new file
gpg -d -o secret.txt secret.txt.gpg

# import a persons public key
gpg --import publickey.txt

# get ASCII-armored public key
gpg --output publickey.txt --armor --export jsmith@example.com
```
