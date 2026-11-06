# Device Configurations: Addressing and Wiring Plan

These configurations were rebuilt from the project documentation. They are not the original device dumps.

## Build order

1. ISP_R, EDGE_ROUTER1, EDGE_ROUTER2
2. FW1, then FW2 (policies and NAT are entered on FW1 only and sync to FW2 through HRP)
3. CORE-1, CORE-2
4. CAM-1 DSW-1, CAM-1 DSW-2, CAM-2 DSW-1, CAM-2 DSW-2
5. DC_DSW-1, DC_DSW-2, DC_SW, DHCP-SERVER
6. Access switches AC-1 to AC-9
7. Servers and PCs

## Wiring and addressing

### Edge and firewalls

| Link | Subnet | Side A | Side B |
|---|---|---|---|
| ISP_R GE0/0/0 to EDGE_ROUTER1 GE4/0/0 | 172.16.255.0/30 | .1 | .2 |
| ISP_R GE0/0/1 to EDGE_ROUTER2 GE4/0/0 | 172.16.255.4/30 | .5 | .6 |
| EDGE_ROUTER1 GE0/0/0 to FW1 GE1/0/0 | 10.10.40.0/30 | .2 | .1 |
| EDGE_ROUTER2 GE0/0/0 to FW1 GE1/0/1 | 10.10.40.4/30 | .6 | .5 |
| EDGE_ROUTER1 GE0/0/1 to FW2 GE1/0/0 | 10.10.40.8/30 | .10 | .9 |
| EDGE_ROUTER2 GE0/0/1 to FW2 GE1/0/1 | 10.10.40.12/30 | .14 | .13 |
| EDGE_ROUTER1 GE0/0/2 to EDGE_ROUTER2 GE0/0/2 | 10.12.12.0/28 (VLAN 55, VIP .14) | .1 | .2 |
| FW1 GE1/0/5 to FW2 GE1/0/5 (heartbeat) | 172.16.210.0/30 | .1 | .2 |
| FW1 GE1/0/4, FW2 GE1/0/4 to DMZ_SW (VIP .1) | 172.16.230.0/24 | .2 | .3 |

### Firewalls to cores

| Link | Subnet | Core | Firewall |
|---|---|---|---|
| CORE-1 GE1/0/6 to FW1 GE1/0/2 | 10.20.20.0/30 | .2 | .1 |
| CORE-2 GE1/0/6 to FW1 GE1/0/3 | 10.20.20.4/30 | .6 | .5 |
| CORE-1 GE1/0/8 to FW2 GE1/0/2 | 10.20.20.8/30 | .10 | .9 |
| CORE-2 GE1/0/8 to FW2 GE1/0/3 | 10.20.20.12/30 | .14 | .13 |

### Cores to distribution, and between cores

| Link | Subnet | Core | Switch |
|---|---|---|---|
| CORE-1 GE1/0/0 to CAM-1 DSW-1 GE0/0/1 | 10.1.0.0/30 | .1 | .2 |
| CORE-1 GE1/0/1 to CAM-1 DSW-2 GE0/0/1 | 10.1.0.4/30 | .5 | .6 |
| CORE-1 GE1/0/2 to CAM-2 DSW-1 GE0/0/1 | 10.2.0.0/30 | .1 | .2 |
| CORE-1 GE1/0/3 to CAM-2 DSW-2 GE0/0/1 | 10.1.0.8/30 | .9 | .10 |
| CORE-2 GE1/0/0 to CAM-1 DSW-1 GE0/0/2 | 10.1.0.12/30 | .13 | .14 |
| CORE-2 GE1/0/1 to CAM-1 DSW-2 GE0/0/2 | 10.2.0.8/30 | .9 | .10 |
| CORE-2 GE1/0/2 to CAM-2 DSW-1 GE0/0/2 | 10.2.0.4/30 | .5 | .6 |
| CORE-2 GE1/0/3 to CAM-2 DSW-2 GE0/0/2 | 10.2.0.12/30 | .13 | .14 |
| CORE-1 Eth-Trunk1 (GE1/0/4, 1/0/5) to CORE-2 Eth-Trunk1 (GE1/0/4, 1/0/5) | 10.0.0.0/30 | .1 | .2 |
| CORE-1 GE1/0/9 (VLAN 130) to DC_DSW-1 GE0/0/1 | 10.10.0.0/30 | .1 | .2 |
| CORE-2 GE1/0/9 (VLAN 140) to DC_DSW-2 GE0/0/1 | 10.10.0.4/30 | .5 | .6 |

