# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
CENTOS_IMAGE = "puphpet/centos65-x64"
UBUNTU_IMAGE = "ubuntu/trusty64"
EXAMPLE_DOMAIN = "example.com"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define :master do |master_config|
    master_config.vm.box = CENTOS_IMAGE
    master_config.vm.host_name = "salt.#{EXAMPLE_DOMAIN}"
    master_config.vm.network :private_network, ip: "172.16.42.10"
    master_config.vm.synced_folder "salt-root", "/srv"
    master_config.vm.provision :salt do |salt|
      salt.install_master = true
      salt.install_type = "stable"
      salt.colorize = true
    end
  end

  config.vm.define :minion1 do |minion_config|
    minion_config.vm.box = CENTOS_IMAGE
    minion_config.vm.host_name = "minion1.#{EXAMPLE_DOMAIN}"
    minion_config.vm.network :private_network, ip: "172.16.42.11"
    minion_config.vm.provision :salt do |salt|
      salt.install_type = "stable"
      salt.colorize = true
    end
  end

  config.vm.define :minion2 do |minion_config|
    minion_config.vm.box = UBUNTU_IMAGE
    minion_config.vm.host_name = "minion2.#{EXAMPLE_DOMAIN}"
    minion_config.vm.network :private_network, ip: "172.16.42.12"
    minion_config.vm.provision :salt do |salt|
      salt.install_type = "stable"
      salt.colorize = true
    end
  end
end
