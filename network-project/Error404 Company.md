# 📄 Project Documentation — Enterprise Campus Network

## 1. Project Overview

| Field | Details |
|-------|---------|
| **Project Title** |Error404 Company Network Design |
| **Tool Used** | Cisco Packet Tracer 9.0.0 |
| **Network Type** | Enterprise / Campus LAN |
| **Design Model** | Cisco Three-Tier Hierarchical |
| **Completion Date** | March 2026 |
| **Team** |Group C |

---

## 2. Project Goals

This project was designed to simulate a production-level enterprise campus network with the following goals:

1. **Segmentation** — Isolate departments using VLANs to reduce broadcast traffic and improve security
2. **Scalability** — Use a hierarchical design that can grow without redesigning the core
3. **Redundancy** — Ensure no single point of failure using STP and redundant uplinks
4. **Security** — Apply ACLs to prevent unauthorized cross-department access
5. **Manageability** — Enable SSH-based remote management and centralized DHCP/DNS

---

## 3. Design Decisions

### Why Three-Tier Hierarchy?

The three-tier model (Core → Distribution → Access) was chosen because:
- It separates functions clearly, making troubleshooting easier
- Each tier can be upgraded independently
- It scales well from small campuses to large enterprises
- It mirrors real-world enterprise deployments

### Why Layer 3 Switching for Inter-VLAN Routing?

Instead of Router-on-a-Stick, a Layer 3 switch with SVIs was chosen for inter-VLAN routing because:
- Higher throughput (hardware-based vs. software-based routing)
- Cleaner design — no single trunk bottleneck
- Easier to scale with additional VLANs

### Why Centralized DHCP?

A dedicated DHCP server in the server VLAN (VLAN 99) was chosen over router DHCP because:
- Centralized management of all IP scopes
- Easier to audit and maintain
- IP helper-address (`ip helper-address`) on SVIs forwards DHCP requests to the server

---

## 4. IP Addressing Plan

### Subnetting Summary

| VLAN | Department | Network Address | Subnet Mask | Usable Range | Gateway |
|------|------------|-----------------|-------------|--------------|---------|
| 10 | IT | 192.168.10.0/24 | 255.255.255.0 | .1 – .254 | .1 |
| 20 | Administration | 192.168.20.0/24 | 255.255.255.0 | .1 – .254 | .1 |
| 30 | Human Resources | 192.168.30.0/24 | 255.255.255.0 | .1 – .254 | .1 |
| 40 | Finance | 192.168.40.0/24 | 255.255.255.0 | .1 – .254 | .1 |
| 50 | Operations | 192.168.50.0/24 | 255.255.255.0 | .1 – .254 | .1 |
| 99 | Management/Servers | 192.168.99.0/24 | 255.255.255.0 | .1 – .254 | .1 |
| — | WAN Link | 10.0.0.0/30 | 255.255.255.252 | .1 – .2 | — |

### DHCP Pools (configured on DHCP Server)

| Pool Name | Network | Excluded IPs | DNS |
|-----------|---------|--------------|-----|
| POOL_IT | 192.168.10.0/24 | .1 – .9 | 192.168.99.11 |
| POOL_ADMIN | 192.168.20.0/24 | .1 – .9 | 192.168.99.11 |
| POOL_HR | 192.168.30.0/24 | .1 – .9 | 192.168.99.11 |
| POOL_FIN | 192.168.40.0/24 | .1 – .9 | 192.168.99.11 |
| POOL_OPS | 192.168.50.0/24 | .1 – .9 | 192.168.99.11 |

---

## 5. Security Policy

### ACL Rules

| Rule | Source | Destination | Action | Reason |
|------|--------|-------------|--------|--------|
| 1 | VLAN 40 (Finance) | VLAN 30 (HR) | DENY | Finance should not access HR data |
| 2 | VLAN 30 (HR) | VLAN 40 (Finance) | DENY | HR should not access financial systems |
| 3 | VLAN 99 (Servers) | Any | PERMIT | Servers must be reachable by all |
| 4 | Any | Any | PERMIT | Default allow for remaining traffic |

### SSH Management

All switches and routers are configured with:
- SSH v2 only (Telnet disabled)
- Management access restricted to VLAN 99
- Strong passwords and `enable secret`

---

## 6. Challenges & Solutions

| Challenge | Solution Applied |
|-----------|-----------------|
| DHCP not reaching across VLANs | Configured `ip helper-address` on each SVI |
| Switching loops after adding redundant links | Verified STP was active; configured root bridge priority |
| PCs not getting correct VLAN | Checked access port VLAN assignments with `show vlan brief` |
| Inter-VLAN routing not working | Ensured `ip routing` was enabled on the Layer 3 switch |

---

## 7. Key Commands Reference

```cisco
! Verify VLANs
show vlan brief

! Verify trunk links
show interfaces trunk

! Verify IP routing
show ip route

! Verify STP
show spanning-tree

! Verify DHCP bindings (on server or router)
show ip dhcp binding

! Verify interface status
show ip interface brief

! Check SSH sessions
show ssh
```

---

## 8. Future Improvements

- [ ] Add **wireless LAN controllers (WLC)** and access points per floor
- [ ] Implement **OSPF** area-based routing instead of static routes
- [ ] Add **redundant core switches** for full high availability
- [ ] Configure **port security** on access layer switches
- [ ] Simulate **VoIP** using IP phones on a dedicated voice VLAN
- [ ] Add **firewall** (ASA) between edge router and core

---

## 9. References

- [Cisco Networking Academy](https://www.netacad.com)
- [Cisco Three-Tier Hierarchical Design](https://www.cisco.com/c/en/us/td/docs/solutions/Enterprise/Campus/campover.html)
- [Cisco IOS Command Reference](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-15-4m-t/products-command-reference-list.html)
- [CCNA Study Guide — Todd Lammle](https://www.sybex.com)

---

*Documentation prepared by: Omar Awad | March 2026*