### Inside each distribution pair and data center

- DSW-1 GE0/0/3 and GE0/0/4 to DSW-2 GE0/0/3 and GE0/0/4: Eth-Trunk2 (CAM) or Eth-Trunk1 (DC)
- CAM DSW GE0/0/5 to GE0/0/8 (CAM-2 also GE0/0/9): trunks to the access switches
- DC_DSW GE0/0/5 to DC_SW GE0/0/1 (DSW-1) and GE0/0/2 (DSW-2)
- Access switch GE0/0/1 to DSW-1, GE0/0/2 to DSW-2, on the same port number: AC-1 on GE0/0/5, AC-2 on 6, AC-3 on 7, AC-4 on 8, AC-5 on 5, AC-6 on 6, AC-7 on 7, AC-8 on 8, AC-9 on 9

## Hosts

| Device | Address | Gateway |
|---|---|---|
| DHCP-SERVER GE0/0/0 | 172.16.220.2/24 | 172.16.220.254 |
| DB server | 172.16.220.3/24 | 172.16.220.254 |
| Internal DNS | 172.16.220.4/24 | 172.16.220.254 |
| MGT host | 172.16.220.5/24 | 172.16.220.254 |
| DMZ web server | 172.16.230.10/24 | 172.16.230.1 |
| DMZ public DNS | 172.16.230.11/24 | 172.16.230.1 |
| DMZ mail server | 172.16.230.12/24 | 172.16.230.1 |
| PCs | DHCP | VLAN gateway (.254) |

Public addresses: web server published as 197.156.10.10 (ports 80, 443). Internal users share 197.156.10.100 (PAT). ISP_R loopback 8.8.8.8 stands in for the internet DNS.

## Access switch VLANs

AC-1 VLAN 10, AC-2 VLAN 11, AC-3 VLAN 17, AC-4 VLAN 19, AC-5 VLAN 21, AC-6 VLAN 22, AC-7 VLAN 23, AC-8 VLAN 24, AC-9 VLAN 25.
AC-2, AC-3 and AC-4 come from your VLAN diagram. The others are placeholders you can change.

## Router IDs

CORE-1 1.1.1.1, CORE-2 2.2.2.2, CAM-1 DSW-1 3.3.3.3, CAM-1 DSW-2 4.4.4.4, CAM-2 DSW-2 6.6.6.6, CAM-2 DSW-1 7.7.7.7, FW1 15.15.15.15, FW2 16.16.16.16, DC_DSW-1 30.30.30.30, DC_DSW-2 31.31.31.31

## Verify

| Check | Command | Expected |
|---|---|---|
| OSPF on CORE-1 | `display ospf peer brief` | 8 neighbors, all Full |
| Firewall HA | `display hrp state` | FW1 active, FW2 standby |
| Gateway redundancy | `display vrrp brief` | DSW-1 Master, DSW-2 Backup |
| Trunks | `display eth-trunk 2` | Both ports Selected |
| DHCP | `ipconfig` on a PC | Address from the VLAN subnet, DNS 172.16.220.4 and 8.8.8.8 |
| Internet to DMZ | `telnet 197.156.10.10 80` from ISP_R | Connected |
| Student policy | Student PC pings Finance PC | Blocked |
| Student policy | Student PC pings 8.8.8.8 | Reply |
| Snooping | `display dhcp snooping user-bind all` on an access switch | Binding entries |
| Port security | `display port-security interface Ethernet0/0/1` | Enabled |
