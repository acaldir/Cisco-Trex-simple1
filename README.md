ekilde:

# TRex EMU ICMP Test Setup

Bu proje, **TRex EMU** ile sanal Cisco CSR1K routerlar arasında ICMP testi yapmak için gerekli yapılandırmaları ve komut örneklerini içerir. TRex ortamını başlatma, EMU profili yükleme ve ağ bağlantısını doğrulama adımlarını kapsar.

## 📌 Proje Açıklaması

Bu proje, sanal CSR routerlar arasında ICMP bağlantısını test etmek için TRex EMU ortamını kurar. İçerikler:

- `trex_config_gen.py` ile TRex konfigürasyonu oluşturma
- `/etc/trex_cfg.yaml` dosyasını düzenleme
- EMU ICMP profilini yükleme
- CSR1K routerlar üzerinden bağlantıyı doğrulama

## ⚙️ Gereksinimler

- Linux host (Ubuntu önerilir)
- TRex v3.08
- Python 3.x
- Cisco CSR1000v sanal routerlar
- `/etc/trex_cfg.yaml` dosyasına erişim

## 🚀 Kurulum ve Başlatma

1. **TRex Konfigürasyonu Oluşturma**

```bash
python3 trex_config_gen.py

veya manuel olarak /etc/trex_cfg.yaml dosyasını düzenleyin:

- port_limit: 2
  version: 2
  low_end: true
  interfaces: ["eth0", "eth1"]
  port_info:
    - ip: 99.1.1.99
      default_gw: 99.1.1.1
    - ip: 7.7.7.99
      default_gw: 7.7.7.1

TRex’i interactive EMU modunda başlatma

./t-rex-64 -i -c 1 --software --emu

EMU ICMP profilini yükleme

emu_load_profile -f emu/simple_icmp1.py -t --ns 1 --clients 5
🧪 Kullanım
CSR Router Arayüz Konfigürasyonunu Doğrulama
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
ICMP Bağlantısını Test Etme
VM-CSR1K-3#ping 7.7.7.12
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 7.7.7.12, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/8/18 ms
ICMP Trafiğini Debug Etme
*Mar 17 08:01:23.760: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.768: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.774: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.782: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
*Mar 17 08:01:23.789: ICMP: echo reply rcvd, src 7.7.7.12, dst 36.1.1.3, topology BASE, dscp 0 topoid 0
🖥 Proje Yapısı
.
├── trex_config_gen.py       # TRex konfigürasyonu oluşturma scripti
├── /etc/trex_cfg.yaml       # TRex YAML konfigürasyonu
└── emu/
    └── simple_icmp1.py      # EMU ICMP profili
🛠 Kullanılan Teknolojiler

TRex v3.08

Python 3

Cisco CSR1000v

YAML konfigürasyonu

📄 Lisans

MIT License – Kişisel veya lab testleri için kullanabilir ve değiştirebilirsiniz.
