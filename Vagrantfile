# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.boot_timeout = 600 
    config.vm.box_check_update=false
    config.vm.box = "ubuntu/jammy64"

    config.vm.define "prom" do |prom|
        prom.vm.network "forwarded_port", guest: 9090, host: 9090,  auto_correct: true
        prom.vm.network "forwarded_port", guest: 9093, host: 9093,  auto_correct: true
        prom.vm.network "forwarded_port", guest: 9094, host: 9094,  auto_correct: true
        prom.vm.network "forwarded_port", guest: 9100, host: 9100,  auto_correct: true
        prom.vm.network "public_network", ip: "192.168.1.100", bridge: "Realtek PCIe GbE Family Controller"
        prom.vm.hostname = "prom"
        prom.vm.provider "virtualbox" do |vb|
            vb.gui = false
            vb.memory=1024
            vb.cpus=1
            vb.check_guest_additions=false
        end
    end

    config.vm.define "client" do |client|
        client.vm.network "forwarded_port", guest: 9100, host: 9100, auto_correct: true
        client.vm.network "public_network", ip: "192.168.1.101", bridge: "Realtek PCIe GbE Family Controller"
        client.vm.hostname = "client"
        client.vm.provider "virtualbox" do |vb|
            vb.gui = false
            vb.memory=1024
            vb.cpus=1
            vb.check_guest_additions=false
        end
    end

    config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "play.yml"
        ansible.install_mode = "pip_args_only"
        ansible.pip_args = "-r /vagrant/requirements.txt"
    end    
end