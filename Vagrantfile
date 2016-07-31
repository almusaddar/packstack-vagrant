# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "geerlingguy/ubuntu1604"
  config.vm.box_check_update = true
  config.vm.hostname = "OpenStack-test"
  config.vm.network "private_network", ip: "192.168.100.100"
  config.vm.post_up_message = "OpenStack-test"
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "public_network"
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "4096"
  end

  config.vm.provision "shell", inline: <<-SHELL
      sudo apt-get -y update
      sudo apt-get install -y git
      sudo useradd stack
      sudo echo stack:123456 | sudo chpasswd
      sudo mkdir /home/stack
      sudo chown stack:stack /home/stack
      sudo sh -c "echo \"stack ALL=(ALL) NOPASSWD:ALL\" >> /etc/sudoers"
      sudo sh -c "echo \"Defaults:stack !requiretty\" >> /etc/sudoers"
      su stack
      cd /home/stack
      git clone https://git.openstack.org/openstack-dev/devstack
      cd devstack
      echo '[[local|localrc]]' > local.conf
      echo ADMIN_PASSWORD=password >> local.conf
      echo DATABASE_PASSWORD=password >> local.conf
      echo RABBIT_PASSWORD=password >> local.conf
      echo SERVICE_PASSWORD=password >> local.conf
      ./stack.sh
  SHELL

end
