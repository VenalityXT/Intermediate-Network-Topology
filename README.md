# Intermediate Network Topology & Enterprise Routing Design

[![Cisco](https://img.shields.io/badge/Platform-Cisco%20IOS-blue?logo=cisco)]()
[![Networking](https://img.shields.io/badge/Focus-Network%20Engineering-green)]()
[![Layer3](https://img.shields.io/badge/Layer-L3%20Routing-orange)]()
[![Firewall](https://img.shields.io/badge/Security-Firewall%20Architecture-red)]()
[![Redundancy](https://img.shields.io/badge/High%20Availability-VRRP-lightgrey)]()
[![Visualization](https://img.shields.io/badge/Tool-diagrams.net-blueviolet)]()

---

## <span style="color:#2b6cb0">Executive Summary</span>

> A detailed and scalable intermediate-level enterprise network topology demonstrating segmentation, VRRP-based gateway redundancy, firewall boundary enforcement, L2/L3 separation, and high-availability access-layer uplinks.  
>  
> The design models real enterprise networking principles: hierarchical segmentation, DMZ isolation, dual-firewall architecture, collapsed core routing, and resilient link paths.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Design Principles & Intent](#design-principles--intent)
- [Capabilities Demonstrated](#capabilities-demonstrated)
- [Technologies Used](#technologies-used)
- [Network Architecture Overview](#network-architecture-overview)
- [Layered Design Breakdown](#layered-design-breakdown)
  - [DMZ Network](#dmz-network)
  - [Core Layer](#core-layer)
  - [Firewall Layer](#firewall-layer)
  - [Access Layer](#access-layer)
  - [Redundancy & High Availability](#redundancy--high-availability)
- [Routing Configuration Summary](#routing-configuration-summary)
- [Engineering Rationale](#engineering-rationale)
- [Skills Demonstrated](#skills-demonstrated)
- [Key Takeaways](#key-takeaways)

---

## <span style="color:#2b6cb0">Project Overview</span>

This project presents a professionally structured enterprise-style network topology built around a **collapsed core** with:

- Two Layer 3 core switches running VRRP  
- Dual external firewalls  
- Segmented VLANs for user, server, voice, infrastructure, and DMZ networks  
- Dual-homed access switches  
- NAT and firewall-driven traffic boundaries  
- A dedicated redundancy VLAN for control-plane heartbeat traffic  

The design focuses on realistic architectural intent—mirroring the networks seen in mid-sized organizations.

---

## <span style="color:#2b6cb0">Design Principles & Intent</span>

### **1. High Availability as a Core Requirement**
- Dual core switches  
- VRRP virtual gateway  
- Dual firewalls for WAN survivability  
- Redundant access-layer uplinks  

### **2. Security Zoning and Isolation**
- A fully isolated DMZ (VLAN 400)  
- Internal networks routed only through the core  
- All external and DMZ communication forced through firewalls  

### **3. Predictable and Maintainable Routing**
- All internal VLANs terminate on core SVIs  
- Firewalls exclusively handle WAN and NAT logic  
- VRRP provides a single consistent gateway address  

### **4. Scalability Without Redesign**
- VLAN numbering aligned by function  
- Subnet ranges sized for growth  
- Collapsed core model reduces complexity but supports expansion  

### **5. Clarity of Traffic Domains**
- Inside, outside, DMZ, and redundancy traffic are deliberately separated  
- Reduces lateral movement risk and simplifies troubleshooting  

---

## <span style="color:#2b6cb0">Capabilities Demonstrated</span>

### **High Availability Routing**
- VRRP virtual IP: **10.100.100.1**  
- Immediate gateway failover between core switches  
- Ensures routing continuity under device or uplink failure  

### **Security Zoning**
- DMZ cannot communicate internally without firewall approval  
- Core layer does not route DMZ traffic directly  
- Firewalls serve as the authoritative boundary between trust levels  

### **Layered Enterprise Architecture**
- **Collapsed Core Layer** for routing, SVIs, and control-plane logic  
- **Firewall Layer** for NAT, WAN access, and DMZ enforcement  
- **Access Layer** for user and server endpoints  

### **VLAN and SVI Hierarchy**
- VLAN 100 → Infrastructure (DHCP)  
- VLAN 333 → Control-plane redundancy  
- VLAN 400 → DMZ  
- VLAN 700/701 → User networks  
- VLAN 800 → Voice  
- VLAN 900 → Servers  

Clean hierarchy ensures predictable routing and subnet management.

### **L2 Redundancy**
💡 Dual uplinks from access switches preserve connectivity during individual link failures.

### **Firewall-Routed DMZ**
⚠️ DMZ traffic is never routed internally, enforcing proper east–west and north–south separation.

---

## <span style="color:#2b6cb0">Technologies Used</span>

- Cisco IOS Layer 2 / Layer 3 switching  
- VRRP – Virtual Router Redundancy Protocol  
- NAT – inside → outside translation  
- VLAN segmentation and SVI-based routing  
- Dual-firewall architecture  
- Spanning Tree–aware access design  
- Diagrams.net for architectural visualization  

---

## <span style="color:#2b6cb0">Network Architecture Overview</span>

<p align="center">
  <img width="1020" height="690" alt="image" src="https://github.com/user-attachments/assets/7d31ae57-ad42-4f69-9fa4-b2260d013e38" />
</p>

The diagram reflects all routing, switching, firewall, VLAN, and redundancy relationships in a single consolidated view.

---

## <span style="color:#2b6cb0">Layered Design Breakdown</span>

---

### <span style="color:#2b6cb0">DMZ Network</span>

- VLAN 400 hosts semi-public systems  
- Routed only via external firewalls  
- Prevents direct internal access  
- Mirrors production-grade DMZ segmentation practices  

---

### <span style="color:#2b6cb0">Core Layer</span>

- Two L3 switches sharing a VRRP virtual IP  
- Hosts SVIs for all internal VLANs  
- Acts as the default gateway for the enterprise  
- Performs all east–west routing  
- Dedicated redundancy VLAN (333) for heartbeat and failover messaging  

---

### <span style="color:#2b6cb0">Firewall Layer</span>

- Dual firewalls provide WAN access and security enforcement  
- NAT (inside → outside) configured  
- External routing entirely handled at the firewall layer  
- DMZ ingress/egress controlled via firewall rules  
- Reflects real security boundary architecture  

---

### <span style="color:#2b6cb0">Access Layer</span>

- Workstations, servers, and voice devices connect here  
- VLANs 700, 701, 800, and 900 terminate at core SVIs  
- Dual-homed L2 links prevent single-point access failures  

---

### <span style="color:#2b6cb0">Redundancy & High Availability</span>

- VRRP provides a unified gateway regardless of which core switch is active  
- Access switches maintain L2 adjacency to both core switches  
- VRRP + dual uplinks provide stable convergence and predictable failover behavior  

---

## <span style="color:#2b6cb0">Routing Configuration Summary</span>

### **Core Switch Routing**
```
VLAN 700 Gateway  → 172.16.70.254
VLAN 701 Gateway  → 172.16.71.254
VLAN 800 Gateway  → 172.16.80.254
VLAN 900 Gateway  → 172.16.90.254
VLAN 100 Gateway  → 10.10.10.1
VLAN 333 Gateway  → 10.100.100.1  (VRRP Virtual IP)
```

### **Firewall Routing**
```
NAT inside → outside: dynamic PAT
Default Route: 0.0.0.0/0 → External Firewall
Route inside: 172.16.0.0/16 → 10.100.100.2
Route outside: WAN → ISP Gateway
```

---

## <span style="color:#2b6cb0">Engineering Rationale</span>

### **Why a Collapsed Core?**
- Reduces complexity while maintaining strong routing capabilities  
- Common in organizations with < 500 endpoints  
- Supports growth without immediate architectural redesign  

### **Why VRRP?**
- Vendor-neutral redundancy protocol  
- Simple configuration and troubleshooting  
- Predictable failover behavior  

### **Why Force DMZ Traffic Through Firewalls?**
- Enforces proper north–south and east–west controls  
- Prevents lateral movement into internal networks  
- Aligns with industry security best practices  

### **Why Dual-Homed Access Switches?**
- Increases availability  
- Reduces dependence on a single uplink or core device  
- Works smoothly with spanning-tree designs  

---

## <span style="color:#2b6cb0">Skills Demonstrated</span>

- Layer 2/Layer 3 enterprise network design  
- VLAN hierarchy and addressing strategy  
- Redundancy planning (VRRP, dual uplinks)  
- Firewall boundary architecture  
- DMZ isolation and segmentation  
- NAT and routing control  
- Collapsed core campus design  
- Professional diagramming and documentation  

---

## <span style="color:#2b6cb0">Key Takeaways</span>

- This topology demonstrates real-world network engineering concepts beyond entry-level labs.  
- The architecture prioritizes **availability**, **security zoning**, and **predictable traffic flow**.  
- VRRP, firewall boundaries, and redundant uplinks model true enterprise resiliency patterns.  
- The modular design allows future expansion into routing protocols, VPNs, QoS, or multi-site designs.

---

