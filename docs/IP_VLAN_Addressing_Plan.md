# IP and VLAN Addressing Plan

Total VLANs: **24** (12 in Building 1, 12 in Building 2 and data center).

## Building 1 VLANs

| VLAN | Name | Subnet | VRRP VIP (gateway) | Purpose |
|---|---|---|---|---|
| 10 | Students | 192.168.10.0/24 | 192.168.10.254 | Student access |
| 11 | Staff | 192.168.11.0/24 | 192.168.11.254 | Academic staff |
| 12 | Faculty | 192.168.12.0/24 | 192.168.12.254 | Faculty |
| 13 | Labs | 192.168.13.0/24 | 192.168.13.254 | Computer labs |
| 14 | Guest | 192.168.14.0/24 | 192.168.14.254 | Guest users |
| 15 | Wi-Fi Users | 192.168.15.0/24 | 192.168.15.254 | Wireless clients |
| 16 | CCTV | 192.168.16.0/24 | 192.168.16.254 | Security cameras |
| 17 | Finance | 192.168.17.0/24 | 192.168.17.254 | Finance office |
| 18 | HR | 192.168.18.0/24 | 192.168.18.254 | Human resources |
| 19 | Wi-Fi APs | 192.168.19.0/24 | 192.168.19.254 | Access point management |
| 20 | VoIP | 192.168.20.0/24 | 192.168.20.254 | Voice |
| 99 | Management | 192.168.99.0/24 | 192.168.99.254 | Network management |

## Building 2 and data center VLANs

VLANs **21 to 31 and 100** serve Building 2 and the data center. They are shown in the Building 2 box of the logical topology diagram (`topology/01-logical-topology.jpg`).

## Infrastructure and transit addressing

| Link or item | Address | Source |
|---|---|---|
| EDGE_ROUTER1 to FW1 (master) | 10.10.40.2/30 | Edge router config |
| EDGE_ROUTER1 to FW2 (standby) | 10.10.40.10/30 | Edge router config |
| EDGE_ROUTER1 to ISP | 172.16.255.2/30 | Edge router config |
| Edge VRRP, VLAN 55 (GE0/0/2.55) | 10.12.12.1/28, VIP 10.12.12.14 | Edge router config |
| CORE-1 to firewall side | 10.20.20.2/30 and 10.20.20.10/30 | Core config |
| CORE-1 loopback and router-ID | 1.1.1.1/32 | Core config |
| CORE-2 router-ID | 2.2.2.2 | OSPF sample |
| Firewall HRP virtual IP | 172.16.210.1 | Firewall sample |
| Internal DNS handed out by DHCP | 172.16.220.4 | DHCP test |
| Public DNS handed out by DHCP | 8.8.8.8 | DHCP test |
| DMZ web server (public) | 197.156.10.10 | ISP to DMZ test |
| Core transit VLANs seen in config | 130, 150 | Core config |

## OSPF neighbor router-IDs seen on CORE-1

3.3.3.3, 4.4.4.4, 7.7.7.7, 6.6.6.6, 15.15.15.15, 16.16.16.16, 30.30.30.30, 2.2.2.2 (CORE-2 via Eth-Trunk1).
