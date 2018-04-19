# ansible.ovpnproxy
Ansible role to install OpenVPN on your VPS for certain list of prohibited IPs.

## Features
* Install and configure OpenVPN
* Generate client configuration
* Use Google DNS
* Proxy traffic from defined IP list
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
2. Run WinSCP as `Administrator` (Right click -> Run as Administrator).
3. Login to VPS:
* Host Name: VPS `IP-address`
* Login: `root`
* Password: `root` password

#### Setup server
1. Open WinSCP terminal (Ctrl+T). Also you can use (PuTTY)[https://www.putty.org/] to execute commands instead WinSCP terminal.
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

#### Setup client
1. Copy (Ctrl+C) client config `ovpnproxy.ovpn` to `C:\Program Files (x86)\OpenVPN\config`.
2. Run OpenVPN GUI.
3. Connect to VPN (Right click on tray icon -> `ovpnproxy` -> Connect).

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

## Variables
Optionally you can set vars in your playbook file. Example with default vars:

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
