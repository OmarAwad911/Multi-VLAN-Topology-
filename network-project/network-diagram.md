# 🗺️ Network Diagram — Enterprise Campus Network

## 1. Design Model

This network follows Cisco's **Three-Tier Hierarchical Model**:

```
┌──────────────────────────────────────────────────────────────────┐
│                          INTERNET / ISP                          │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                    ┌──────────┴──────────┐
                    │    EDGE ROUTER       │
                    │  (NAT / Firewall)    │
                    └──────────┬──────────┘
                               │
═══════════════════════════════╪══════════════  CORE LAYER
                    ┌──────────┴──────────┐
                    │    CORE SWITCH       │
                    │  (Layer 3 / 6500)    │
                    └────┬──────────┬─────┘
                         │          │
          ───────────────┘          └─────────────────
          │                                           │
═══════════╪═══════════════════════════════════════════╪══  DISTRIBUTION LAYER
┌──────────┴──────────┐                   ┌───────────┴──────────┐
│  DISTRIBUTION SW-A   │                   │  DISTRIBUTION SW-B   │
│  (Building A / L3)   │                   │  (Building B / L3)   │
└──┬─────┬─────┬──────┘                   └──┬─────┬──────┬──────┘
   │     │     │                              │     │      │
═══╪═════╪═════╪══════════════════════════════╪═════╪══════╪═══  ACCESS LAYER
┌──┴─┐ ┌─┴─┐ ┌┴──┐                        ┌──┴─┐ ┌─┴──┐ ┌─┴──┐
│SW1 │ │SW2│ │SW3│                        │SW4 │ │SW5 │ │SW6 │
│IT  │ │ADM│ │HR │                        │FIN │ │OPS │ │SRV │
└──┬─┘ └─┬─┘ └┬──┘                        └──┬─┘ └─┬──┘ └─┬──┘
   │     │    │                               │     │      │
  PCs   PCs  PCs                            PCs   PCs   Servers
```

---

## 2. Layer Details

### 🔵 Core Layer
| Device | Model | Role |
|--------|-------|------|
| Core-SW | Cisco Catalyst 6500 (or L3 equivalent in PT) | Central backbone switch |
| Edge-Router | Cisco 2911 | NAT, default route, ISP connection |

- Connects all distribution switches
- Handles routing between buildings
- No end-user devices connect here

---

### 🟡 Distribution Layer

| Device | Location | Role |
|--------|----------|------|
| Dist-SW-A | Building A | Aggregates access switches for IT, Admin, HR |
| Dist-SW-B | Building B | Aggregates access switches for Finance, Ops, Servers |

- Implements inter-VLAN routing via SVIs
- Enforces ACL security policies
- Runs STP root bridge per VLAN

---

### 🟢 Access Layer

| Switch | Department | VLAN | Connected Devices |
|--------|------------|------|-------------------|
| SW1 | IT Department | VLAN 10 | IT Staff PCs, Workstations |
| SW2 | Administration | VLAN 20 | Admin PCs, Printers |
| SW3 | Human Resources | VLAN 30 | HR PCs, Printers |
| SW4 | Finance | VLAN 40 | Finance PCs, Terminals |
| SW5 | Operations | VLAN 50 | Operations PCs |
| SW6 | Server Farm | VLAN 99 | DNS, DHCP, Web Servers |

---

## 3. VLAN Trunk Map

All uplinks between Access → Distribution → Core are configured as **802.1Q trunk** ports carrying all VLANs.

```
Access SW ──(trunk)──► Distribution SW ──(trunk)──► Core SW
   (VLAN 10,20,30...)         (VLAN 10,20,30...)
```

Access ports to end devices are configured as **access** ports assigned to their respective VLAN.

---

## 4. Redundancy & Loop Prevention

- **STP (Spanning Tree Protocol)** is enabled on all switches
- Distribution switches are configured as **Root Bridge** per VLAN
- Uplinks between distribution and core use redundant paths with STP blocking one port per loop

---

## 5. Server Farm (VLAN 99)

| Server | IP Address | Service |
|--------|------------|---------|
| DHCP Server | 192.168.99.10 | IP address assignment for all VLANs |
| DNS Server | 192.168.99.11 | Internal name resolution |
| Web Server | 192.168.99.12 | Internal intranet portal |

---

## 6. Internet Connectivity

```
[Core Switch] → [Edge Router Fa0/1: 10.0.0.1/30]
                      ↕ (WAN link)
              [ISP Router Fa0/0: 10.0.0.2/30]
                      ↕
                 [INTERNET CLOUD]
```

NAT/PAT is configured on the Edge Router to translate all internal addresses to the public IP.

---

*Last updated: March 2026*
