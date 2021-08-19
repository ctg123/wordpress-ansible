# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "ansible-master" do |vm1|
    vm1.vm.box = "bento/ubuntu-18.04"
    vm1.vm.hostname = "ansible-master"
    vm1.vm.network "private_network", ip: "10.23.45.10"

    config.vm.provider "virtualbox" do |vb|
	  vb.gui = false
	  vb.memory = "4096"
      vb.cpus = "2"
	  vb.customize ['modifyvm', :id, '--cableconnected1', 'on']
    end
	config.vm.provision "shell", run: "always", inline: <<-SHELL
		echo "Welcome to the Ubuntu Ansible network."
	SHELL
  end

  config.vm.define "ansible-node1" do |vm2|
    vm2.vm.box = "bento/ubuntu-18.04"
    vm2.vm.hostname = "ansible-node1"
    vm2.vm.network "private_network", ip: "10.23.45.20"

    config.vm.provider "virtualbox" do |vb|
	  vb.gui = false
	  vb.memory = "2048"
      vb.cpus = "2"
	  vb.customize ['modifyvm', :id, '--cableconnected1', 'on']
    end
	config.vm.provision "shell", run: "always", inline: <<-SHELL
		echo "Welcome to the Ubuntu Ansible network."
	SHELL
  end
  
  config.vm.define "ansible-node2" do |vm3|
    vm3.vm.box = "bento/ubuntu-18.04"
    vm3.vm.hostname = "ansible-node2"
    vm3.vm.network "private_network", ip: "10.23.45.30"

    config.vm.provider "virtualbox" do |vb|
	  vb.gui = false
	  vb.memory = "2048"
      vb.cpus = "2"
	  vb.customize ['modifyvm', :id, '--cableconnected1', 'on']
    end
	config.vm.provision "shell", run: "always", inline: <<-SHELL
		echo "Welcome to the Ubuntu Ansible network."
	SHELL
  end
end
