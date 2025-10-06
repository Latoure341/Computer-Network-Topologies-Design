# Computer-Network-Topologies-Design
This project demonstrates the design and configuration of multiple network topologies — Bus, Star, Ring, Mesh, and Extended Star — combined into a unified Hybrid Network Model using Cisco Packet Tracer. The hybrid setup integrates VLAN segmentation, DHCP for dynamic IP allocation, DNS for domain resolution, and both IPv4 and IPv6 addressing.

## Topologies Included
| Topology Type | File Name | Description |
|----------------|------------|--------------|
| Bus Topology | `BusTopology.pkt` | Linear connection with a shared communication medium. |
| Star Topology | `StarTopology.pkt` | Centralized topology using a single switch as a hub. |
| Ring Topology | `RingTopology.pkt` | Nodes connected in a loop for redundancy. |
| Mesh Topology | `Mesh Topology.pkt` | Fully connected devices for high fault tolerance. |
| Extended Star | `ExtendedStarTopology.pkt` | Multiple stars linked together for scalability. |
| Hybrid Model | `HybridModel.pkt` | Combination of all the above — includes VLANs, DHCP, and routing. |

## Hybrid Network Design
The **HybridModel.pkt** integrates different topologies for better scalability and performance.  
It uses VLANs for segmentation, DHCP for automatic IP assignment, and DNS for name resolution.

## Network Diagram

---
## IP Addressing Scheme

### IPv4 Addressing Table
| Device | Interface | IP Address | Subnet Mask | VLAN |
|---------|------------|-------------|--------------|------|
| Router | G0/0 | 192.168.1.1 | 255.255.255.0 | - |
| Router | G0/1 | 192.168.10.1 | 255.255.255.0 | 10 (Admin) |
| DHCP/DNS Server | NIC0 | 192.168.1.2 | 255.255.255.0 | - |
| PC1 | NIC0 | DHCP | - | 10 |
| PC2 | NIC0 | DHCP | - | 10 |

### IPv6 Addressing Example
| Device | Interface | IPv6 Address | Prefix |
|---------|------------|---------------|---------|
| Router | G0/0 | 2001:DB8:1::1 | /64 |
| Router | G0/1 | 2001:DB8:2::1 | /64 |
| PC1 | NIC0 | Auto (SLAAC) | /64 |

## ⚙️ Router Configuration
```plaintext
Router> enable
Router# configure terminal
Router(config)# hostname CoreRouter
Router(config)# interface g0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config)# interface g0/1
Router(config-if)# encapsulation dot1Q 10
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config)# ipv6 unicast-routing
Router(config)# ipv6 address 2001:DB8:1::1/64
Router(config)# ipv6 dhcp relay destination 2001:DB8:1::2
Router(config)# end
Router# write memory
```

## Switch Configuration (VLAN & Trunk)

```plaintext
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name Admin
Switch(config-vlan)# exit
Switch(config)# interface range f0/1 - 10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit
Switch(config)# interface g0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10
Switch(config-if)# exit
Switch# write memory
```

## DHCP Server Configuration

| Setting | Value |
|----------|--------|
| DHCP Pool Name | VLAN10_POOL |
| Network | 192.168.10.0 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.10.1 |
| DNS Server | 192.168.1.2 |
| IP Range | 192.168.10.10 – 192.168.10.50 |

**Example CLI (if configured on router):**
```plaintext
Router(config)# ip dhcp pool VLAN10_POOL
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 192.168.1.2
Router(dhcp-config)# exit
```

## DNS Server Setup
| Setting | Example |
|----------|----------|
| Domain Name | admin.local |
| Host Record | PC1 → 192.168.10.10 |
| DNS Server IP | 192.168.1.2 |
---

## 🧪 Testing and Verification

| Test | Command | Expected Result |
|------|----------|-----------------|
| PC to Gateway | `ping 192.168.10.1` | Successful |
| PC to Server | `ping 192.168.1.2` | Successful |
| Inter-VLAN Routing | `ping 192.168.1.x` | Successful |
| DNS Lookup | `nslookup admin.local` | Resolves IP |

---

## ⚠️ Challenges & Fixes
| Challenge | Cause | Fix |
|------------|--------|-----|
| Overlapping subnets | Duplicate network on router | Assign unique /24 networks |
| Trunk link down | VLAN mismatch | Ensure trunk allows VLAN 10 |
| DHCP failure | Wrong default gateway | Correct DHCP default router address |


## ✅ Conclusion
This project successfully demonstrated how different topologies (Bus, Star, Ring, Mesh, Extended Star) can be integrated into a single **Hybrid Network** using Cisco Packet Tracer.  
It implemented VLAN segmentation, DHCP automation, DNS resolution, and IPv6 addressing — ensuring scalability, manageability, and redundancy.

---
## 📦 Repository Structure
```plaintext
NetworkProject/
│
├── BusTopology.pkt
├── StarTopology.pkt
├── RingTopology.pkt
├── Mesh Topology.pkt
├── ExtendedStarTopology.pkt
├── HybridModel.pkt
└── README.md
```
