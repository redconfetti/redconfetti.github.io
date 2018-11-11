---
layout: page
title: Vagrant
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

``` shell
# creates virtual machine (VM) for first time, or starts the VM if it's not running
vagrant up

# view the status of the vagrant VM associated with the current project
vagrant status

# shuts down VM
vagrant halt

# hibernate the VM
vagrant suspend

# take VM out of hibernation
vagrant resume

# outputs the status of all VMs for current user
vagrant global-status

# output the status of the VM for the current project
vagrant status

# stop and delete VM
vagrant destroy

# reapply configuration from Vagrantfile, restart VM
vagrant reload

# connect to VM via SSH
vagrant ssh
```
