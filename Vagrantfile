# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos/7"

  config.vm.define "jenkins" do |jenkins|
      config.vm.hostname = "jenkins.local"
      jenkins.vm.network :private_network, ip: "172.16.10.100"
      jenkins.vm.provider :libvirt do |lb|
          lb.memory = 1024
      end
  end

  config.vm.define "sonar" do |sonar|
      config.vm.hostname = "sonar.local"
      sonar.vm.network :private_network, ip: "172.16.10.110"
      sonar.vm.provider :libvirt do |lb|
          lb.memory = 2048
      end
  end

  config.vm.define "nexus", primary: true do |nexus|
      config.vm.hostname = "nexus.local"
      nexus.vm.network :private_network, ip: "172.16.10.120"
      nexus.vm.provider :libvirt do |lb|
        lb.memory = 1024
      end
  end

  config.vm.define "app", primary: true do |app|
    config.vm.hostname = "app.local"
    app.vm.network :private_network, ip: "172.16.10.130"
    app.vm.provider :libvirt do |lb|
        lb.memory = 512
    end
  end

  config.vm.provider "libvirt" do |libvirt|
      libvirt.storage_pool_name = "ext_storage"
  end

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/alm.yml"
      ansible.groups = {
          "jenkins_server" => ["jenkins"],
          "sonar_server" => ["sonar"],
          "nexus_server" => ["nexus"],
          "app_server" => ["app"]
      }
  end

  if Vagrant.has_plugin?("vagrant-hostmanager")
      config.hostmanager.enabled = true
      config.hostmanager.manage_host = true
      config.hostmanager.manage_guest = true
  end
end
