# TRex EMU ICMP Test Setup

This repository contains scripts and configuration examples for running **TRex EMU** with ICMP testing between virtual Cisco routers (CSR1K). It demonstrates the setup, configuration, and verification steps.

## 📌 Project Description

This project sets up a TRex EMU environment for testing ICMP connectivity between virtual Cisco routers. It includes:

- Generating TRex configuration using `trex_config_gen.py`
- Editing TRex YAML configuration (`/etc/trex_cfg.yaml`)
- Loading EMU profiles for ICMP traffic
- Verifying connectivity using CSR1K routers

## ⚙️ Requirements

- Linux host (Ubuntu recommended)
- TRex v3.08
- Python 3.x
- Cisco CSR1000v virtual routers
- Access to `/etc/trex_cfg.yaml` for configuration

## 🚀 Installation & Setup

1. **Generate TRex Configuration**

```bash
python3 trex_config_gen.py

or manually edit /etc/trex_cfg.yaml to set interfaces and IPs:

- port_limit: 2
  version: 2
  low_end: true
  interfaces: ["eth0", "eth1"]
  port_info:
    - ip: 99.1.1.99
      default_gw: 99.1.1.1
    - ip: 7.7.7.99
      default_gw: 7.7.7.1

Start TRex in interactive EMU mode

./t-rex-64 -i -c 1 --software --emu

Load EMU ICMP profile in different console

emu_load_profile -f emu/simple_icmp1.py -t --ns 1 --clients 5

Verify Interface Configuration on CSR Routers

VM-CSR1K-3#show running-config interface gigabitEthernet 2
interface GigabitEthernet2
 ip address 99.1.1.1 255.255.255.0
 load-interval 30
 negotiation auto
 arp timeout 70
 no mop enabled
 no mop sysid
end

VM-CSR1K-6#show running-config interface gigabitEthernet 2
interface GigabitEthernet2
 ip address 7.7.7.1 255.255.255.0
 load-interval 30
 negotiation auto
 arp timeout 70
 no mop enabled
 no mop sysid
end
