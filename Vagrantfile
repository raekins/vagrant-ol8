# -*- mode: ruby -*-
# vi: set ft=ruby :

######################################################################
# Customisation. Change variables here!
#
# virtual machine number of cpus.
#
vm_cpus = "2"
#
# virtual machine memory.
#
vm_memory = "4096"
#
# hostname (without .local)
#
vm_hostname = "ol8-vagrant"
#
# Box metadata location and box name
BOX_URL = "https://oracle.github.io/vagrant-projects/boxes"
BOX_NAME = "oraclelinux/8"
#
######################################################################
#
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

unless Vagrant.has_plugin?("vagrant-vboxmanage")
  puts 'Installing vagrant-vboxmange Plugin...'
  system('vagrant plugin install vagrant-vboxmange')
end

unless Vagrant.has_plugin?("vagrant-vbguest")
  puts 'Installing vagrant-vbguest Plugin...'
  system('vagrant plugin install vagrant-vbguest')
end

unless Vagrant.has_plugin?("vagrant-reload")
  puts 'Installing vagrant-reload Plugin...'
  system('vagrant plugin install vagrant-reload')
end

unless Vagrant.has_plugin?("vagrant-proxyconf")
  puts 'Installing vagrant-proxyconf Plugin...'
  system('vagrant plugin install vagrant-proxyconf')
end

Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = BOX_NAME
  config.vm.box_url = "#{BOX_URL}/#{BOX_NAME}.json"
  config.vm.hostname = "#{vm_hostname}.localdomain"  

  # synced_folder is disabled. it requires a kernel module that is specific to a kernel.
  # this module will not be there once the kernel is upgraded.
  config.vm.synced_folder "vagrant", "/vagrant", disabled: false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest:  80, host: 8080
  config.vm.network "forwarded_port", guest: 443, host: 8443


  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "#{vm_hostname}" 
    vb.memory = "#{vm_memory}"
    vb.cpus = "#{vm_cpus}"
  end

  # Run Ansible from the Vagrant VM
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook       = "playbook.yaml"
    #ansible.verbose        = "-vvv"
    ansible.version        = "latest"
    ansible.inventory_path = "inventory"
    ansible.install        = true
    ansible.limit          = "all" # or only "nodes" group, etc.
    ansible.extra_vars = {
    }
  end
end
