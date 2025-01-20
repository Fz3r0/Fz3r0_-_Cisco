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


# VXLAN and SDN: The Cisco Perspective

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

## VXLAN: Device Requirements and Cisco Support

To deploy VXLAN in a network, the primary requirement is having devices that support VXLAN capabilities. VXLAN introduces specific hardware and software requirements due to its reliance on encapsulation, tunneling, and advanced routing functionalities. Cisco offers a wide range of devices that fully support VXLAN, with the **Cisco Nexus 9000 series** being a cornerstone for VXLAN deployments.

Deploying VXLAN begins with selecting the right hardware. Cisco’s Nexus 9000 series and other VXLAN-capable devices provide the foundation for creating scalable, flexible, and efficient network overlays. These devices ensure seamless integration with modern SDN platforms like Cisco DNA Center and ACI, making them the ideal choice for enterprise and data center environments. By meeting hardware and licensing requirements, organi

## Hardware Requirements for VXLAN

Not all network devices can support VXLAN due to the advanced processing required for encapsulation and decapsulation of VXLAN traffic, as well as the necessity for enhanced routing and tunneling capabilities. Here are the key requirements:

### Essential Features:
1. **VXLAN Support**:
   - Devices must support VXLAN encapsulation (UDP-based) and decapsulation to enable communication across Layer 3 networks.
2. **VTEP (VXLAN Tunnel Endpoint)**:
   - Hardware or software support for VTEP functionality is required. The device should be capable of creating and managing VXLAN tunnels for traffic encapsulation and decapsulation.
3. **Routing Protocols**:
   - Devices must support dynamic routing protocols such as OSPF, IS-IS, or BGP to enable Layer 3 connectivity for VXLAN overlays.
4. **Increased MTU Support**:
   - VXLAN adds 54 bytes of overhead to each packet. Devices must support a higher MTU (e.g., 1600 bytes or higher) to handle VXLAN-encapsulated traffic without fragmentation.
5. **Advanced Control Plane**:
   - Devices must support protocols like BGP EVPN or multicast for MAC address learning and VXLAN control plane operations.
6. **High-Performance Forwarding**:
   - Since VXLAN is designed for large-scale environments, devices must offer high-speed forwarding and processing to handle encapsulated traffic efficiently.

---

## Cisco Devices That Support VXLAN

Cisco offers a broad portfolio of VXLAN-enabled devices, with the **Cisco Nexus 9000 series** being the flagship solution for VXLAN in modern data centers. These devices are specifically designed to handle VXLAN traffic, provide scalability, and integrate seamlessly with SDN solutions like Cisco DNA Center and Cisco ACI.

### Cisco Nexus 9000 Series

The Nexus 9000 series is the gold standard for VXLAN deployments. These switches are optimized for data centers and SDN environments, offering high performance, scalability, and rich feature sets for VXLAN overlays.

| **Feature**                        | **Nexus 9000 Series**                                                                                 |
|------------------------------------|-------------------------------------------------------------------------------------------------------|
| **VXLAN Support**                  | Fully supports VXLAN encapsulation, decapsulation, and tunneling.                                     |
| **VXLAN EVPN (Control Plane)**     | Integrates with BGP EVPN for advanced VXLAN control plane operations.                                 |
| **Programmability**                | Supports Cisco NX-OS and ACI mode for flexible VXLAN configurations.                                 |
| **Automation**                     | Compatible with Cisco DNA Center for centralized automation and management.                          |
| **MTU Support**                    | Configurable for increased MTU to handle VXLAN encapsulated traffic.                                 |
| **Scalability**                    | Designed for large-scale environments, supporting millions of VXLANs (VNIs) and high-density ports.   |

### Cisco Catalyst 9000 Series
While primarily designed for enterprise access and distribution layers, the Catalyst 9000 series also supports VXLAN in certain models, enabling VXLAN-based segmentation in campus networks.

| **Feature**                        | **Catalyst 9000 Series**                                                                              |
|------------------------------------|-------------------------------------------------------------------------------------------------------|
| **Use Case**                       | Best suited for enterprise campus networks and branch connectivity.                                   |
| **VXLAN Support**                  | Supported in advanced models with specific licenses (e.g., DNA Advantage).                           |
| **Controller Integration**         | Fully integrated with Cisco DNA Center for SD-Access deployments.                                    |

---

## Licensing and Software Requirements

