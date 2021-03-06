# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
VAGRANTFILE_API_VERSION="2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  ###################################################
  # Section 1 : Common Settings
  ###################################################
  # For Vagrant Cachier
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  ###################################################
  # Section 2 : Individual Server Settings
  ###################################################
  # Development Server
  config.vm.define :develop do |develop|
    develop.vm.box = "bento/centos-7.5"
    develop.vm.hostname = "develop.pythonwebapp"
    develop.vm.network "private_network", ip: "192.168.33.10"
    develop.vm.synced_folder './application',
                             '/var/www/application',
                             id: 'vagrant-root',
                             nfs: false,
                             owner: 'vagrant',
                             group: 'vagrant',
                             mount_options: ['dmode=777', 'fmode=777']


    config.vm.provision "shell", inline: <<-SHELL
      sudo yum -y update
    SHELL
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbooks/develop.yml"
      ansible.inventory_path = "provisioning/hosts"
      ansible.limit = 'develop'
    end
  end

  # CI Server
  config.vm.define :ci do |ci|
    ci.vm.box = "bento/centos-7.5"
    ci.vm.hostname = "ci.pythonwebapp"
    ci.vm.network "private_network", ip: "192.168.33.100"
  end

  # Deploy Target Server
  config.vm.define :deploy do |deploy|
    deploy.vm.box = "bento/centos-7.5"
    deploy.vm.hostname = "deploy.pythonwebapp"
    deploy.vm.network "private_network", ip: "192.168.33.200"
  end
end
