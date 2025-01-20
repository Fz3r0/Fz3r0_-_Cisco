# 🔥🧱🛡️ Cisco Nexus: `VXLAN`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)


### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Datacenter` `Cisco Nexus` `NX-OS`

---

<br>

# 📝❓📄 `Index`

- 

# VXLAN (Virtual Extensible LAN)

VXLAN (Virtual Extensible LAN) is a network virtualization technology that enables the creation of Layer 2 overlays on top of Layer 3 networks. It was designed to address limitations of traditional VLAN-based networks, particularly in large-scale data center environments and Software-Defined Networking (SDN) solutions. This document provides an in-depth analysis of VXLAN, its architecture, key components, and practical use cases with a focus on Cisco implementations.

VXLAN represents a transformative shift from traditional VLAN-based networking to a scalable, efficient, and flexible architecture. By encapsulating Layer 2 traffic into Layer 3 tunnels, VXLAN enables enterprises to build robust and scalable networks suitable for modern data centers, cloud environments, and SDN deployments. Cisco’s robust portfolio of VXLAN-enabled solutions, such as the Nexus 9000 series and Cisco DNA Center, ensures seamless integration and operational simplicity.

## Traditional VLAN Architecture: A Foundation for VXLAN

Traditional VLANs allow segmentation of Layer 2 networks, enabling devices in the same VLAN to communicate directly while isolating devices in different VLANs. Inter-VLAN communication is facilitated via Layer 3 routing. However, VLANs are constrained by scalability limitations (12-bit VLAN ID supports only 4,096 VLANs) and are prone to inefficiencies like Spanning Tree Protocol (STP) issues and broadcast traffic overhead.

### Workflow Example

Consider a scenario where two PCs (PC-A and PC-B) reside in VLAN 10 and communicate through a Layer 2 network. The switches connecting the PCs use access ports for direct PC connections and trunk ports for inter-switch communication. MAC address learning occurs dynamically, and ARP broadcasts are used to discover destination MAC addresses.

