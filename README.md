# ansible.ovpnproxy
Ansible role to install OpenVPN on your VPS for certain list of prohibited IPs.

## Features
* Install and configurate OpenVPN
* Generate client configuration
* Proxy traffic from defined IP list

## Requirements
* VPS in the appropriate location (country)
* CentOS 7 distribution
* TUN module enabled

## Usage
Clone role into your ansible playbook directory:

    git clone https://github.com/sukhoykin/ansible.ovpnproxy.git

Add role to your playbook file:

```yaml

- hosts: all
  roles:
  - ansible.ovpnproxy
```

Set vars in your playbook file. Example with default vars:

```yaml

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
```
