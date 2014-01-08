# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://release.crevette.pheromone.ca/ubuntu-12.04.box"
  config.vm.network :private_network, ip: "172.16.3.2"
  config.vm.synced_folder "projects/vagrant-control", "/home/crevette/vagrant-control", owner: "crevette", group: "crevette"
  config.vm.synced_folder "projects/vagrant-worker", "/home/crevette/vagrant-worker", owner: "crevette", group: "crevette"
  config.vm.synced_folder "projects/htpasswd-api", "/home/crevette/htpasswd-api", owner: "crevette", group: "crevette"
  config.vm.synced_folder "projects/nginx-api", "/root/htpasswd-api"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "bootstrap.yml"
    ansible.inventory_path = "inventory"
  end
end
