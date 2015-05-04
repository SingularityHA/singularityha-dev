# -*- mode: ruby -*-
# # vi: set ft=ruby :
# SOURCE: http://blog.scottlowe.org/2014/10/22/multi-machine-vagrant-with-yaml/

# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"
 
# Require YAML module
require 'yaml'

# Read YAML file with box details
servers = YAML.load_file('servers.yaml')

# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  # Iterate through entries in YAML file
  servers.each do |servers|
    config.vm.define servers["name"], autostart: false do |srv|
      srv.vm.box = servers["box"]
      srv.vm.box_url = servers["box_url"]
      srv.vm.network "private_network", ip: servers["ip"]
      srv.vm.provider :virtualbox do |vb|
        vb.name = servers["name"]
        vb.memory = servers["ram"]
      end
      config.vm.provision "puppet" do |puppet|
        puppet.module_path = "puppet/modules"
        puppet.manifests_path = "puppet/manifests"
        puppet.manifest_file = "default.pp"
      end
    end
  end
end