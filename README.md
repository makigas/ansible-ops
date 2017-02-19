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

## Files overview

Ansible:
* `makigas.yml`: the playbook for provisioning servers.
* `requirements.yml`: roles required to install via Ansible Galaxy.

Vagrant:
* `Vagrantfile`: vagrant file configuration.
* `data/`: shared folder. It can be accesed through `/vagrant` in the VM.