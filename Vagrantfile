# -*- mode: ruby -*-
# vi: set ft=ruby :
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure("2") do |config|

  config.vm.define "nodep1" do |nodep1|
    nodep1.vm.box = "./trusty-server-cloudimg-amd64-vagrant-disk1.box"
    nodep1.vm.network "private_network", ip: "192.168.33.51"
	nodep1.vm.hostname = "nodep1"
	nodep1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
	  vb.name = "nodep1"
    end
	nodep1.vm.provision "shell", inline: <<-SHELL
      sudo echo "192.168.33.50 puppet" >> /etc/hosts
	  sudo echo "192.168.33.51 nodep1" >> /etc/hosts
    SHELL
  end

  config.vm.define "nodep2" do |nodep2|
    nodep2.vm.box = "./trusty-server-cloudimg-amd64-vagrant-disk1.box"
    nodep2.vm.network "private_network", ip: "192.168.33.52"
	nodep2.vm.hostname = "nodep2"
	nodep2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
	  vb.name = "nodep2"
    end
	nodep2.vm.provision "shell", inline: <<-SHELL
      sudo echo "192.168.33.50 puppet" >> /etc/hosts
      sudo echo "192.168.33.52 nodep2" >> /etc/hosts
      
    SHELL
  end
#  
#  config.vm.define "nodep3" do |nodep3|
#    nodep3.vm.box = "./trusty-server-cloudimg-amd64-vagrant-disk1.box"
#    nodep3.vm.network "private_network", ip: "192.168.33.53"
#	nodep3.vm.hostname = "nodep3"
#	nodep3.vm.provider "virtualbox" do |vb|
#      vb.memory = "512"
#	  vb.name = "nodep3"
#    end
#	nodep3.vm.provision "shell", inline: <<-SHELL
#      sudo echo "192.168.33.50 puppet" >> /etc/hosts
#      sudo echo "192.168.33.53 nodep3" >> /etc/hosts
#      sudo mkdir -p /etc/facter/facts.d/
#      sudo echo "boxrole=webserver /etc/facter/facts.d/myconf.txt"
#      
#    SHELL
#  end
#  
  config.vm.define "puppet" do |puppet|
    puppet.vm.box = "./trusty-server-cloudimg-amd64-vagrant-disk1.box"
    puppet.vm.network "private_network", ip: "192.168.33.50"
	puppet.vm.hostname ="puppet"
	puppet.vm.provider "virtualbox" do |vb|
     vb.memory = "512"
	  vb.name = "puppet"
    end
	
	puppet.vm.provision "shell", inline: <<-SHELLSUDO
     sudo echo "192.168.33.50 puppet" >> /etc/hosts
	  sudo echo "192.168.33.51 nodep1" >> /etc/hosts
	  sudo apt-get -y install sshpass
   	  sudo apt-get update
    SHELLSUDO
	
	puppet.vm.provision "shell", privileged: false, inline: <<-SHELL
	  ssh-keygen -t rsa -P "" -f ~/.ssh/id_rsa
	  echo "StrictHostKeyChecking no" >> ~/.ssh/config
	  sshpass -p "vagrant" ssh-copy-id -i /home/vagrant/.ssh/id_rsa.pub vagrant@nodep1
   SHELL
		
  end
  
end