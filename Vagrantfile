# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/jessie64"

  # This will give the machine its own IP with a full range of accessible ports.
  config.vm.network "private_network", ip: "192.168.40.10"

  # Offer a passthrough folder to the machine.
  config.vm.synced_folder "data", "/vagrant"

  # Use insecure_private_key for making SSH access faster.
  config.ssh.insert_key = false

  # Let's imagine we are using a CPU low in resources.
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  # TODO: Don't start working on Ansible until the VM works.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "makigas.yml"
    ansible.host_key_checking = false
    ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
  end
end
