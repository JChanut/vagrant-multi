# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'socket'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Get the ip of the host
ip_address = Socket.ip_address_list.find { |ai| ai.ipv4? && !ai.ipv4_loopback? }.ip_address

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  
  config.vm.provision "shell", inline: "echo Hello"
  
  config.vm.define "web" do |web|
    web.vm.hostname = "web.ddsim.local"
    web.vm.provision :shell, path: "bootstrap.sh"
    web.vm.network "private_network", ip: "10.20.1.2"
    web.vm.provision :hosts do |provisioner|
      provisioner.autoconfigure = true
      provisioner.add_host ip_address, ['my.ddsim.local']
    end
    web.vm.network "forwarded_port", guest: 80, host: 8080
  end

  config.vm.define "db" do |db|
    db.vm.hostname = "db.ddsim.local"
    db.vm.network "private_network", ip: "10.20.1.3"
    db.vm.provision :hosts do |provisioner|
      provisioner.autoconfigure = true
      provisioner.add_host ip_address, ['my.ddsim.local']
    end
  end
end
