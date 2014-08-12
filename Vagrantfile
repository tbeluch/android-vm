# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
PROJECT_NAME = "android.dev"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.ssh.forward_x11 = true
  config.vm.host_name = PROJECT_NAME

	# Boot with a GUI so you can see the screen. (Default is headless)
	#config.vm.boot_mode = :gui
  config.vm.provider "virtualbox" do |v|
    config.vm.box_url = "http://files.vagrantup.com/precise64.box"
    v.name = PROJECT_NAME
    v.gui = true
  end


  config.vm.provider "vmware_desktop" do |v, override|
    config.vm.box = "hashicorp/precise64"
    v.name = PROJECT_NAME
    v.gui = true
    v.vmx["memsize"] = "2048"
    v.vmx["numvcpus"] = "2"
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network :forwarded_port, guest: 5037, host: 5037, auto_correct: true # Android ADB port


  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  config.vm.provision :chef_solo do |chef|

    chef.cookbooks_path = "cookbooks"

    chef.add_recipe "apt"
    chef.add_recipe "git"
    chef.add_recipe "build-essential"
    chef.add_recipe "java"
    chef.add_recipe "sqlite-dev"
    chef.add_recipe "vim"

  end

  config.vm.provision "shell", inline: "echo Installing Android ADT bundle and NDK..."
  config.vm.provision "shell", path: "provision.sh"
  config.vm.provision "shell", inline: "echo Installation complete"

  config.vm.synced_folder "./", "/vagrant", owner: 'vagrant', group: 'vagrant'
end
