---
layout: post
published: false
title: Low Budget Static Hosting
date: 2021-08-15 07:19:55 -0700
comments: true
categories:
  - hosting
tags:
  - static
  - hosting
  - ansible
  - digital ocean
---

## Goal

In [a previous article], I raved about the benefits of the [JAMStack] approach
to website development and publishing. You can host your code repository for
free on Github or Bitbucket, and have a service like [Render] perform your
builds and host your static website for free. But I don't like depending on
a service to perform my builds and hosting, I'd rather configure a server and
software to perform the same function.
<!--more-->

I want to control the configuration of the server using Ansible,
so that I can control not only the code running on the server, but the server
configuration itself, using files tracked in a Git repository.

My goal is to use a $5 a month Droplet hosted by Digital Ocean to host my
static website (the "production" environment), while using a local virtual
server to test my Ansible configuration and application code (the "development"
environment).

This solution involves three repositories I'm hosting from Github.

1. [redconfetti/server-config] - Ansible repository used to configure the server
2. [redconfetti/site-builder] - Site Builder script programmed in Ruby
3. [redconfetti/eleventy-example] - Static site using the [Eleventy] site generator

[redconfetti/server-config]: https://github.com/redconfetti/server-config
[redconfetti/site-builder]: https://github.com/redconfetti/site-builder
[redconfetti/eleventy-example]: https://github.com/redconfetti/eleventy-example

<!--more-->

## Server Config Repository

I've setup a new repository, [redconfetti/server-config], to store the
configuration for the server.

### EditorConfig

I like my code to use 2 spaces for each 'tab', so I'm using the EditorConfig
extension to enforce that in the [Visual Studio Code] editor (aka VSCode) I'm
using. I place a `.editorconfig` file in the root directory:

```ini
; editorconfig.org
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[*.md]
max_line_length = 80
trim_trailing_whitespace = false
```

I also create `.vscode/extensions.json` to make the VSCode editor suggest
the installation of this plugin.

```json
{
  "recommendations": [
    "EditorConfig.editorconfig"
  ]
}
```

[Visual Studio Code]: https://code.visualstudio.com/

### Install Tools

On my Mac OSX machine, I use [Homebrew] to install software packages.
I create a 'Brewfile' in the root directory to specify the tools that are
needed.

```bash
# Brewfile
# Run 'brew bundle' to install

cask "vagrant"
cask "virtualbox"
brew "ansible"
```

I'm using [Vagrant] with [Virtualbox] to setup my local development server.
Vagrant orchestrates the setup and running of a virtual machine (VM) inside
of Virtualbox. Even though Virtualbox can run in a GUI window, Vagrant ensures
that it runs headless (without a window showing on your screen).

We will be using [Ansible] as a tool to store how we want the server to be
setup and configured. We specify the state of the server in a structure
of files and folders, using mostly YAML files, and then run Ansible commands
to apply changes to the server. Ansible uses [modules] that read the
configuration from the YAML files and apply changes to the state of the server
if needed. For instance, if we have a configuration that specified that a
user named 'website' should exist on the server, and Ansible notices that
the user already exists, it will not try to add the user once again. Ansible
operations (called tasks) are intended to be idempotent (when performed multiple
times, have no further effect on the server after the first time they are
performed).

I run `brew bundle` to install these dependencies specified in the `Brewfile`.

[Homebrew]: https://brew.sh/
[Ansible]: https://www.ansible.com/
[modules]: https://docs.ansible.com/ansible/2.7/user_guide/modules.html
[Vagrant]: https://www.vagrantup.com/
[VirtualBox]: https://www.virtualbox.org/

### Git Ignore

Create a `.gitignore` file that tells Git to ignore certain folders or files
that are generated, with the following contents:

```ini
# Vagrant
.vagrant
```

This will ensure that the Vagrant generated files are not checked into your
repository.

## Vagrant Setup

### VagrantFile

I run `vagrant init` to create a [Vagrantfile] in the root directory. This
file contains the configuration for the virtual server that will be created
and started within Virtualbox by Vagrant. Vagrant is a tool used by developers
to run and test their application code within a virtualized Linux environment
that matches the same configuration as the production servers that the same
code is intended to be run from. This practice ensures lower risk of bugs
with the software being developed, and a much higher probability of reproducing
the bugs in the local development environment so they can be
investigated and fixed quickly.

The Vagrantfile generated uses Ruby code for its configuration. It should
contain many comments to give you an idea of the options supported in the
Vagrantfile.

One comment points me to the [Vagrant Box Search] page. I use this to identify
a [Debian 10] image that I can use to match the Debian 10 Droplet I'm going to
setup with DigitalOcean. I update the `config.vm.box` setting to reflect the
name of this image.

