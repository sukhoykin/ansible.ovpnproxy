# ansible.ovpnproxy
Ansible role to install OpenVPN on your VPS for certain list of prohibited IPs.

## Features
* Install and configure OpenVPN
* Generate client configuration
* Use Google DNS
* Selective service proxy from defined IP list
* Support Windows, Linux and MacOS

## Requirements
* VPS in the appropriate location (country)
* CentOS 7 distribution
* Public `IP-address`
* `Root` SSH-access to VPS
* Ensure TUN module enabled (in case of OpenVZ)

## Quick start
Assume you have proper VPS with public `IP-address` and `root` SSH-access (by password).

### Windows
1. Install [WinSCP](https://winscp.net/eng/download.php) SCP-client.
2. Run WinSCP as Administrator (Right click -> Run as Administrator).
3. Login to VPS:
* Host Name: VPS `IP-address`
* Login: `root`
* Password: `root` password

#### Setup server
1. Open WinSCP terminal (Ctrl+T). Also you can use [PuTTY](https://www.putty.org/) for execute commands instead WinSCP terminal.
2. Install Ansible and Git on VPS (Enter command):
```
yum -y install epel-release
yum -y update
yum -y install ansible git
```
3. Configure server (Enter command and wait few minutes):
```
ansible-pull -U https://github.com/sukhoykin/ansible.ovpnproxy.git
```
4. Close terminal and refresh directory (F5), you will see client config `ovpnproxy.ovpn`.

#### Setup client
1. Install [OpenVPN-client](https://openvpn.net/index.php/open-source/downloads.html).
2. Copy client config `ovpnproxy.ovpn` (select and press Ctrl+C) to `C:\Program Files (x86)\OpenVPN\config`.
3. Run OpenVPN GUI.
4. Connect to VPN (Right click on tray icon -> `ovpnproxy` -> Connect).

### Linux
This guide cover CentOS 7 distribution. Other distributions have similar steps.
#### Setup server
1. Login to VPS.
2. Install Ansible and Git on VPS:
```
# yum -y install epel-release
# yum -y update
# yum -y install ansible git
```
3. Configure server:
```
# ansible-pull -U https://github.com/sukhoykin/ansible.ovpnproxy.git
```

#### Setup client
1. Install OpenVPN:
```
$ sudo yum -y install openvpn
```
2. Copy client config `ovpnproxy.ovpn` to `/etc/openvpn/` with rename:
```
$ sudo scp {vps-ip}:ovpnproxy.ovpn /etc/openvpn/ovpnproxy.conf
```
3. Start OpenVPN:
```
$ sudo systemctl enable openvpn@ovpnproxy
$ sudo systemctl start openvpn@ovpnproxy
```

## Settings
1. Install Ansible and Git on VPS or control machine:
```
# yum -y install ansible git
```
2. Clone repository:
```
$ git clone https://github.com/sukhoykin/ansible.ovpnproxy.git
$ cd ansible.ovpnproxy
```
3. Edit `group_vars/all.yaml` for change settings (see options below) or services `vars/*.yml` files for tune selective mode.
4. Apply changes to OpenVPN server, either:
* run playbook local on VPS:
```
$ ansible-playbook local.yml
```
* run playbook remote from control machine:
```
$ ansible-playbook -i {vps-ip}, site.yml
```

### Selective mode
In this case you define a list of services (IP addresses) that will be accessed through the VPN.
1. Create file for list of service IPs `vars/{service}.yml`.
2. Define list variable `ovpn_service_{service}` and fill it with IPs:
```
ovpn_service_{service}:
  - 108.174.10.10 255.255.255.255
  - 108.174.150.0 255.255.255.0
```
3. Every list item is an IP subnet in format `IP/NET MASK`.
4. Register service name `{service}` in `ovpn_services` variable of `group_vars/all.yaml` file:
```
ovpn_services: [linkedin, {service}]
```
5. Apply changes.

### Gateway mode
In this mode all IP traffic (DNS, HTTP, any service traffic) will go through the VPN.
1. Set `ovpn_services` list in `group_vars/all.yaml` to empty array:
```
ovpn_services: []
````
2. Apply changes.

### OpenVPN settings
You can override default OpenVPN settings in `group_vars/all.yaml` file. `ovpn_req_*` variables affect only in first playbook run. Default settings are:
```
ovpn_port: 1194
ovpn_proto: udp
ovpn_subnet: 10.42.0.0 255.255.255.0

ovpn_req_cn: "example.com"
ovpn_req_country: "US"
ovpn_req_province: "California"
ovpn_req_city: "Beverly Hills"
ovpn_req_org: "ACME CORPORATION"
ovpn_req_email: "user@example.com"
ovpn_req_ou: "Anvil Department"
```
