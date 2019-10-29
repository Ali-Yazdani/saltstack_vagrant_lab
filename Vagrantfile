# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'vagrant-host-shell'

Vagrant.configure("2") do |config|

  # Configure salt master
  config.vm.define "master", autostart: true do |master|
    # Set timeout to a higher value
    master.vm.boot_timeout = 120

    # Seed all files locally before bringing master and minions up
    master.vm.provision :host_shell do |host_shell|
      host_shell.inline = 'salt-key -u $USER --gen-keys-dir=files/keys/ --gen-keys=master &&
                        salt-key -u $USER --gen-keys-dir=files/keys/ --gen-keys=minion1 &&
                        salt-key -u $USER --gen-keys-dir=files/keys/ --gen-keys=minion2'
    end

    master.vm.box = "ubuntu/xenial64"
    master.vm.host_name = "master"
    master.vbguest.auto_update = false

    # Link local salt files into master
    master.vm.synced_folder "files/salt/", "/srv/salt"
    master.vm.synced_folder "files/pillar/", "/srv/pillar"
    master.vm.synced_folder "files/formulas/", "/srv/formulas"
    master.vm.network "private_network", ip: "192.168.110.10"
    master.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 1
        vb.name = "master"
    end

    master.vm.provision :salt do |salt|
      salt.master_config = "files/etc/master"
      salt.minion_config = "files/etc/minion"
      salt.master_key = "files/keys/master.pem"
      salt.master_pub = "files/keys/master.pub"
      salt.minion_key = "files/keys/master.pem"
      salt.minion_pub = "files/keys/master.pub"
      salt.seed_master = {
        "master" => "files/keys/master.pub",
        "minion1" => "files/keys/minion1.pub",
        "minion2" => "files/keys/minion2.pub",
      }

      salt.install_master = true
      salt.install_type = "stable"
      salt.no_minion = false
      salt.verbose = true
      salt.colorize = true
      salt.run_highstate = true
      salt.install_args = "2017.7"
    end
  end

  # Configure minion1
  config.vm.define "minion1", autostart: true do |minion1|
    minion1.vm.boot_timeout = 120
    minion1.vm.box = "ubuntu/xenial64"
    minion1.vm.host_name = "minion1"
    minion1.vm.network "private_network", ip: "192.168.110.11"
    minion1.vbguest.auto_update = false
    minion1.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
        vb.name = "minion1"
    end

    minion1.vm.provision :salt do |salt|
      salt.minion_config = "files/etc/minion"
      salt.minion_key = "files/keys/minion1.pem"
      salt.minion_pub = "files/keys/minion1.pub"

      salt.install_type = "stable"
      salt.verbose = true
      salt.colorize = true

      salt.run_highstate = true
      salt.install_args = "2017.7"
    end
  end

  # Configure minion2
  config.vm.define "minion2", autostart: true do |minion2|
    minion2.vm.boot_timeout = 120
    minion2.vm.box = "geerlingguy/centos7"
    minion2.vm.box_version = "1.2.18"
    minion2.vm.host_name = "minion2"
    minion2.vm.network "private_network", ip: "192.168.110.12"
    minion2.vbguest.auto_update = false
    minion2.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
        vb.name = "minion2"
    end

    minion2.vm.provision :salt do |salt|
      salt.minion_config = "files/etc/minion"
      salt.minion_key = "files/keys/minion2.pem"
      salt.minion_pub = "files/keys/minion2.pub"

      salt.install_type = "stable"
      salt.verbose = true
      salt.colorize = true

      salt.run_highstate = true
      salt.install_args = "2017.7"
    end
  end
end