Here's how my Vagrant file is configured (without comments):

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "generic/debian10"
end
```

I uncomment the following line so that I can go to [http://localhost:8080] later
to test the Nginx server.

[http://localhost:8080]: http://localhost:8080

```ruby
config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
```

Because we are using Virtualbox to manage the virtual server, we can add
[Virtualbox specific configuration]. I'm giving the VM the name 'static-dev',
and configuring some other options to limit the resources used by the VM.

```ruby
Vagrant.configure("2") do |config|
  # use Debian 10 image
  config.vm.box = "generic/debian10"

  # forward http://localhost:8080 to virtual machine on port 80
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # virtualbox configs
  config.vm.provider "virtualbox" do |v|
    v.name = "static-dev"
    v.linked_clone = true
    v.check_guest_additions = false
    v.memory = 1024
    v.cpus = 2
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
  end
end
```

Now, if I simply run `vagrant up`, Vagrant will work with Virtualbox to
download the image for the VM, and then run it.

__Note:__ I ran into an issue on my Mac concerning an error:
"VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005)". See
[VBoxManage error solved] for the solution to this issue.

You should see output like the following:

```bash
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Checking if box 'generic/debian10' version '3.3.2' is up to date...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 80 (guest) => 8080 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
```

You don't see it, but there is a virtual machine running in VirtualBox.
You can even open the VirtualBox application and you'll see the 'static-dev'
server that was setup, and the 'generic-debian-10-virtualbox' image.

![Virtualbox with static-dev and Debian 10](/images/posts/2021-08-15-static-sites-hosting-virtualbox.jpg "Virtualbox with static-dev and Debian 10")

If you run 'vagrant ssh' you'll be logged into the server as the 'vagrant'
user.

```bash
$ vagrant ssh
Last login: Mon Jul 26 20:32:08 2021 from 10.0.2.2
vagrant@debian10:~$
```

[Vagrantfile]: https://www.vagrantup.com/docs/vagrantfile
[Vagrant Box Search]: https://app.vagrantup.com/boxes/search
[Debian 10]: https://app.vagrantup.com/generic/boxes/debian10
[Virtualbox specific configuration]: https://www.vagrantup.com/docs/providers/virtualbox/configuration
[VBoxManage error solved]: https://scriptcrunch.com/solved-vboxmanage-error-component-machinewrap/

## Ansible Setup

Now that we have the server setup as our foundation, we can begin to
[provision the server using Ansible]. In the `Vagrantfile`, add the
following configuration.

```ruby
# Run Ansible from the Vagrant Host
config.vm.provision "ansible" do |ansible|
 ansible.playbook = "server.yml"
end
```

Next, create a `server.yml` file in the root directory with the following
contents:

```yaml
# Ansible Playbook for Local Workstation
# Used by Vagrant via Ansible Provisioner
---
- hosts: all
  tasks:
    - name: Update Apt
      become: true
      apt:
        update_cache: yes
        upgrade: full
```

How that we have a basic configuration for Vagrant to run Ansible on the local
server. The task defined above named "Update Apt" uses the [apt module] to
update the package cache (equivalent of the `apt-get update` command),
and also upgrades all existing software packages (equivalent of the
`apt-get upgrade` command). As a result, this single task ensures that our
server is running the most up-to-date software packages.

Now that this is in place, run the `vagrant provision` command to run the
`server.yml` playbook against the server.

You should see output as follows:

```bash
$ vagrant provision
==> default: Running provisioner: ansible...
    default: Running ansible-playbook...

PLAY [local] *******************************************************************

TASK [Gathering Facts] *********************************************************
ok: [default]
[DEPRECATION WARNING]: Distribution debian 10.10 on host default should use
/usr/bin/python3, but is using /usr/bin/python for backward compatibility with
prior Ansible releases. A future Ansible release will default to using the
discovered platform python for this host. See https://docs.ansible.com/ansible/
2.11/reference_appendices/interpreter_discovery.html for more information. This
 feature will be removed in version 2.12. Deprecation warnings can be disabled
by setting deprecation_warnings=False in ansible.cfg.

TASK [Update Apt] **************************************************************
ok: [default]

PLAY RECAP *********************************************************************
default                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

To avoid the above deprecation warning, create an Ansible Configuration file
(`ansible.cfg`) file with the following:

```ini
[defaults]
interpreter_python=/usr/bin/python3
```

Run 'vagrant provision' once again, and you'll get the same output without
the deprecation warning.

[apt module]: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html
[provision the server using Ansible]: https://www.vagrantup.com/docs/provisioning/ansible_intro

## Hosts Inventory

### Vagrant Auto-Generated Inventory

Ansible is designed to provision changes to one or many hosts. Just the same
Vagrant is able to be configured to build and run multiple virtual hosts
locally. It doesn't make sense for that configuration to be defined twice (once
for Vagrant, and again for Ansible). It is because of this that Vagrant
generates its own host inventory configuration that it uses with Ansible
playbooks.

If you want to see the inventory configuration that is generated by Vagrant for
your current configuration, you can inspect
`.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory`.

See [Vagrant - Ansible Intro - Auto-generated inventory] for more information
on how the Vagrant configuration translates to the inventory used by Ansible
when provisioning locally managed virtual machines using Vagrant.

[Vagrant - Ansible Intro - Auto-generated inventory]: https://www.vagrantup.com/docs/provisioning/ansible_intro#auto-generated-inventory

### Non-Vagrant Inventory

