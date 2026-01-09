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
  prefixlen: ""

rocky_router_ifs:
  wan: ""
  lan_bridge: ""
  bridge_slaves: []

rocky_router_fw:
  lan_zone: lan
  lan_services: []
  wan_zone: wan
  wan_icmp_block: false
  wan_target: "REJECT"
  wan_services: []
  policy: "lan-to-wan"
  policy_target: "ACCEPT"

rocky_router_dhcp:
  start_ip: ""
  end_ip: ""
  lease_time: "24h"
  lease_file: "/var/lib/dnsmasq/dnsmasq.leases"
  log_facility: "/var/log/dnsmasq.log"

rocky_router_dnf_auto:
  timer: "dnf-automatic-install.timer"
  on_calendar: "Sat *-*-* 00:00:00"

rocky_router_dns:
  servers: []
  local_domain: home.arpa
  directives:
    - "no-resolv"
    - "bogus-priv"
    - "expand-hosts"
    - "domain-needed"
    - "log-queries"
    - "log-async=50"
    - "cache-size=1000"

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
    - vars/sample.yml # Copy/edit role vars to a vars file
  roles:
    role: machinemonkeys.rocky_router
```

License
-------

MIT

Author Information
------------------

_Gino Curtis_

_machinemonkeys.com_
