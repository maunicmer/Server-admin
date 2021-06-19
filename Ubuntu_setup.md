## Fixed IP

Ubuntu 17.10 and later uses Netplan as the default network management tool. The previous Ubuntu versions were using ifconfig and its configuration file /etc/network/interfaces to configure the network.

Netplan configuration files are written in YAML syntax with a .yaml file extension. To configure a network interface with Netplan, you need to create a YAML description for the interface, and Netplan will generate the required configuration files for the chosen renderer tool.

Netplan supports two renderers, NetworkManager and Systemd-networkd. NetworkManager is mostly used on Desktop machines, while the Systemd-networkd is used on servers without a GUI.

/etc/netplan/01-netcfg.yaml

/**
network:
  version: 2
  renderer: networkd
  ethernets:
    ens3:
      dhcp4: no
      addresses:
        - 192.168.121.221/24
      gateway4: 192.168.121.1
      nameservers:
          addresses: [8.8.8.8, 1.1.1.1]
**/
