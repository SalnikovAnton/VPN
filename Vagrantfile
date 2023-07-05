# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|
 config.vm.box = "centos/stream8"
 config.vm.define "server" do |server|
 server.vm.hostname = "server.loc"
 server.vm.network "private_network", ip: "192.168.56.10"
 config.vm.provision "ansible" do |ansible|
        ansible.playbook = "serverplaybook.yml"
      end
 end
 config.vm.define "client" do |client|
 client.vm.hostname = "client.loc"
 client.vm.network "private_network", ip: "192.168.56.20"
 config.vm.provision "ansible" do |ansible|
        ansible.playbook = "clientplaybook.yml"
      end
 end
end
