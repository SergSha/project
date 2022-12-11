# -*- mode: ruby -*-
# vi: set ft=ruby :

MACHINES = {
  :fw => {
    :box_name => "centos/7",
#    :vm_name => "fw",
    #:public => {:ip => '10.10.10.1', :adapter => 1},
    :net => [
      {ip: '192.168.1.11', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "privnet"},
      {ip: '192.168.200.11', adapter: 8},
    ]
  },
  :web => {
    :box_name => "centos/7",
#    :vm_name => "web",
    :net => [
      {ip: '192.168.1.12', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "privnet"},
      {ip: '192.168.200.12', adapter: 8},
    ]
  },
  :data => {
    :box_name => "centos/7",
#    :vm_name => "data",
    :net => [
      {ip: '192.168.1.13', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "privnet"},
      {ip: '192.168.200.13', adapter: 8},
    ]
  },
  :replica => {
    :box_name => "centos/7",
#    :vm_name => "replica",
    :net => [
      {ip: '192.168.1.14', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "privnet"},
      {ip: '192.168.200.14', adapter: 8},
    ]
  },
  :backup => {
    :box_name => "centos/7",
#    :vm_name => "backup",
    :net => [
      {ip: '192.168.1.15', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "privnet"},
      {ip: '192.168.200.15', adapter: 8},
    ]
  }#,
#  :logger => {
#    :box_name => "centos/7",
#    :vm_name => "logger",
#    :net => [
#      {ip: '192.168.1.16', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "privnet"},
#      {ip: '192.168.200.16', adapter: 8},
#    ]
#  }
}
Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxname.to_s
      boxconfig[:net].each do |ipconf|
        box.vm.network "private_network", ipconf
      end
      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end
      box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
#        sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
#        systemctl restart sshd
      SHELL
#      if boxconfig[:vm_name] == "backup"
      if boxname.to_s == "backup"
        box.vm.provision "ansible" do |ansible|
          ansible.playbook = "ansible/playbook.yml"
          ansible.inventory_path = "ansible/hosts"
          ansible.host_key_checking = "false"
          ansible.verbose = "v"
          ansible.limit = "all"
        end
      end
    end
  end
end
