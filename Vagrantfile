# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu/trusty64"

  # increase memory of virtualbox
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.define "python2" do |python2|
    python2.vm.network "forwarded_port", guest: 5000, host: 5002
    python2.vm.network "forwarded_port", guest: 15672, host: 15672

    # start Ansible provisioning
    python2.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.sudo = true
      ansible.playbook = "provisioning/playbook.yml"
    end
  end

  config.vm.define "python3", primary: true do |python3|
    python3.vm.network "forwarded_port", guest: 5000, host: 5003
    python3.vm.network "forwarded_port", guest: 15672, host: 15673

    # start Ansible provisioning
    python3.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.sudo = true
      ansible.playbook = "provisioning/playbook.yml"
      ansible.extra_vars = { ansible_python_version: '3' }
    end
  end
end
