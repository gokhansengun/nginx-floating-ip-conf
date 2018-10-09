# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_NAME = "gsengun/hashi-dev-box"
BOX_VERSION = "18.02.16"

BOX_IP_PREFIX="172.29.44.10"

Vagrant.configure("2") do |config|
  (1..2).each do |i|
    config.vm.define "lb-0#{i}" do |node|

      node.vm.box = BOX_NAME
      node.vm.box_version = BOX_VERSION

      node.vm.hostname = "lb-0#{i}"
      node.vm.network "private_network", ip: "#{BOX_IP_PREFIX}#{i}"

      node.vm.provision "shell", inline: "apt-get update -y", privileged: true
      node.vm.provision "shell", inline: "apt-get install -y nginx keepalived", privileged: true

      # Move the file under the config directory
      if i == 1
        node.vm.provision "shell", inline: "cp /vagrant/master.keepalived.conf /etc/keepalived/keepalived.conf", privileged: true
      else
        node.vm.provision "shell", inline: "cp /vagrant/backup.keepalived.conf /etc/keepalived/keepalived.conf", privileged: true
      end

      if i == 1
        node.vm.provision "shell", inline: "cp /vagrant/server-1.html /var/www/html/index.nginx-debian.html", privileged: true
      else
        node.vm.provision "shell", inline: "cp /vagrant/server-2.html /var/www/html/index.nginx-debian.html", privileged: true
      end

      node.vm.provision "shell", inline: "systemctl start keepalived", privileged: true

      node.vm.provider "virtualbox" do |vb|
        # Customize the amount of memory on the VM:
        vb.memory = "2048"
        vb.cpus = "1"
      end
    end
  end
end