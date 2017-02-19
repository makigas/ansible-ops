# ansible-makigas-ops

This repository contains the Ansible playbook used for provisioning servers
for running the [makigas](https://github.com/makigas/makigas) web site. This
playbook is only used for provisioning; Capistrano is used for deployment
at this moment.

Additionally, a Vagrant box for provisioning staging servers in order to test
provisioning, deployment or backups is available.

## Requirements

Ansible 2.2+ and a server to deploy to.

The control machine (laptop, desktop, workstation) requires Python 2.6+ and
either a GNU/Linux flavour such as Debian or CentOS or any other UNIX OS such
as MacOS X or FreeBSD. Windows is not supported for the control machine.

The managed machine requires Python 2.4+, OpenSSH.

To provision a Vagrant box, [Vagrant](https://vagrantup.com) is required.
It will require a virtualization provider such as VirtualBox or VMware. The
development team only uses VirtualBox, therefore that is the only supported
platform.

## Provisioning instructions

At this moment this playbook assumes the following:

* You must already have Python 2.6 / 2.7 installed. Ansible still doesn't
  support Python 3 on the target server, so even if your distribution comes with
  Python 3, you'll still require Python 2.
* You must already have an SSH service running on your server. Bonus points if
  you already have an RSA key installed so that you can connect without
  providing a password, but you'll be able to connect with password login.

Instructions:

1. Install third party roles: `ansible-galaxy install -r requirements.yml`
2. Copy hosts file from template and modify it to the actual server IP or domain to configure:
  * `cp hosts.example hosts`
  * `vim hosts`
3. Copy variables file from template and modify it to the actual values for
   usernames, passwords, databases and directories to install in.
   * `cp variables.yml.example variables.yml`
   * `vim variables.yml`
4. Provision! `ansible-playbook makigas.yml -i hosts --ask-become-pass`
   * If you are not using RSA keys (why wouldn't you), add `--ask-pass` flag.
5. After deployment, you should add the user you connect via SSH from to
   the deployment group so that you can also deploy via Capistrano:
   `sudo adduser [your username] [deployment.group]`.

### Vagrant specific instructions

There is a Vagrant box configured with Debian 8.0. The configuration file
already provisions the machine when `vagrant up` is executed for the first
time.

## Files overview

Ansible:
* `makigas.yml`: the playbook for provisioning servers.
* `requirements.yml`: roles required to install via Ansible Galaxy.
* `variables.yml`: provisioning variables; you'll probably won't find this,
  that's because...
* `variables.yml.example`: you are supposed to copy this file into variables.yml
  and set the values to the actual usernames, passwords and system information
  you want to use with the playbook and which you should totally keep private.
* `hosts`: the server IPs to install. This file doesn't exist either because
  you are supposed to bootstrap the example template.
* `hosts.example`: the template for the hosts file.
* `roles/`: custom roles for provisioning.

Vagrant:
* `Vagrantfile`: vagrant file configuration.
* `data/`: shared folder. It can be accesed through `/vagrant` in the VM.