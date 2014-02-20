# -*- mode: ruby -*-
# vi: set ft=ruby :

$user = "vagrant"
VAGRANTFILE_API_VERSION = "2"
$ip       = '172.16.3.2'
$ip_lxc   = '10.0.3.16'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "vagrant" do |v|
    v.vm.hostname = "vagrantcontrol.sandbox.pheromone.ca"

    v.vm.synced_folder "projects/vagrant-control", "/home/" + $user + "/vagrant-control", owner: $user, group: $user
    v.vm.synced_folder "projects/vagrant-worker", "/home/" + $user + "/vagrant-worker", owner: $user, group: $user
    v.vm.synced_folder "projects/htpasswd-api", "/home/" + $user + "/htpasswd-api", owner: $user, group: $user 
    v.vm.synced_folder "projects/nginx-api", "/root/nginx-api"

    v.vm.provider :virtualbox do |vb, override|
      override.vm.box = "precise64"
      override.vm.box_url = "http://release.crevette.pheromone.ca/ubuntu-12.04.box"
      override.vm.network :private_network, ip: $ip
    end

    v.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/bootstrap.yml"
      ansible.host_key_checking = false
      #ansible.inventory_path = "inventory"
      #ansible.verbose = 'vvvv'
    end
  end
end
