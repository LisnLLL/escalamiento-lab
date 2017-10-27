# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.network "public_network", bridge: "enp9s0", auto_config: false

  config.vm.provision "shell",
    run: "always",
    inline: "ifconfig enp0s8 163.10.33.193 netmask 255.255.255.192 up"

  config.vm.provision "file", source: "./src/dhcpd.conf", destination: "/tmp/dhcpd.conf"
  config.vm.provision "file", source: "./src/undionly.kpxe", destination: "/tmp/undionly.kpxe"
  config.vm.provision "file", source: "./src/ipxe.conf", destination: "/tmp/ipxe.conf"
  config.vm.provision "file", source: "./src/cloud-config.yml", destination: "/tmp/cloud-config.yml"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y isc-dhcp-server tftpd-hpa nginx
    cp /tmp/dhcpd.conf /etc/dhcp/
    cp /tmp/undionly.kpxe /var/lib/tftpboot
    rm -f /var/www/html/*
    cp /tmp/ipxe.conf /var/www/html/
    cp /tmp/cloud-config.yml /var/www/html/
    sed 's/^INTERFACES=.*/INTERFACES="enp0s8"/' -i /etc/default/isc-dhcp-server
    systemctl restart isc-dhcp-server
    echo 1 > /proc/sys/net/ipv4/ip_forward
    iptables -t nat -I POSTROUTING -o enp0s3 -j MASQUERADE
  SHELL
end
