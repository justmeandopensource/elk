# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.provision "shell", path: "bootstrap.sh"

  config.vm.define "server" do |server|
    server.vm.box = "centos/7"
    server.vm.hostname = "server.example.com"
    server.vm.network "private_network", ip: "172.42.42.10"
    server.vm.provider "virtualbox" do |vb|
      vb.name = "server"
      vb.memory = 1024
      vb.cpus = 1
    end
  end

  config.vm.define "client" do |client|
    client.vm.box = "centos/7"
    client.vm.hostname = "client.example.com"
    client.vm.network "private_network", ip: "172.42.42.20"
    client.vm.provider "virtualbox" do |vb|
      vb.name = "client"
      vb.memory = 512
      vb.cpus = 1
    end
  end

end
