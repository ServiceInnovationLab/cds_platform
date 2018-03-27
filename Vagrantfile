# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = # insert box name here
  config.vm.box_url = # insert url to centos box here
  config.vm.box_check_update = false

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.define :delegations do |delegations|
    delegations.vm.hostname = "vagrant.delegations.org.nz.vm"

    delegations.vm.network :private_network, ip: "10.128.100.2", netmask: "255.255.0.0", auto_config: false

    singleHostIps = (2..7).map{|j| "10.128.100.#{j}"}.join(",")
    delegations.vm.provision "shell", name:"Remove connection (might generate error message on stdout)", inline: "sudo nmcli con del enp0s8 | true"
    delegations.vm.provision "shell", name:"Setup connection and bind to interface", inline: "sudo nmcli con add type ethernet ifname enp0s8 con-name enp0s8"
    delegations.vm.provision "shell", name:"Setup connection and bind to interface", inline: "sudo nmcli con mod enp0s8 ipv4.addresses '#{singleHostIps}' ipv4.method manual ipv4.routes 10.128.0.0/16"
    delegations.vm.provision "shell", name:"Restart network", inline: "sudo systemctl restart network"

    delegations.vm.provision "ansible_local" do |ansible|
        ansible.inventory_path = "staging/vagrant/inventory"
        ansible.limit = "all,localhost"
        ansible.playbook = "playbook.yml"

        ansible.galaxy_role_file = "roles/requirements.yml"
        ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file}" # download as local role, only once

        # uncomment these to get a specific version of ansible. Oterwise, you get what yum has
        #ansible.install_mode = "pip"
        #ansible.version = "2.2.1.0"

        # uncomment this to get more verbose logging from ansible plays
        ansible.verbose = "v"
    end

    delegations.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "3072"]
      vb.name = "delegations"
    end
  end

end