VXLAN functionality is often tied to specific software licenses, particularly in Cisco devices. Advanced features like BGP EVPN, SDN integration, and VXLAN tunneling require higher-tier licenses.

| **License Tier**       | **Features**                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------|
| **Essentials**         | Basic Layer 2/3 functionality. VXLAN is not supported in this tier.                           |
| **Advantage**          | Enables advanced routing (OSPF, BGP), VXLAN support, and SDN integration with Cisco DNA Center.|
| **Premier/ACI Mode**   | Provides full capabilities for VXLAN-based data center fabric automation in ACI environments.  |

---

## Benefits of Using Cisco VXLAN-Ready Devices

1. **Scalability**:
   - Cisco devices like the Nexus 9000 series are purpose-built for large-scale VXLAN environments, supporting up to 16 million VNIs.

2. **Centralized Management**:
   - Integration with Cisco DNA Center and Cisco ACI ensures that VXLAN-enabled devices can be centrally managed, reducing operational complexity.

3. **Interoperability**:
   - Cisco VXLAN-enabled devices are compatible with other VXLAN-capable platforms, ensuring smooth interoperability in multi-vendor environments.

4. **Future-Ready**:
   - Cisco's focus on SDN and automation ensures that VXLAN-enabled devices are ready for evolving networking requirements.











# VXLAN Multicast Flood and Learn

VXLAN Multicast Flood and Learn is one of the key mechanisms for achieving MAC address learning and broadcast replication in VXLAN environments. It is a data plane learning approach that relies on multicast groups to replicate broadcast, unknown unicast, and multicast (BUM) traffic across the VXLAN network. 

VXLAN Multicast Flood and Learn is a straightforward mechanism for enabling VXLAN overlays in multicast-enabled networks. While it offers simplicity and compatibility with existing underlay multicast setups, its reliance on data plane learning and multicast replication can present scalability and efficiency challenges. As networks grow in size and complexity, control-plane solutions like BGP EVPN are increasingly preferred for VXLAN deployments due to their enhanced scalability and operational efficiency.

This section dives deep into the concepts, workflow, and considerations of using VXLAN Multicast Flood and Learn.

## Overview of VXLAN Flood and Learn

- In traditional Layer 2 networks, MAC address learning occurs via broadcast frames and flooding. 
- VXLAN, operating over a Layer 3 network, uses a similar mechanism for MAC learning in its Flood and Learn mode.
- **Instead of relying on Layer 2 broadcast, VXLAN uses IP multicast to replicate BUM traffic between VXLAN Tunnel Endpoints (VTEPs).**

