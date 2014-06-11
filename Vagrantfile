# -*- mode: ruby -*-
# vi: set ft=ruby :

$user = "vagrant"
VAGRANTFILE_API_VERSION = "2"
$ip       = '172.16.3.2'
$ip_lxc   = '10.0.3.16'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "vagrantcontrol.sandbox.pheromone.ca" do |v|
    v.vm.hostname = "vagrantcontrol.sandbox.pheromone.ca"

    v.vm.synced_folder "projects/vagrant-control", "/home/" + $user + "/vagrant-control", owner: $user, group: $user
    v.vm.synced_folder "projects/vagrant-worker", "/home/" + $user + "/vagrant-worker", owner: $user, group: $user
    v.vm.synced_folder "projects/htpasswd-api", "/home/" + $user + "/htpasswd-api", owner: $user, group: $user 
    v.vm.synced_folder "projects/ansible", "/home/" + $user + "/ansible", owner: $user, group: $user 
    v.vm.synced_folder "projects/nginx-api", "/root/nginx-api"

    v.vm.provider :virtualbox do |vb, override|
      override.vm.box = "trusty64"
      override.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
      override.vm.network :private_network, ip: $ip
      vb.memory = 1024
      vb.cpus = 2
    end

    v.vm.provider :lxc do |lxc, override|
      override.vm.box = 'fgrehm/trusty64-lxc'
      lxc.customize 'network.ipv4', $ip_lxc
    end

    v.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/bootstrap.yml"
      ansible.host_key_checking = false
      #ansible.verbose = 'vvvv'
    end

    v.vm.provision :shell, run: "always" do |s|
      s.inline = <<SCRIPT
      service vagrant-control restart
      service nginx-api restart
      service htpasswd-api restart
      python /home/vagrant/vagrant-worker/worker.py high &
      python /home/vagrant/vagrant-worker/worker.py low &
SCRIPT
    end
  end
end
