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

  # Without this, vagrant ssh logs you in as "vagrant".
  # This is the user that runs jupyter, so that seems good.
  #config.ssh.username='root'
  #config.ssh.password='vagrant'
  #config.ssh.insert_key='true'

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "bento/centos-7.4"
  config.vm.box_download_insecure=true
  # often use  config.vm.box = "centos/7"

  # Disable automatic box update checking. 
  # These configurations are fragile, 
  # Let's not invite the framework to break it.
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8888" will access port 8888 on the guest machine.
  # This is the port for jupyter
  config.vm.network "forwarded_port", guest: 8888, host: 8888
  
  # or create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "10.0.0.2"
  
  # Do not do this, since the jupyter installation is unsecure:
  #config.vm.network "public_network"

  if Vagrant.has_plugin?("vagrant-vbguest")
      puts "Use of vbguest with this VM is not recommended."
      # vbguest succeeds only after kernel is updated
      config.vm.provision "shell", inline: "yum -y update kernel"
      config.vm.provision :reload
  end

  # pass any proxy details to guest
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = ENV['http_proxy'] || ""
    config.proxy.https    = ENV['https_proxy'] || ""
    config.proxy.no_proxy = "localhost,127.0.0.1"
  end


  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/home/vagrant", type: "virtualbox"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  # Customize the amount of memory on the VM:
     vb.memory = "8192" #"4096"
     vb.linked_clone = true
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end
  

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
  config.vm.provision :shell, path: "bootstrap.sh"
  
  # start jupyter
  config.trigger.after :up, :stderr => true do
    #if Vagrant.has_plugin?("vagrant-vbguest")
      #run "vagrant vbguest --auto-reboot --no-provision"
      #run "mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant"
    #end
    run_remote "systemctl start jupyter"
  end


end
