# 🏢 Enterprise Campus Network Design
### Cisco Packet Tracer Simulation

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Network Type](https://img.shields.io/badge/Network-Enterprise%20%2F%20Campus-0052CC?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-28a745?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-f0a500?style=for-the-badge)

> A fully simulated **Enterprise Campus Network** built with Cisco Packet Tracer.  
> Demonstrates hierarchical network design, VLAN segmentation, inter-VLAN routing, redundancy, and multi-department connectivity across a corporate campus environment.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Network Topology](#-network-topology)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Department VLANs](#-department-vlans)
- [IP Addressing Scheme](#-ip-addressing-scheme)
- [Testing & Verification](#-testing--verification)
- [Learning Objectives](#-learning-objectives)
- [Author](#-author)

---

## 🌐 Overview

This project simulates a real-world **enterprise campus network** serving multiple departments across a multi-floor or multi-building environment. The design follows Cisco's **three-tier hierarchical model**:

| Layer | Role |
|-------|------|
| **Core Layer** | High-speed backbone — fast packet forwarding between buildings/blocks |
| **Distribution Layer** | Policy enforcement, inter-VLAN routing, redundancy |
| **Access Layer** | End-device connectivity — PCs, printers, IP phones per department |

---

## 🗺️ Network Topology

See full details → [`docs/network-diagram.md`](docs/network-diagram.md)

```
                        ┌─────────────────────┐
                        │     CORE SWITCH      │
                        │   (Layer 3 / L3SW)   │
                        └────────┬────────┬────┘
                                 │        │
               ┌─────────────────┘        └──────────────────┐
               │                                              │
     ┌─────────┴──────────┐                      ┌───────────┴────────┐
     │  DIST. SWITCH A     │                      │  DIST. SWITCH B    │
     │  (Building A)       │                      │  (Building B)      │
     └──┬──────┬──────┬───┘                      └──┬──────┬──────┬───┘
        │      │      │                              │      │      │
     [ACC]  [ACC]  [ACC]                          [ACC]  [ACC]  [ACC]
      SW1    SW2    SW3                            SW4    SW5    SW6
       │      │      │                              │      │      │
     [IT] [Admin] [HR]                           [Fin] [Ops]  [Srv]
```

---

## 🛠️ Technologies Used

| Technology | Purpose |
|------------|---------|
| **VLANs** | Department segmentation and traffic isolation |
| **Inter-VLAN Routing** | Layer 3 switch or Router-on-a-Stick |
| **STP / RSTP** | Loop prevention and redundancy |
| **EtherChannel / LACP** | Link aggregation for uplinks |
| **DHCP** | Dynamic IP assignment per VLAN |
| **DNS** | Internal name resolution |
| **OSPF / Static Routing** | Campus-wide routing |
| **ACLs** | Security — restrict inter-department access |
| **NAT/PAT** | Internet access via edge router |
| **SSH** | Secure remote device management |

---

## 📁 Project Structure

```
enterprise-campus-network/
│
├── project_new.pkt              # Cisco Packet Tracer simulation file
├── README.md                    # This file
├── LICENSE
│
├── docs/
│   ├── network-diagram.md       # Full topology & diagram description
│   └── project-documentation.md # Design decisions, goals, and analysis
│
└── configs/
    └── device-configs.md        # CLI configurations for all devices
```

---

## 🚀 Getting Started

### Prerequisites

- [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) **v8.0 or later**
- Free registration at [Cisco NetAcad](https://www.netacad.com)

### Open the Project

```bash
# 1. Clone this repository
git clone https://github.com/YOUR_USERNAME/enterprise-campus-network.git

# 2. Navigate to the project folder
cd enterprise-campus-network

# 3. Open the .pkt file in Cisco Packet Tracer
#    File → Open → select project_new.pkt
```

---

## 🏷️ Department VLANs

| VLAN ID | Department | Network | Color Code |
|---------|------------|---------|------------|
| VLAN 10 | IT Department | 192.168.10.0/24 | 🔵 |
| VLAN 20 | Administration | 192.168.20.0/24 | 🟣 |
| VLAN 30 | Human Resources | 192.168.30.0/24 | 🟡 |
| VLAN 40 | Finance | 192.168.40.0/24 | 🟢 |
| VLAN 50 | Operations | 192.168.50.0/24 | 🟠 |
| VLAN 99 | Management | 192.168.99.0/24 | ⚫ |

> *(Update with your actual VLAN IDs and subnets)*

---

## 📊 IP Addressing Scheme

| Device | Interface | IP Address | Subnet Mask | Role |
|--------|-----------|------------|-------------|------|
| Core-Switch | VLAN 10 SVI | 192.168.10.1 | 255.255.255.0 | Gateway IT |
| Core-Switch | VLAN 20 SVI | 192.168.20.1 | 255.255.255.0 | Gateway Admin |
| Core-Switch | VLAN 40 SVI | 192.168.40.1 | 255.255.255.0 | Gateway Finance |
| Edge-Router | Fa0/0 | 10.0.0.1 | 255.255.255.252 | WAN Uplink |
| DHCP Server | NIC | 192.168.99.10 | 255.255.255.0 | DHCP/DNS |

---

## ✅ Testing & Verification

Inside Packet Tracer, verify the network with these steps:

**1. Ping between departments (inter-VLAN routing):**
```
PC-IT> ping 192.168.20.x
PC-Finance> ping 192.168.40.x
```

**2. Check VLAN assignments on a switch:**
```cisco
Switch# show vlan brief
Switch# show interfaces trunk
```

**3. Verify routing table:**
```cisco
Core-Switch# show ip route
```

**4. Test DHCP lease:**
```
PC> ipconfig /renew
PC> ipconfig
```

**5. Use Packet Tracer PDU tool** to trace packet paths between any two devices.

---

## 📚 Learning Objectives

This project demonstrates proficiency in:

- [x] Hierarchical (3-tier) campus network design
- [x] VLAN creation and trunk configuration
- [x] Inter-VLAN routing using Layer 3 switch SVIs
- [x] DHCP server configuration for multiple scopes
- [x] Spanning Tree Protocol (STP/RSTP)
- [x] Access Control Lists (ACLs) for security
- [x] Network documentation and IP planning
- [ ] EtherChannel / LACP *(mark if implemented)*
- [ ] OSPF dynamic routing *(mark if implemented)*
- [ ] NAT/PAT for internet simulation *(mark if implemented)*

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first.

```bash
git checkout -b feature/add-ospf-routing
git commit -m "feat: configure OSPF between distribution switches"
git push origin feature/add-ospf-routing
```

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

## 👤 Author

**Your Name**
- 🐙 GitHub: https://github.com/OmarAwad911
- 💼 LinkedIn: https://www.linkedin.com/in/omar-eldomyaty/
- 📧 Email: omareldomyaty70@gmail.com

---

> 💡 *Designed to simulate real enterprise networking scenarios using Cisco technologies.*  
> ⭐ *If this project helped you, consider giving it a star!*
