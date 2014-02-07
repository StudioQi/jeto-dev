# -*- mode: ruby -*-
# vi: set ft=ruby :

$user = "vagrant"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://release.crevette.pheromone.ca/ubuntu-12.04.box"
  config.vm.network :private_network, ip: "172.16.3.2"
  config.vm.hostname = "vagrantcontrol.sandbox.pheromone.ca"

  config.vm.synced_folder "projects/vagrant-control", "/home/" + $user + "/vagrant-control", owner: $user, group: $user
  config.vm.synced_folder "projects/vagrant-worker", "/home/" + $user + "/vagrant-worker", owner: $user, group: $user
  config.vm.synced_folder "projects/htpasswd-api", "/home/" + $user + "/htpasswd-api", owner: $user, group: $user 
  config.vm.synced_folder "projects/nginx-api", "/root/nginx-api"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/bootstrap.yml"
    ansible.inventory_path = "inventory"
  end
end