![image](https://github.com/user-attachments/assets/6ca80d48-648c-4084-947b-5479b96325a5)

**Key Characteristics of Traditional VLANs:**

- VLAN ID: 12 bits (4,096 VLANs maximum).
- Requires Layer 2 connectivity (trunks) between switches.
- Uses STP to prevent loops, potentially blocking redundant links.


## VXLAN Architecture: Extending Layer 2 Across Layer 3

VXLAN (RFC 7348) is an overlay technology that encapsulates Layer 2 Ethernet frames into UDP packets for transport across an IP-based network. 

It was originally developed to address the need for seamless Layer 2 connectivity between geographically distributed data centers. However, VXLAN has evolved into a versatile solution applicable to data centers, service provider networks, and enterprise SDN deployments.

### Key Features of VXLAN:

| Feature                       | Description                                                                                  |
|-------------------------------|----------------------------------------------------------------------------------------------|
| **Encapsulation**             | Uses UDP encapsulation with a VXLAN header (port 4789). Adds 54 bytes to the packet size.    |
| **Scalability**               | Supports up to 16,777,216 VXLAN Network Identifiers (VNIs) using a 24-bit space.             |
| **Transport Requirements**    | Operates over Layer 3 (IP network). No reliance on Layer 2 (trunks, STP).                   |
| **Multitenancy**              | Each VXLAN segment is isolated (broadcast domain), enabling tenant separation.               |


## VXLAN Components

1. **VXLAN Network Identifier (VNI):**
   - Analogous to VLAN IDs but with a 24-bit space (16,777,216 VNIs).
   - Each VNI represents a broadcast domain.

2. **VXLAN Tunnel Endpoint (VTEP):**
   - A device or software entity that encapsulates and decapsulates VXLAN traffic.
   - Responsible for forwarding VXLAN traffic over the IP network using tunnels.

3. **Virtual Tunnel Interface (VTI):**
   - Logical interface used by VTEPs for VXLAN communication.

4. **VXLAN Gateway:**
   - Device designated to bridge VXLAN and non-VXLAN networks (e.g., traditional VLANs).

5. **Multicast/BGP EVPN:**
   - For MAC address learning and broadcast traffic replication:
     - Multicast: Uses multicast groups for replication.
     - BGP EVPN: A control-plane mechanism for MAC/IP learning and forwarding.


## VXLAN vs. Traditional VLANs: A Comparison

| **Feature**                 | **Traditional VLANs**                   | **VXLAN**                                    |
|-----------------------------|------------------------------------------|----------------------------------------------|
| **Scalability**             | Limited to 4,096 VLANs                  | Supports 16 million VNIs                     |
| **Control Plane**           | Layer 2 broadcast-based MAC learning    | Centralized control (e.g., BGP EVPN)         |
| **Transport Network**       | Layer 2 (requires trunks)               | Layer 3 (IP network)                         |
| **Redundancy**              | STP for loop prevention                 | Routing protocols (OSPF, IS-IS, etc.)        |
| **Overhead**                | Minimal                                | Adds 54-byte VXLAN header                    |

---

### VXLAN Workflow: Communication Across Layer 3

In VXLAN, devices in the same VNI communicate as if they are in the same Layer 2 domain, but the underlying transport is Layer 3. Below is a simplified example of VXLAN communication:

1. **VTEP Setup:**
   - VTEPs are configured on the edge devices (e.g., switches or routers) where VXLAN traffic is initiated/terminated.
   - Each VTEP is assigned a unique IP address for tunnel identification.

2. **Traffic Encapsulation:**
   - When PC-A sends a frame to PC-B, the local VTEP encapsulates the Ethernet frame with a VXLAN header, UDP header, and outer IP header.

3. **Tunnel Transport:**
   - The encapsulated packet is routed through the Layer 3 network to the destination VTEP.

4. **Traffic Decapsulation:**
   - The destination VTEP removes the VXLAN header and forwards the original Ethernet frame to PC-B.

## Example: PC-A in VNI 700 Communicating with PC-B

- VNI 700 is configured on both VTEPs.
- PC-A generates an ARP request for PC-B.
- The local VTEP learns PC-A's MAC and encapsulates the ARP request.
- The destination VTEP decapsulates the ARP request and forwards it to PC-B.

![image](https://github.com/user-attachments/assets/dc08c7a8-b000-4dfa-a5b5-3bd25fc54949)


## Benefits of VXLAN in Modern Networks

1. **Layer 2 Extension Over Layer 3:**
   - Enables seamless communication between devices in different geographical locations.

2. **Elimination of STP:**
   - No blocked ports or inefficiencies caused by STP. Redundancy is handled via routing protocols.

3. **Enhanced Scalability:**
   - Supports millions of VXLAN segments compared to the 4,096 VLAN limitation.

4. **Multitenancy:**
   - Perfect for environments with multiple tenants, as each VNI is isolated.


## VXLAN and SDN: The Cisco Perspective

VXLAN forms the foundation of many SDN solutions, including Cisco’s SD-Access and SD-WAN. For example:

- **Cisco SD-Access:**
  - Uses Cisco DNA Center as a centralized controller.
  - Automates network configuration and management.
  - Leverages VXLAN for Layer 2 overlays and VRF segmentation.

- **Cisco Nexus 9000 Series:**
  - Fully supports VXLAN.
  - Designed for data center environments with high performance and scalability.

- **Licensing:**
  - Features like VXLAN require advanced licenses (e.g., Cisco DNA Advantage).





















































VXLAN significado: 



![image](https://github.com/user-attachments/assets/3b5dab0a-1ea5-4d30-b990-82a35931aedd)

![image](https://github.com/user-attachments/assets/75f830c2-1e1f-47c5-9b6d-42ceb4077e31)

---

![image](https://github.com/user-attachments/assets/f2a90c1a-cea4-43e5-8bbe-89ac96b06b88)


---

![image](https://github.com/user-attachments/assets/27a6b1df-8259-4807-b3a0-7d97d70086c0)



# 📚🗂️🎥 Resources

- https://www.youtube.com/live/uW--KhRzdEk?si=ujMneJdPkihxBtT4
- [Cisco DNA Center](https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2023/pdf/BRKOPS-2077.pdf)



  
---

<span align="center"> <p align="center"> ![giphy](https://user-images.githubusercontent.com/94720207/166587250-292d9a9f-e590-4c25-a678-d457e2268e85.gif) </p> </span> 



&nbsp;

<span align="center"> <p align="center"> _I hope this information was useful for someone_ </p> </span> 
<span align="center"> <p align="center"> _and please, don't forget to enjoy your days..._ </p> </span> 
<span align="center"> <p align="center"> _...It is getting dark... so dark..._ </p> </span> 

&nbsp;

<span align="center"> <p align="center"> _In the mist of the night you could see me come, where shadows move and Demons lie..._ </p> </span> 
<span align="center"> <p align="center"> _I am [Fz3r0 💀](https://github.com/Fz3r0/) and the Sun no longer rises..._ </p> </span> 

---






---

> ![hecho en mexico 5](https://user-images.githubusercontent.com/94720207/166068790-fa1f243d-2db9-4810-a6e4-eb3c4ad23700.png)
>
> _- Hecho en México - by [Fz3r0 💀](https://github.com/Fz3r0/)_  
>
> _"In the mist of the night you could see me come, where shadows move and Demons lie..."_ 

