Rocky Router
=========

An ansible role to configure a Rocky linux host as a home/edge router. 

Requirements
------------

- 2 Network Interfaces for LAN and WAN
- Firewalld
- NetworkManager
- Dnsmasq

Role Variables
--------------

```yaml
# Core variable dictionary
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

# SSH variable dictionary
rocky_router_ssh:
  wan_port: "23432"
  wan_allowed_users: ["rruser"]
  wan_permit_passwords: "no"
  wan_permit_keys: "yes"
  banner_file: "/etc/ssh/banner"
  banner_text: "Authorized Users Only. Connections subject to monitoring."
```

Dependencies
------------

N/A

Example Playbook
----------------

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
```

License
-------

MIT

Author Information
------------------

_Gino Curtis_

_machinemonkeys.com_