![image](https://github.com/user-attachments/assets/b1c6ef7e-e749-4d4c-bd06-c2bf043b14fa)

### How It Works

1. **Flooding Traffic Across VTEPs**:
   - When a VTEP receives BUM traffic (e.g., an ARP request or unknown unicast frame), it encapsulates the frame with a VXLAN header and sends it to a multicast group.
   - The multicast group is associated with the VXLAN Network Identifier (VNI) for the broadcast domain.

2. **Multicast Groups**:
   - Each VNI is mapped to a unique multicast group address in the underlay IP network.
   - Devices (VTEPs) interested in a specific VNI subscribe to the corresponding multicast group.

3. **MAC Learning**:
   - As VTEPs receive encapsulated traffic from other VTEPs, they learn the source MAC addresses and the associated VTEP IP address from the VXLAN header.
   - This builds the MAC-to-VTEP mappings in the VXLAN forwarding table.

4. **Traffic Optimization**:
   - Once the MAC address is learned, unicast traffic to that MAC is sent directly to the corresponding VTEP, bypassing multicast replication.



## VXLAN Multicast Flood and Learn Workflow

### Step-by-Step Process:

1. **Initial Frame Transmission**:
   - A host (e.g., PC-A) sends a frame (e.g., ARP request) to another host (e.g., PC-B) in the same VXLAN segment (VNI).
   - The local VTEP encapsulates the frame with a VXLAN header, including the VNI, and forwards it to the multicast group associated with that VNI.

2. **Multicast Replication**:
   - The multicast-enabled underlay network replicates the packet to all VTEPs subscribed to the multicast group.
   - Only VTEPs associated with the VNI will receive the traffic.

3. **MAC Learning at Destination VTEPs**:
   - Destination VTEPs decapsulate the frame and forward it to the local hosts.
   - The source MAC address of the original frame is learned and associated with the originating VTEP.

4. **Unicast Optimization**:
   - Once the destination MAC address is learned, future traffic to that MAC address is sent directly to the corresponding VTEP using unicast, eliminating the need for multicast replication.


## VXLAN Multicast Flood and Learn Topology

Here’s an example to illustrate the process:

1. **Hosts in the Same VNI**:
   - PC-A and PC-B are part of the same VXLAN segment (e.g., VNI 700) but are connected to different VTEPs (Switch A and Switch B).
   - VNI 700 is mapped to multicast group `239.1.1.1` in the underlay network.

2. **Traffic Flow**:
   - PC-A sends an ARP request to PC-B.
   - Switch A (VTEP-A) encapsulates the ARP request into a VXLAN packet with a VNI 700 header and forwards it to the multicast group `239.1.1.1`.
   - The underlay network replicates the packet to all VTEPs subscribed to `239.1.1.1` (e.g., VTEP-B).
   - VTEP-B decapsulates the packet and forwards the ARP request to PC-B.
   - VTEP-B learns PC-A’s MAC address and associates it with VTEP-A.

3. **Subsequent Traffic**:
   - After the initial broadcast, any unicast traffic from PC-B to PC-A is sent directly to VTEP-A using the learned MAC-to-VTEP mapping.


## Key Considerations for Multicast Flood and Learn

1. **Multicast-Enabled Underlay**:
   - The underlay network must support IP multicast (PIM-SM or PIM-SSM) to handle traffic replication efficiently.
   - Each VNI requires a unique multicast group, increasing multicast group management overhead.

2. **Scalability Challenges**:
   - As the number of VNIs increases, the number of multicast groups grows, potentially leading to scalability issues in large-scale networks.

3. **Inefficiency of Flood and Learn**:
   - Multicast Flood and Learn relies heavily on the data plane, which may lead to increased traffic in the underlay network.
   - This is why BGP EVPN (control-plane learning) is often preferred for large deployments.

4. **Broadcast Traffic**:
   - Broadcast-intensive applications (e.g., ARP-heavy workloads) can cause excessive replication in the underlay, impacting network performance.


## VXLAN Multicast Flood and Learn vs. BGP EVPN

| **Feature**                      | **Multicast Flood and Learn**                              | **BGP EVPN**                                     |
|----------------------------------|-----------------------------------------------------------|-------------------------------------------------|
| **MAC Learning**                 | Data-plane learning (via multicast).                      | Control-plane learning (via BGP).               |
| **Traffic Replication**          | Uses IP multicast for BUM traffic.                        | No multicast required; uses unicast BGP updates.|
| **Scalability**                  | Limited by multicast group management in the underlay.    | Highly scalable; eliminates the need for multicast. |
| **Deployment Complexity**        | Requires multicast configuration in the underlay network. | Requires BGP EVPN configuration on VTEPs.       |
| **Efficiency**                   | Inefficient in broadcast-heavy environments.              | More efficient with reduced BUM traffic.        |


## Use Cases for VXLAN Multicast Flood and Learn

1. **Small-Scale VXLAN Deployments**:
   - Suitable for environments where multicast is already enabled in the underlay and the number of VNIs is manageable.

2. **Backward Compatibility**:
   - Useful in scenarios where a legacy multicast-enabled underlay exists, and BGP EVPN is not yet supported or implemented.

3. **Proof-of-Concept Deployments**:
   - Ideal for testing VXLAN functionality in lab environments before moving to production with BGP EVPN.

---
































































![image](https://github.com/user-attachments/assets/3b5dab0a-1ea5-4d30-b990-82a35931aedd)

![image](https://github.com/user-attachments/assets/75f830c2-1e1f-47c5-9b6d-42ceb4077e31)

---

![image](https://github.com/user-attachments/assets/f2a90c1a-cea4-43e5-8bbe-89ac96b06b88)


---

![image](https://github.com/user-attachments/assets/27a6b1df-8259-4807-b3a0-7d97d70086c0)


























# 📚🗂️🎥 Resources

- https://www.youtube.com/live/uW--KhRzdEk?si=ujMneJdPkihxBtT4
- [Cisco DNA Center](https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2023/pdf/BRKOPS-2077.pdf)
- https://www.linkedin.com/pulse/vxlan-via-flood-learn-multicast-gedion-hulluka/
- [How To's Deploy VXLAN - Flood & Learn](https://www.youtube.com/watch?v=v-2YaYWLnuo)




  
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

