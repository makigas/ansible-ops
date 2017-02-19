# ansible-makigas-ops

This repository contains the Ansible playbook used for provisioning servers
for running the [makigas](https://github.com/makigas/makigas) web site. This
playbook is only used for provisioning; Capistrano is used for deployment
at this moment.

Additionally, a Vagrant box for provisioning staging servers in order to test
provisioning, deployment or backups is available.

## Requirements

Ansible 2.0+ and a server to deploy to.

Ansible requires Python 3. Additionally, support for OpenSSH at the operating
system level is required. Supported operating systems include Linux, MacOS,
FreeBSD and Solaris. Windows is not supported. None of the developers for this
project use Windows, so that's OK.

To provision a Vagrant box, [Vagrant](https://vagrantup.com) is required.
It will require a virtualization provider such as VirtualBox or VMware. The
development team only uses VirtualBox, therefore that is the only supported
platform.

## General instructions

:warning: **This playbook is not ready for production yet**. The playbook has
been tested with the provided Vagrant box but it hasn't been tested with a
real staging server yet. I'm working on it. Keeping that in mind:

1. `cp variables.yml.example variables.yml` and fill in the variables as
   you need.
1. `vagrant up` to bring the system up. The first time it should take a long
   time unless you already have a jessie64 image on your computer. It should
   already run the provisioning playbook.
1. `vagrant ssh` and test the server works as expected (i.e. you can sudo su
   as your deployment user, files and databases actually exists...)
1. You'll deploy the application using Capistrano later. You can use any
   server user you want as long as that user is in the deployment group. Make
   sure it is. For instance, for adding `vagrant` user to the `operators`
   group in order to deploy as vagrant@192.168.40.10:
   `sudo adduser vagrant operators`.
1. Test that the Capistrano schema in the web application repository can
   deploy correctly using `cap vagrant deploy`. You should be able to access
   the web application by going to http://192.168.40.10.

## Files overview

Ansible:
* `makigas.yml`: the playbook for provisioning servers.
* `requirements.yml`: roles required to install via Ansible Galaxy.
* `variables.yml`: provisioning variables; you'll probably won't find this,
  that's because...
* `variables.yml.example`: you are supposed to copy this file into variables.yml
  and set the values to the actual usernames, passwords and system information
  you want to use with the playbook and which you should totally keep private.
* `roles/`: custom roles for provisioning.

Vagrant:
* `Vagrantfile`: vagrant file configuration.
* `data/`: shared folder. It can be accesed through `/vagrant` in the VM.