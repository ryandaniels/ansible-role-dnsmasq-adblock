# ansible-role-dnsmasq-adblock

Use dnsmasq for adblocking with OpenVPN. Use this Ansible role after installing OpenVPN ([PiVPN](https://github.com/pivpn/pivpn) or [Streisand](https://github.com/StreisandEffect/streisand)) on a RaspberryPi or a VPS for example.  


Distros tested
------------

* Ubuntu 16.04
* Raspbian Jessie
* Raspbian Stretch


Prerequisites
------------

You need a Ubuntu/Debian server to run OpenVPN. You can use a RaspberryPi or your own VPS. For example you can use Linode or Vultr.


Default Settings
------------

- **adblock_file_remote_hosts_blocked**: URL of hosts file to use for adblocking
- **adblock_file_local_hosts_blocked**: local file for adblock hosts
#DNS servers (OpenDNS used below)
- **adblock_DNS_server1**: 208.67.222.222
- **adblock_DNS_server2**: 208.67.220.220
- **adblock_file_local_adblock_script**: crontab script to update adblock hosts file
#If adblock_manage_openvpn: true, copy openvpn ip to dnsconf conf file and modify openvpn conf file.
- **adblock_manage_openvpn**: true|false (default true)
#OpenVPN server IP
- **adblock_openvpn_server_ip**: OpenVPN server's OpenVPN IP
- **adblock_openvpn_server_config_file**: OpenVPN config file


Example Playbook install-dnsmasq-adblock.yml
------------

```
---
- hosts: '{{inventory}}'
  become: yes
  roles:
  - server-update-reboot
  - dnsmasq-adblock
```


Prep
------------

- Install OpenVPN (manually, pivpn, or Streisand)

To install OpenVPN via PiVPN, follow the [steps](https://github.com/pivpn/pivpn#installation).

To install (only) OpenVPN via Streisand, follow [prerequisites](https://github.com/StreisandEffect/streisand#prerequisites) steps, and then:
```
remote_server_IP=1.2.3.4  # Change IP to your VPS/remote server IP
git clone https://github.com/ryandaniels/ansible-role-dnsmasq-adblock.git ~/ansible/roles/dnsmasq-adblock
git clone https://github.com/StreisandEffect/streisand.git && cd streisand

~/streisand/deploy/streisand-existing-cloud-server.sh --ip-address $remote_server_IP --ssh-user root --site-config ~/ansible/roles/dnsmasq-adblock/files/streisand-local-site.yml
```

- run ansible commands to enable adblocking


Usage
------------

- Install if not using Streisand.  
Also modify OpenVPN config to work with adblocking (copy openvpn IP to dnsmasq config file and modify openvpn config file.)
```
ansible-playbook install-dnsmasq-adblock.yml --extra-vars "inventory=openvpn-server" -i hosts
```

- Install if using Streisand (since Streisand is already using dnsmasq and OpenVPN together):
```
ansible-playbook install-dnsmasq-adblock.yml --extra-vars "inventory=streisand-host adblock_manage_openvpn=false" -i ~/streisand/inventories/inventory-existing
```


Disclaimer
----------------
Wherever you get a server from, be sure you're obeying their TOS. I'm not responsible for anything you do from following this guide.


Acknowledgements
----------------
[Steven Black](https://github.com/StevenBlack/hosts) for his great consolidated hosts file for malware & adblocking.  
[Bob Nisco](https://github.com/BobNisco/adblocking-vpn) for putting together the information and the manual commands of the steps and configuration of dnsmasq for adblocking.  
[Streisand](https://github.com/StreisandEffect/streisand) is an amazing auto-installer using Ansible for many privacy related tools, including OpenVPN.  
[PiVPN](https://github.com/pivpn/pivpn) is a simple installer if you have a RaspberryPi and want to run OpenVPN.  

