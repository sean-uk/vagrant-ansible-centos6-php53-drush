A work-in-progress vanilla LAMP vagrant setup provisioned with ansible

# Getting Started...

Install virtuabox and vagrant on your host machine. Then go your working dir and run `vagrant init ubuntu/xenial64`

You can learn more about provisionint vagrant boxes using ansible [here](https://www.vagrantup.com/docs/provisioning/ansible_local.html).
I'm opting to use the option of installing ansible on the box itself rather then the host, for wider host compatibility.

When you've added the ansible provisioner config to your Vagrantfile you'l need to set

* `"ansible.playbook"` to be `"ansible/playbook.yml"`, and
* `"ansible.galaxy_role_file"` to `"ansible/requirements.yml"`

(more [here](https://www.vagrantup.com/docs/provisioning/ansible_common.html))

## Network

The simplest thing to do is to map a port on the host machine to one on the VM
by putting a line like this in your Vagrantfile: `config.vm.network "forwarded_port", guest: 80, host: 8080`.  
In that case you'd be able to reach your VM on localhost: 8080

[This fedora magazine article](https://fedoramagazine.org/using-ansible-provision-vagrant-boxes/) will provide some context.
