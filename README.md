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
rocky_router_features:
  dnsmasq: false
  wan_ssh: false
  wiregaurd: false
  dns_blocklist: false
  fail2ban: false
  port_forwarding: false

rocky_router_ip:
  addr: ""
  prefix: ""

rocky_router_ifs:
  wan: ""
  lan_bridge: ""
  bridge_slaves: []

rocky_router_fw:
  lan_zone: lan
  wan_zone: wan
  policy: "lan-to-wan"

rocky_router_dhcp:
  start_ip: ""
  end_ip: ""
  lease_time: "24h"

rocky_router_dnf_auto:
  timer: "dnf-automatic-install.timer"
  on_calendar: "Sat *-*-* 00:00:00"

rocky_router_dns:
  servers: []
  local_domain: home.arpa

rocky_router_ssh:
  wan_port: ""
  wan_allowed_users: []
  wan_permit_passwords: "no"
  wan_permit_keys: "yes"
  banner_file: ""
  banner_text: ""
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
