# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.hostmanager.enabled = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "puppet" do |puppet|
    # Puppetmaster on CentOS 7
    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
    puppet.vm.synced_folder ".", "/vagrant"
    #puppet.vm.synced_folder "../code", "/puppet_code"
    #puppet.vm.synced_folder "../puppetserver", "/puppet_puppetserver"
    puppet.vm.box = "centos/7"
    puppet.vm.hostname = "puppet.mylab.local"
    puppet.vm.network :private_network, ip: "172.28.128.100"
    puppet.hostmanager.aliases = %w(puppet)
    puppet.vm.provision "shell", inline: <<-SHELL
      sudo yum update -y
      sudo rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm
      sudo yum install puppetserver -y
      #sudo rm -rf /etc/puppetlabs/code
      #sudo ln -s /puppet_code /etc/puppetlabs/code
      #sudo rm -rf /etc/puppetlabs/puppetserver
      #sudo ln -s /puppet_puppetserver /etc/puppetlabs/puppetserver
      sudo sed -i 's/2g/512m/g' /etc/sysconfig/puppetserver
      #echo "*.mylab.com" | sudo tee /etc/puppetlabs/puppet/autosign.conf
      sudo service puppetserver start
    SHELL
  end

  config.vm.define "agent1" do |agent1|
    # Puppet agent on CentOS 7
    agent1.vm.box = "mpathira/centos72"
    agent1.vm.hostname = "agent1.mylab.local"
    agent1.vm.network :private_network, ip: "172.28.128.3"
    agent1.hostmanager.aliases = %w(agent1)
    agent1.vm.provision "shell", inline: <<-SHELL
      sudo yum update -y
      sudo rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm
      sudo yum install puppet-agent -y
      sudo service puppet start
    SHELL
  end

  config.vm.define "agent2" do |agent2|
    # Puppet agent on Debian
    agent2.vm.box = "ARTACK/debian-jessie"
    agent2.vm.hostname = "agent2.mylab.local"
    agent2.vm.network :private_network, ip: "172.28.128.4"
    agent2.hostmanager.aliases = %w(agent2)
    agent2.vm.provision "shell", inline: <<-SHELL
      wget https://apt.puppetlabs.com/puppetlabs-release-pc1-jessie.deb
      sudo dpkg -i puppetlabs-release-pc1-jessie.deb
      sudo apt-get update
      sudo apt-get install puppet-agent -y
      sudo /opt/puppetlabs/bin/puppet agent --enable
      sudo service puppet start
    SHELL
  end

  config.vm.define "agent3" do |agent3|
    # Puppet agent on Windows 2012
    agent3.vm.box = "devopsguys/Windows2012R2Eval"
    agent3.vm.hostname = "agent3"
    agent3.vm.network :private_network, ip: "172.28.128.5"
    agent3.hostmanager.aliases = %w(agent3)
    agent3.vm.provision "shell", :path => "windows.ps1"
  end
end