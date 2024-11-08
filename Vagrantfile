# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"

  config.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y bind9 bind9utils bind9-doc
    SHELL

  config.vm.define "DNSA" do |dnsa|
    dnsa.vm.hostname = "dnsa.ies.test"
    dnsa.vm.network "private_network", ip: "192.168.57.10"
    
    dnsa.vm.provision "shell", inline: <<-SHELL

      sudo cp /vagrant/files/DNSA/named.conf.local /etc/bind/named.conf.local
      sudo mkdir /etc/bind/zones
      sudo cp /vagrant/files/DNSA/zones/* /etc/bind/zones/
      sudo systemctl restart bind9

    SHELL
  end # DNSA

  config.vm.define "DNSB" do |dnsb|
    dnsb.vm.hostname = "dnsb.ies.test"
    dnsb.vm.network "private_network", ip: "192.168.57.11"

    dnsb.vm.provision "shell", inline: <<-SHELL

      sudo cp /vagrant/files/DNSB/named.conf.local /etc/bind/named.conf.local
      sudo systemctl restart bind9

    SHELL
  end # DNSB

end
