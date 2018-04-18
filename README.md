# ansible.ovpnproxy
Ansible role to install OpenVPN on your VPS for certain list of prohibited IPs.

## Features
* Install and configure OpenVPN
* Generate client configuration
* Use Google DNS
* Proxy traffic from defined IP list

## Requirements
* VPS in the appropriate location (country)
* CentOS 7 distribution
* TUN module enabled (in OpenVZ case)

## Usage
Create ansible playbook directory:

    mkdir -p ansible/roles
    touch ansible/main.yml

Clone role into your ansible playbook directory:

    cd ansible/roles
    git clone https://github.com/sukhoykin/ansible.ovpnproxy.git

Add role to `main.yml` playbook file:

```yaml

- hosts: all
  roles:
  - ansible.ovpnproxy
```

Run playbook:

    ansible-playbook -i <vps-ip>, main.yml

Use client configuration file `<vps-ip>.ovpn` for access to VPN server.

## Variables
Optionally you can set vars in your playbook file. Example with default vars:

```yaml

- hosts: all
  roles:
  - ansible.ovpnproxy

  vars:
    ovpn_port: 1194
    ovpn_proto: udp
    ovpn_subnet: 10.8.0.0 255.255.255.0
    ovpn_req_cn:  "example.com"
    ovpn_req_country:  "US"
    ovpn_req_province: "California"
    ovpn_req_city: "Beverly Hills"
    ovpn_req_org: "ACME CORPORATION"
    ovpn_req_email: "user@example.com"
    ovpn_req_ou: "Anvil Department"
    ovpn_proxy_pool:
      # rutracker
      - 195.82.146.214 255.255.255.255
      - 195.82.146.114 255.255.255.255
      # telegram
      - 149.154.167.99 255.255.255.255
      - 149.154.167.118 255.255.255.255
      - 149.154.160.0 255.255.240.0
      - 149.154.164.0 255.255.252.0
      - 91.108.4.0 255.255.252.0
      - 91.108.56.0 255.255.252.0
      - 91.108.8.0 255.255.252.0
      - 149.154.168.0 255.255.252.0
      - 91.108.16.0 255.255.252.0
      - 91.108.56.0 255.255.254.0
```
