# Rocky Router
An ansible role to configure a Rocky linux host as a home/edge router. 

## Requirements
- 2 Network Interfaces for LAN and WAN
- Firewalld
- NetworkManager
- Dnsmasq

### Supported Distributions
- Tested: Rocky Linux 9.x
- Expected: Rocky Linux 8, 10 and other RHEL-based distributions

### Core Features
- IP Routing
- Firewall
- DHCP
- NAT
- DNS
- DNF automatic

### Additional Features
- SSH hardening over WAN
- VPN using Wiregaurd
- Port forwarding
- DNS blocklist
- fail2ban

#### Core Variables (Dictionary)
```yaml
rocky_router:
  # IP info
  prefixlen: "24"
  lan_ip: "192.168.10.1"
  # Netowrk Interfaces
  wan_if: eno1
  bridge_if: br0
  lan_ifs: [enp1s0, enp2s0]
  # Firewall
  lan_zone: lan
  wan_zone: wan
  # DNS
  dns_servers: ["8.8.8.8", "8.8.4.4"]
  local_domain: local.arpa
  # DHCP
  dhcp_start_ip: "192.168.10.50"
  dhcp_end_ip: "192.168.10.254"
  dhcp_lease_time: "24h"
  # DNF Automatic updates
  dnf_auto_timer: dnf-automatic-install.timer
  dnf_auto_frequency: "Sat *-*-* 00:00:00"
  # Additional features
  wan_ssh_enabled: false
  wiregaurd_enabled: false
  dns_blocklist_enabled: false
  fail2ban_enabled: false
  port_forwarding_enabled: false
```

#### Additional Variables
```yaml
# SSH
rocky_router_ssh:
  wan_port: "23432"
  wan_allowed_users: ["rruser"]
  wan_permit_passwords: "no"
  wan_permit_keys: "yes"
  banner_file: "/etc/ssh/banner"
  banner_text: "Authorized Users Only. Connections subject to monitoring."

# Wireguard (TBD)

# DNS Blocklist (TBD)

# Fail2ban (TBD)

# Port Forwarding (TBD)
```

#### Sample playbook using the role
```yaml
---
- name: Rocky Router
  hosts: all
  become: yes
  vars_files:
    - vars/sample.yml
  tasks:
    - name: Include rocky router role
      ansible.builtin.include_role:
        name: machinemonkeys.rocky_router
    -
```

#### Foot Notes:

- The default outcome should be a host configured as a local gateway, firewall, dhcp server, and dns server.
- You can copy and paste the variable dictionaries and make changes to customize the configurations.
- You can enable additional features by setting them in the main dictionary to true.
- The LAN IP for the router will be used by the bridge interface defined. 
- LAN facing network interfaces can be configured as bridge slaves to service clients/devices connected to them such as switches, access points, hosts etc.
- In order to maximize the use of the machine, currently looking into the following features:
  - qemu/kvm hypervisor
  - IDS/IPS
  - p2p node
