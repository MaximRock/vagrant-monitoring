# -*- mode: ruby -*-
# vi: set ft=ruby :

NETWORK_BRIDGE = "enp4s0"
CPUS_PROM = 2
CPUS_CLIENT = 1
MEMORY_PROM = 2048
MEMORY_CLIENT = 1024
IP_PROM = "192.168.1.100"
IP_CLIENT = "192.168.1.101"

Vagrant.configure("2") do |config|

    # config.vm.boot_timeout = 600 
    config.vm.box_check_update=false
    config.vm.box = "ubuntu/jammy64"

    config.vm.define "prom" do |prom|
        prom.vm.network "forwarded_port", guest: 9090, host: 9090,  auto_correct: true
        prom.vm.network "forwarded_port", guest: 9093, host: 9093,  auto_correct: true
        prom.vm.network "forwarded_port", guest: 9094, host: 9094,  auto_correct: true
        prom.vm.network "forwarded_port", guest: 9100, host: 9100,  auto_correct: true
        prom.vm.network "forwarded_port", guest: 3000, host: 3000,  auto_correct: true
        prom.vm.network "public_network", ip: IP_PROM, bridge: NETWORK_BRIDGE
        prom.vm.hostname = "prom"
        prom.vm.synced_folder "src/", "/home/vagrant/src/" 
        prom.vm.provider "virtualbox" do |vb|
            vb.gui = false
            vb.memory = MEMORY_PROM
            vb.cpus = CPUS_PROM
            vb.check_guest_additions = false
        end
    end

    config.vm.define "client" do |client|
        client.vm.network "forwarded_port", guest: 9100, host: 9100, auto_correct: true
        client.vm.network "public_network", ip: IP_CLIENT, bridge: NETWORK_BRIDGE
        client.vm.hostname = "client"
        client.vm.provider "virtualbox" do |vb|
            vb.gui = false
            vb.memory = MEMORY_CLIENT
            vb.cpus = CPUS_CLIENT
            vb.check_guest_additions = false
        end
    end

    # for linux

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "play.yml"
      end  
  
    # for windows
      
    # config.vm.provision "ansible_local" do |ansible|
    #     ansible.playbook = "play.yml"
    #     ansible.install_mode = "pip_args_only"
    #     ansible.pip_args = "-r /vagrant/requirements.txt"
    # end    
end