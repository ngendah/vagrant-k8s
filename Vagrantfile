# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https:#docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https:#vagrantcloud.com/search.
  config.vm.box = "ubuntu/jammy64"
  config.vm.box_check_update = false

  machines = [
    {
      name: 'control01', ip: '192.168.56.10',
      ports: [{ guest: 6443, host: 6443, auto_correct: false },],
    },
    {name: 'node01', ip: '192.168.56.11' },
    {name: 'node02', ip: '192.168.56.12' },
  ]
  config.vm.provider :virtualbox do |vb|
    vb.cpus = 2
    vb.memory = '2048'
  end
  etc_hosts = machines.collect{ |vm|
    { domain: vm[:name], ip: vm[:ip] }
  }
  machines.each { |vm|
    config.vm.define(vm[:name], privileged: false) { |cfg|
      cfg.vm.hostname = vm[:name]
      cfg.vm.network :private_network, ip: vm[:ip]
      cfg.vm.synced_folder './cluster', '.', disabled: true
      (vm[:ports] || []).each { |port|
        cfg.vm.network :forwarded_port,
          guest: port[:guest],
          host: port[:host],
          protocol: :tcp,
          auto_correct: port[:auto_correct]
      }
      cfg.vm.provision :ansible do |ansible| 
        ansible.playbook = 'cluster/main.yml'
        ansible.extra_vars = {
          host_ip: vm[:ip],
          etc_hosts: etc_hosts,
          no_falco: false,
          no_gvisor: false,
        }
        ansible.groups = {
          'control': ['control01'],
          'node': [
            'node01',
            'node02',
          ],
        }
      end
    }
  }
end
