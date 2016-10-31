A work-in-progress vanilla LAMP vagrant setup provisioned with ansible

# Getting Started...

Install virtuabox and vagrant on your host machine. Then go your working dir and run `vagrant init ubuntu/xenial64`

You can learn more about provisionint vagrant boxes using ansible [here](https://www.vagrantup.com/docs/provisioning/ansible_local.html).
I'm opting to use the option of installing ansible on the box itself rather then the host, for wider host compatibility.

When you've added the ansible provisioner config to your Vagrantfile you'l need to set

* `ansible.playbook` to be `"ansible/playbook.yml"`, and
* `ansible.galaxy_role_file` to `"ansible/requirements.yml"`

ie; 

    Vagrant.configure("2") do |config|
      # Run Ansible from the Vagrant VM
      config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "ansible/playbook.yml"
        ansible.galaxy_role_file = "ansible/requirements.yml"
      end
    end

(more [here](https://www.vagrantup.com/docs/provisioning/ansible.html))

## Network

The simplest thing to do is to map a port on the host machine to one on the VM
by putting a line like this in your Vagrantfile: `config.vm.network "forwarded_port", guest: 80, host: 8080`.  
In that case you'd be able to reach your VM on localhost:8080

[This fedora magazine article](https://fedoramagazine.org/using-ansible-provision-vagrant-boxes/) will provide some context.

If you want access via hostname ie; myproject.dev, add that as a serveralias in `ansible\vars\vhosts.yml` and then add
that domain to your hosts file. 

You can give your box a static IP using the ["private_network" setting in the Vagrantfile](https://www.vagrantup.com/docs/networking/private_network.html),
ie; `config.vm.network "private_network", ip: "192.168.33.10"`.
*There is a known issue using this setting in Vagrant 1.8.1 on ubuntu xenial. More [here](https://askubuntu.com/questions/760871/network-settings-fail-for-ubuntu-xenial64-vagrant-box).*
Try ubuntu/trusty64 as your base box instead.

## Sync'ed folder

The idea is to have your project folder, with this project checked out into a subfolder for your dev VM.
Your setup would look something like this:

* MyProject
 * _this folder_
  * README.md
  * ansible
  * ...
 * _data_ (web root)

To then sync the content of _data_ with /var/www, add a line like this to your Vagrantfile:
`config.vm.synced_folder "../data", "/var/www"`
(in the `|config|` section as before)

### Unison

Performance problems with the sync between Windows hosts and Linux guests are well known.   
Consider using the [vagrant unison plugin](https://github.com/mrdavidlaing/vagrant-unison)