For hosts not provisioned with Vagrant, we need to define them in an Ansible
host inventory.

Ansible is designed to maintain the one or many hosts. Large organizations are
likely to use it to control a large fleet of servers. In such an
environment it is logical to configure a single server as the heavily secured
[control node] that is whitelisted with SSH access to all the machines in the
cluster of nodes that host the companies application(s).

Ansible supports an [inventory] of all the hosts that your Ansible configuration
can be applied to. By default this is stored globally within
`/etc/ansible/hosts`. This makes sense for administrators setting up a control
node. You can define an inventory file anywhere, and simply use
`-i <path to file>` when running a command. You can even configure
Ansible to pull the hosts from a [dynamic inventory], such as an LDAP server.

For our purposes the control node is our own personal workstation
at home sitting behind a consumer grade Wifi router/modem/firewall, hopefully
unexposed to public requests. So instead of using the default configurations,
we're setting everything up in our own local repository, which requires some
special configurations.

We need to tell Ansible to use the hosts file in our repository, so that it
does not look for the inventory in `/etc/ansible/hosts`. We do this by
creating an `ansible.cfg` file.

```ini
[defaults]
interpreter_python=/usr/bin/python3
inventory = hosts.yml
```

This hosts file won't be used by Vagrant when provisioning to the local server,
but it will be used when we run Ansible playbooks that don't involve Vagrant.

Next create a `hosts.yml` file with the following:

```yaml
all:
  hosts:
    production:
      ansible_host: 104.248.68.213
      ansible_connection: ssh
      ansible_user: admin
      ansible_ssh_port: 22
```

[inventory]: https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#intro-inventory
[control node]: https://docs.ansible.com/ansible/latest/network/getting_started/basic_concepts.html#control-node
[dynamic inventory]: https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html

### Running Playbook in Production

If your hosts.yml is configured with the expected username, SSH configuration is
setup properly for SSH key based authentication, and the 'become' password is
provided is correct, you should see output such as the following:

```shell
$ ansible-playbook server.yml --ask-become-pass
BECOME password:

PLAY [all] ************************************************************************************

TASK [Gathering Facts] ************************************************************************
ok: [production]

TASK [Update Apt] *****************************************************************************
changed: [production]

PLAY RECAP ************************************************************************************
production                 : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Your playbook is now applying to the other servers you specify.

If you want to run a playbook against a specific host, you can run the same
playbook with the hosts specified like so.

```shell
ansible-playbook server.yml --ask-become-pass -l production
```

If you have another host you want to run your playbook against, such as a
'staging' server, this command can be useful.

## Roles

So far we've defined a single task in our playbook. We could continue to
define tasks in a single playbook file, but I would rather break out our
configuration into different sets that setup the server to perform a specific
role (web server, database server, etc).

Ansible uses the same term, [roles], to organize the configuration.

It's typical for a role to be named 'common' with the tasks that are applied
across all the servers. This includes tasks like updating the systems packages,
syncing the system date/time from an NTP server, etc.

Here is what we're wanting to do with our server:

- Common
  - Establish Admin User
  - Secure the server
- Web Server
  - Establish Web User (www-data)
  - Install Nginx
  - Configure Static Site
  - Setup a site builder service

[roles]: https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html

Ansible requires three things to have full access to configure the server as
needed:

1. SSH access
2. sudo permission
3. A Python 2 interpreter

[Eleventy]: https://www.11ty.dev/
[a previous article]: /2020/12/jamstack-websites/
[JAMStack]: https://jamstack.org/
[Render]: https://render.com/

## Reference

- [Ansible Docs - Writing Tasks Plays and Playbooks][]
- [Ansible Docs - Playbook Blocks][]
- [Ansible Docs - Tips and Tricks][]
- [Ansible Docs - Encrypting Vault][]
- [Ansible][roles]
- [Ansible Galaxy - ssh-hardening][]
- [How to harden SSH configs with Ansible on Linux][]
- [5 ways to harden a new system with Ansible][]
- [redconfetti/config]
- [roots/trellis]

[Ansible Docs - Writing Tasks Plays and Playbooks]: https://docs.ansible.com/ansible/latest/user_guide/index.html#writing-tasks-plays-and-playbooks
[Ansible Docs - Playbook Blocks]: https://docs.ansible.com/ansible/latest/user_guide/playbooks_blocks.html
[Ansible Docs - Tips and Tricks]: https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#playbooks-tips-and-tricks
[Ansible Galaxy - ssh-hardening]: https://galaxy.ansible.com/dev-sec/ssh-hardening
[Ansible Docs - Encrypting Vault]: https://docs.ansible.com/ansible/latest/user_guide/vault.html#vault
[How to harden SSH configs with Ansible on Linux]: https://ulayer.net/blog/2019/08/02/how-to-harden-ssh-configs-with-ansible-on-linux/
[5 ways to harden a new system with Ansible]: https://www.redhat.com/sysadmin/harden-new-system-ansible
[redconfetti/config]: https://github.com/redconfetti/config
[roots/trellis]: https://github.com/roots/trellis
