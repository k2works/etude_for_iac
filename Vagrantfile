# -*- mode: ruby -*-
# vi: set ft=ruby :

$server_script = <<EOF
    sudo apt-get update -y
    sudo apt-get install -y ntpdate
    sudo ntpdate ntp.nict.jp    
    sudo bash -c "echo '192.168.33.102 chef-client' >> /etc/hosts"
    sudo bash -c "echo '192.168.33.10 vagrant.vm' >> /etc/hosts"
EOF

$workstation_script = <<EOF
    sudo apt-get update -y
    sudo apt-get install -y ntpdate
    sudo ntpdate ntp.nict.jp    
    sudo bash -c "echo '192.168.33.102 chef-client' >> /etc/hosts"
    sudo bash -c "echo '192.168.33.10 vagrant.vm' >> /etc/hosts"
EOF

$client_script = <<EOF
    sudo apt-get update -y
    sudo apt-get install -y ntpdate
    sudo ntpdate ntp.nict.jp
    sudo bash -c "echo '192.168.33.10 chef-server' >> /etc/hosts"
    sudo bash -c "echo '192.168.33.10 vagrant.vm' >> /etc/hosts"
EOF

Vagrant.configure("2") do |config|
  config.vm.box = "hiroshima-arc/gin_and_bitters"

  config.vm.define "chef_client" do |host|
    host.vm.box = 'bento/ubuntu-16.04'
    host.vm.hostname = "chef-client"
    host.vm.synced_folder ".", "/vagrant", disabled: true
    host.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2224
    host.vm.network "private_network", ip: "192.168.33.102"
    host.vm.provision :shell, :inline => $client_script
  end

  config.vm.define "chef_server" do |host|
    host.vm.hostname = "chef-server"
    host.vm.synced_folder ".", "/vagrant", disabled: false
    host.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2222
    host.vm.network "private_network", ip: "192.168.33.10"
    host.vm.provision :shell, :inline => $server_script
  end

  config.vm.define "workstation" do |host|
    host.vm.hostname = "chef-workstation"
    host.vm.synced_folder ".", "/vagrant", disabled: false
    host.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2223
    host.vm.network "private_network", ip: "192.168.33.101"
    host.vm.provision :shell, :inline => $workstation_script
  end
end
