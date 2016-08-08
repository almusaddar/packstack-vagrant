# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.2"
  config.vm.box_check_update = false
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "private_network", ip: "192.168.99.99"
  config.vm.network "public_network"
  config.vm.hostname = "openstack.lab"
  config.vm.post_up_message = "Start OpenStack"
  config.vm.synced_folder "./", "/vagrant"
  config.ssh.insert_key = false
  config.ssh.username = "vagrant"
  config.ssh.password = "vagrant"
  config.vm.provider "virtualbox" do |v|
      v.name = "openstack.lab"
      v.cpus = 4
      v.customize ["modifyvm", :id, "--memory", "8192"]
  end
  config.vm.provision "shell", inline: <<-SHELL
    echo "configure network settings"
    sudo systemctl disable firewalld > /dev/null
    sudo systemctl stop firewalld > /dev/null
    sudo systemctl disable NetworkManager > /dev/null
    sudo systemctl stop NetworkManager > /dev/null
    sudo systemctl enable network > /dev/null
    sudo systemctl start network > /dev/null
    echo "Update System & Install openstack-packstack"
    sudo yum install -y centos-release-openstack-mitaka > /dev/null
    sudo yum update -y > /dev/null
    sudo yum install -y openstack-packstack > /dev/null
  SHELL
end
