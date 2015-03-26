# -*- mode: ruby -*-
# vi: set ft=ruby :

$user = "vagrant"
VAGRANTFILE_API_VERSION = "2"
$ip       = '172.16.3.2'
$ip_lxc   = '10.0.3.16'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "jeto.local" do |v|
    v.vm.hostname = "jeto.local"

    v.vm.synced_folder "projects/jeto", "/home/" + $user + "/jeto", owner: $user, group: $user
    v.vm.synced_folder "projects/vagrant-worker", "/home/" + $user + "/vagrant-worker", owner: $user, group: $user
    v.vm.synced_folder "projects/htpasswd-api", "/home/" + $user + "/htpasswd-api", owner: $user, group: $user 
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
      lxc.customize 'aa_profile', 'unconfined'
      # Required to boot nested containers
      config.vm.provision :shell, inline: %[
        if ! [ -f /etc/default/lxc ]; then
          cat <<STR > /etc/default/lxc
            LXC_AUTO="true"
            USE_LXC_BRIDGE="true"
            LXC_BRIDGE="lxcbr0"
            LXC_ADDR="10.0.253.1"
            LXC_NETMASK="255.255.255.0"
            LXC_NETWORK="10.0.253.0/24"
            LXC_DHCP_RANGE="10.0.253.2,10.0.253.254"
            LXC_DHCP_MAX="253"
            LXC_SHUTDOWN_TIMEOUT=120
STR
          fi
        ]      
    end

    v.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/bootstrap.yml"
      ansible.host_key_checking = false
      #ansible.verbose = 'vvvv'
      #ansible.tags = 'test'
    end

    v.vm.provision :shell, run: "always" do |s|
      s.inline = <<SCRIPT
      service jeto restart
      service nginx-api restart
      service htpasswd-api restart
      python /home/vagrant/vagrant-worker/worker.py high &
      python /home/vagrant/vagrant-worker/worker.py low &
SCRIPT
    end
  end
end
