# Test Plan and Results

Environment: Huawei eNSP 1.3.00.200T, VirtualBox 5.2.44, WinPcap 4.1.3, Wireshark 3.6.10.

## A. Tests with evidence

| # | Area | Test | Expected | Result | Screenshot |
|---|---|---|---|---|---|
| 1 | Routing | `display ospf peer brief` on CORE-1 | All neighbors Full | 8 of 8 Full | `screenshots/routing/ospf-peer-brief-core1.png` |
| 2 | Routing | OSPF routing table on a remote device | VLAN subnets learned via OSPF | Pass | `screenshots/routing/ospf-routing-table.png` |
| 3 | Redundancy | `display vrrp brief` on edge routers | R1 Master, R2 Backup | Pass | `screenshots/redundancy/vrrp-edge-routers.png` |
| 4 | Redundancy | `display vrrp brief` on CAM-1 DSW-1/2 | DSW-1 Master, DSW-2 Backup for 12 VLANs | Pass | `screenshots/redundancy/vrrp-cam1-dsw.png` |
| 5 | Redundancy | `display eth-trunk 2` | Up, members selected | Up, 2 ports selected | `screenshots/redundancy/eth-trunk2.png` |
| 6 | Firewall | `display hrp state` | Active and standby, priority 45000 | Pass | `screenshots/firewall/hrp-state-normal.png` |
| 7 | Firewall | Shut down FW1 | FW2 becomes active | Pass | `screenshots/firewall/hrp-failover.png` |
| 8 | Services | DHCP on a VLAN 12 host | Address, gateway, DNS | 192.168.12.253, GW .254, DNS 172.16.220.4 and 8.8.8.8 | `screenshots/services/dhcp-ipconfig.png` |
| 9 | Connectivity | Ping 10.1.0.1 from PC | No loss | 5 of 5, 0% loss, 47 to 93 ms | `screenshots/services/ping-10-1-0-1.png` |
| 10 | DMZ | Telnet to 197.156.10.10 port 80 from ISP router | HTTP response | Connected, HTTP 404 from eNSP HttpServer (service reachable) | `screenshots/services/isp-to-dmz-http.png` |
| 11 | Health | Firewall CPU and memory | Healthy | CPU about 23%, memory 65% | `screenshots/firewall/cpu-memory.png` |

## B. Tests described in the report, screenshots to add

| # | Area | Test | Expected |
|---|---|---|---|
| 12 | Redundancy | Power off master distribution switch | Backup becomes VRRP master, sessions continue |
| 13 | Edge | Disconnect one ISP link | Floating static route takes over |
| 14 | Security | Port scan from untrusted side | Firewall blocks access to Trust zone |
| 15 | Routing | Core link failure | OSPF reroutes quickly |

## C. Tests to run when the lab is rebuilt

| # | Area | Test | Expected |
|---|---|---|---|
| 16 | ACL | Student ping to Staff, Finance, HR, Management | Blocked |
| 17 | ACL | Student to DNS, DHCP, Internet | Permitted |
| 18 | ACL | Guest to any internal VLAN | Blocked |
| 19 | L2 | Rogue DHCP server on access port | Offers dropped by DHCP snooping |
| 20 | L2 | Static IP with spoofed ARP | Dropped by DAI |
| 21 | L2 | Extra MAC on a secured port | Port-security violation action |
| 22 | L2 | Switch plugged into edge port | Port shut by BPDU protection |
| 23 | NAT | Internet user to published DMZ web service | Reachable through port forwarding |
| 24 | QoS | Congested link with mixed classes | High-priority classes protected |
