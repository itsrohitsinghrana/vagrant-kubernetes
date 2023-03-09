# -*- mode: ruby -*-
# vi: set ft=ruby :

NUM_WORKER_NODES=2
IP_NW="192.168.56."
IP_START=10
node_name=0

Vagrant.configure("2") do |config|
    config.vm.box = "generic/ubuntu2204"
    config.vm.box_check_update = true

    config.vm.define "kmaster" do |master|
      master.vm.hostname = "kmaster"
      master.vm.network "private_network", ip: IP_NW + "#{IP_START}"
      master.vm.provider "virtualbox" do |vb|
          vb.memory = 3500
          vb.cpus = 2
          vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
          vb.customize ["modifyvm", :id, "--name", "kmaster"]
      end
      master.vm.provision "shell", path: "scripts/common.sh"
      master.vm.provision "shell", path: "scripts/master.sh"
    end

    (1..NUM_WORKER_NODES).each do |i|
      config.vm.define "kworker#{i}" do |node|
        node.vm.hostname = "kworker#{i}"
        node.vm.network "private_network", ip: IP_NW + "#{IP_START + i}"
        node.vm.provider "virtualbox" do |vb|
            vb.memory = 2000
            vb.cpus = 1
            vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
	    vb.customize ["modifyvm", :id, "--name", "kworker-#{node_name + i}"]
        end
	node.vm.provision "shell", path: "scripts/common.sh"
        node.vm.provision "shell", path: "scripts/node.sh"
      end
    end
  end
