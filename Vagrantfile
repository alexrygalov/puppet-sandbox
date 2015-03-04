# -*- mode: ruby -*-
# vi: set ft=ruby :

domain = 'example.com'

puppet_nodes = [
  {:hostname => 'puppet',  :ip => '10.37.132.100', :box => 'parallels/ubuntu-14.04', :fwdhost => 8140, :fwdguest => 8140, :ram => 512},
  {:hostname => 'client1', :ip => '10.37.132.101', :box => 'parallels/ubuntu-14.04'},
  {:hostname => 'client2', :ip => '10.37.132.102', :box => 'parallels/ubuntu-14.04'},
]

Vagrant.configure("2") do |config|
  puppet_nodes.each do |node|
    config.vm.define node[:hostname] do |node_config|
      node_config.vm.box = node[:box]
      node_config.vm.hostname = node[:hostname] + '.' + domain
      node_config.vm.network :private_network, ip: node[:ip]
      node_config.puppet_install.puppet_version = :latest
    
      if node[:fwdhost]
        node_config.vm.network :forwarded_port, guest: node[:fwdguest], host: node[:fwdhost]
      end

       config.vm.provider "parallels" do |v|
       v.update_guest_tools = true
       v.memory = 512
       v.cpus = 2
       end

       node_config.vm.provision :puppet do |puppet|
        puppet.manifests_path = 'provision/manifests'
        puppet.module_path = 'provision/modules'
      end
    end
  end
end
