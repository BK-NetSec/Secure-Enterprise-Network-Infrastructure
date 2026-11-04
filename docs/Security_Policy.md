# Security Policy

## 1. Principles

- **Segmentation:** VLANs separate users by role, department and data sensitivity.
- **Least privilege:** each VLAN reaches only the resources it needs.
- **Defense in depth:** perimeter firewall, inter-VLAN ACLs, Layer 2 protections, management isolation.

## 2. Inter-VLAN access matrix

| Role (VLAN) | Permitted | Denied |
|---|---|---|
| Students (10) | Internet, DNS, DHCP, Labs (13) | Staff (11), Faculty (12), Finance (17), HR (18), Management (99), database servers |
| Staff (11) | Internet, DNS, DHCP, database servers | Other sensitive VLANs |
| Faculty (12) | Internet, DNS, DHCP, database servers, Labs (13), Management (99) | Finance and HR unless authorized |
| Labs (13) | Internet, DNS, DHCP | Sensitive VLANs |
| Guest (14) | Internet, DNS | All internal VLANs |
| Wi-Fi Users (15) | Internet, DNS | All internal VLANs |
| Finance (17), HR (18) | Internet, DNS, DHCP, database servers, Management | Other sensitive VLANs without authorization |
| Management (99) | All VLANs and servers | None |

**Database servers** are reachable only by Staff (11), Faculty (12), Finance (17) and HR (18).
**DNS and Internet** are available to all VLANs. **DHCP** is available to all user VLANs.

> Open item: confirm whether Students (10) may reach the DMZ. The report does not say. Update this table and the report to agree.

## 3. Perimeter

| Control | Description |
|---|---|
| Zones | Trust, Untrust, DMZ on the NGFW pair |
| High availability | HRP active/standby (FW1 priority 100, FW2 priority 90) |
| NAT | Source NAT for internal users at the edge |
| Port forwarding | Public DMZ web services and applications published through the firewall (for example web server 197.156.10.10:80) |
| Inspection | Stateful packet inspection between zones |

## 4. Layer 2 security (access layer)

| Control | Purpose |
|---|---|
| Port security | Limit MAC addresses per port, stop unauthorized devices and MAC flooding |
| DHCP snooping | Block rogue DHCP servers, allow DHCP replies only from trusted ports |
| DHCP binding table | Records IP, MAC, VLAN and port for each lease |
| Dynamic ARP Inspection (DAI) | Drop spoofed ARP using the binding table |
| STP edge ports | Fast convergence on end-user ports |
| BPDU protection | Shut down an access port that receives BPDUs (rogue switch) |
| Rapid PVST+ / RSTP | Fast, loop-free layer 2 convergence |

## 5. QoS classes

| Priority | VLANs |
|---|---|
| Control | Management (99) |
| High | CCTV (16), Finance (17), HR (18) |
| Medium | Staff (11), Faculty (12), Labs (13), Wi-Fi APs (19) |
| Low | Students (10), Guest (14), Wi-Fi Users (15) |

## 6. Evidence status

| Control | Evidence in repository |
|---|---|
| NGFW HRP and failover | Yes |
| DMZ reachability from internet | Yes |
| Inter-VLAN ACL enforcement | Not yet: add blocked and permitted ping screenshots |
| Port security, DHCP snooping, DAI, BPDU protection | Not yet: add `display` command output |
| Port forwarding rule | Not yet: add the firewall NAT server output |
| QoS | Not yet: add policy output |
