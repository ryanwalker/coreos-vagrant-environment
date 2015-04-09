# -*- mode: ruby -*-
# # vi: set ft=ruby :

require 'fileutils'

Vagrant.require_version ">= 1.6.0"

CLOUD_CONFIG_PATH = File.join(File.dirname(__FILE__), "user-data")
CONFIG = File.join(File.dirname(__FILE__), "config.rb")

# Defaults for config options defined in CONFIG
$update_channel = "alpha"
$vm_gui = false
$vm_memory = 1024
$vm_cpus = 1

if File.exist?(CONFIG)
  require CONFIG
end

Vagrant.configure("2") do |config|
  # always use Vagrants insecure key
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    # On VirtualBox, we don't have guest additions or a functional vboxsf
    # in CoreOS, so tell Vagrant that so it can be smarter.
    v.check_guest_additions = false
    v.functional_vboxsf     = false
  end

  config.vm.define 'coreos' do |coreos|
    coreos.vm.box = "coreos-%s" % $update_channel
    coreos.vm.box_version = ">= 308.0.1"
    coreos.vm.box_url = "http://%s.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json" % $update_channel

    config.vm.network "forwarded_port", guest: 5000, host: $expose_docker_tcp, auto_correct: true
    config.vm.network "forwarded_port", guest: 4001, host: $expose_etcd_tcp, auto_correct: true
    config.vm.network "forwarded_port", guest: 8091, host: $expose_fleet_tcp, auto_correct: true

    coreos.vm.provider :virtualbox do |vb|
      vb.gui = $vm_gui
      vb.memory = $vm_memory
      vb.cpus = $vm_cpus
    end

    if File.exist?(CLOUD_CONFIG_PATH)
      coreos.vm.provision :file, :source => "#{CLOUD_CONFIG_PATH}", :destination => "/tmp/vagrantfile-user-data"
      coreos.vm.provision :shell, :inline => "mv /tmp/vagrantfile-user-data /var/lib/coreos-vagrant/", :privileged => true
    end
  end
end
