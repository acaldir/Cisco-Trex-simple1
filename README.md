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

Load EMU ICMP profile

emu_load_profile -f emu/simple_icmp1.py -t --ns 1 --clients 5
🧪 Usage
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
Test ICMP Connectivity
VM-CSR1K-3#ping 7.7.7.12
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 7.7.7.12, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/8/18 ms
Debugging ICMP Traffic
*Mar 17 08:01:23.760: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.768: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.774: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.782: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.789: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
🖥 Project Structure
.
├── trex_config_gen.py       # Python script to generate TRex config
├── /etc/trex_cfg.yaml       # TRex YAML configuration
└── emu/
    └── simple_icmp1.py      # EMU ICMP profile
🛠 Technologies Used

TRex v3.08

Python 3

Cisco CSR1000v

YAML configuration

📄 License

MIT License – feel free to use and modify for personal or lab testing.
