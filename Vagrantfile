# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos/7"
  config.vm.box_url = "https://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-1803_01.VirtualBox.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 443, host: 1210

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
  end

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # if Vagrant.has_plugin?("vagrant-proxyconf")
  #   config.proxy.http = "http://username:password@host:port/"
  #   config.proxy.https = "http://username:password@host:port/"
  #   config.proxy.no_proxy = "localhost,127.0.0.1"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
  #
  config.vm.provision "shell", inline: <<-SHELL
    # package install
    yum -y install epel-release
    yum -y install python-devel unzip wget ansible
    ssh-keygen -f /root/.ssh/id_rsa -t rsa -N ""
    cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys

    # provision with ansible
    cp -r /vagrant/ansible /root
    cd /root/ansible

    # Making of self-signing certificate
    openssl genrsa -out /root/ansible/resource/web/opt/nginx/conf/server.key 2048
    openssl req -new -key /root/ansible/resource/web/opt/nginx/conf/server.key -out /root/ansible/resource/web/opt/nginx/conf/server.csr -subj "/CN=localhost"
    openssl x509 -req -days 365 -in /root/ansible/resource/web/opt/nginx/conf/server.csr -signkey /root/ansible/resource/web/opt/nginx/conf/server.key -out /root/ansible/resource/web/opt/nginx/conf/server.crt

    # Making of unit-self-sign certificate
    openssl genrsa -out /root/ansible/resource/ap/opt/x509/unit.key 2048 -outform DER
    openssl req -new -key /root/ansible/resource/ap/opt/x509/unit.key -out /root/ansible/resource/ap/opt/x509/unit.csr -subj "/CN=localhost"
    openssl x509 -req -days 3650 -in /root/ansible/resource/ap/opt/x509/unit.csr -signkey /root/ansible/resource/ap/opt/x509/unit.key -out /root/ansible/resource/ap/opt/x509/unit-self-sign.crt

    date; ansible-playbook /root/ansible/init_personium.yml ; date
  SHELL



end
