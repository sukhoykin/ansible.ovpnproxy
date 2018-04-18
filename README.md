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
