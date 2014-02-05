Vagrant machine for all projects related to VagrantControl. Mostly all uwsgi-python applications loaded via sockets throught nginx.

To run the project you will need :
    - vagrant
    - Ansible (pip install ansible)

# Note 

To use ansible directly on the machine without talking to vagrant :

        ansible default -i vagrant_ansible_inventory_default --private-key=~/.vagrant.d/insecure_private_key -u vagrant -m setup
