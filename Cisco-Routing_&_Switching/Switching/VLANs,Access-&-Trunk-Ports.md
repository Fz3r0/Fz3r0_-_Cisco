# 🌐 **VLAN (Virtual Local Area Network)**  

A **VLAN (Virtual Local Area Network)** allows the creation of **independent logical networks** within a **single physical network**. This improves network **segmentation, security, and efficiency** by reducing **broadcast domains** and logically separating departments, devices, or services.  

Multiple VLANs can exist on a **single switch** or across multiple interconnected switches using VLAN **trunking**. VLANs operate primarily at **Layer 2 (Data Link Layer)** but can also integrate with **Layer 3 (Network Layer) routing** for inter-VLAN communication.

## 🔮 VLAN: `Cheatsheet`

![image](https://github.com/user-attachments/assets/7865cd91-38bf-4b0c-b5bd-f086a42bda71)

## 📜 **Why VLANs?**  

1. **Reduces `Broadcast Domains`** 🏢🔻  
   - VLANs prevent unnecessary traffic from reaching all devices in a network.  

2. **Improves Security** 🔐  
   - Devices in separate VLANs cannot communicate unless explicitly configured via **routing**.  

3. **Enhances Performance** 🚀  
   - Less congestion and optimized traffic flow improve network efficiency.  

4. **Flexibility & Scalability** 🔄  
   - VLANs allow **logical segmentation** of users/devices, independent of physical location.  

## 🌐 VLAN: `Broadcast Domain vs Collision Domain`

In networking, **broadcast domains** and **collision domains** define how devices interact within a network and how data is transmitted efficiently.

![image](https://github.com/user-attachments/assets/8473b914-0fbf-4c0f-8862-532cc86a07fd)

- **Collision Domains** exist at **Layer 1** and are eliminated by **switches**.  
- **Broadcast Domains** exist at **Layer 2** and are reduced using **routers or VLANs**.  
- **Switches** improve network efficiency by **eliminating collisions** but do **not break broadcast domains** unless VLANs are used.  
- **Routers** segment both **collision and broadcast domains**, improving overall network performance.  
 
### 📜 Broadcast Domain vs Collision Domain: `Key Differences`

| **Feature**              | **Broadcast Domain** 🌍 | **Collision Domain** ⚡ |
|--------------------------|-----------------------|----------------------|
| **Definition**           | A group of devices that receive broadcast traffic from any device in the domain. | A group of devices that share the same transmission medium and can experience collisions. |
| **Layer**               | Layer 2 (Data Link) | Layer 1 (Physical) |
| **Impact on Traffic**   | Increases network congestion if too many devices are in the same domain. | Reduces network efficiency due to collisions in shared media. |
| **Devices Affected**    | All devices in the VLAN or subnet. | Only devices within the same physical segment (e.g., hub or repeater). |
| **Separation Method**   | VLANs or routers segment broadcast domains. | Switches and bridges segment collision domains. |
| **Network Devices That Extend** | Switches (if VLANs are not configured), Hubs, and Bridges. | Hubs and Repeaters. |
| **Network Devices That Break** | Routers and Layer 3 Switches. | Switches and Bridges. |

### 🔗 **Broadcast & Collision Domains in Network Devices**  

| **Device** 🖥️ | **Broadcast Domains** 🌍 | **Collision Domains** ⚡ |
|--------------|----------------------|----------------------|
| 🔄 **Hub**  | 1 (All devices share the same broadcast domain) | 1 (All ports share the same collision domain) |
| 🌉 **Bridge**  | 1 per segment | 1 per port |
| 🏢 **Switch (No VLANs)**  | 1 (All ports in the same VLAN are in the same broadcast domain) | 1 per port (Each port is its own collision domain) |
| 🌐 **Switch (With VLANs)**  | 1 per VLAN | 1 per port |
| 🌍 **Router**  | 1 per interface | 1 per interface |


## 🏗 **Types of VLANs**  

VLANs can be classified based on how **devices are assigned** to them:

| **VLAN Type**          | **Layer** | **Description** | **Usage Frequency** 📊 | **Real-World Example** 🌍 |
|------------------------|----------|----------------|-----------------------|--------------------------|
| **Port-based VLAN** 🔌 | L1 | Each switch **port** is assigned to a VLAN. Users must be **manually reassigned** if they change ports. | ⭐⭐⭐⭐⭐ (Most Common) | Used in most enterprise networks to segment departments (e.g., HR VLAN 10 on interfaces Gi 1-10, IT VLAN 20interfaces Gi 11-20). |
| **MAC-based VLAN** 📡 | L2 | VLAN membership is based on **device MAC addresses**, allowing mobility across ports without reconfiguration. | ⭐⭐ (Less Common) | Ideal for dynamic environments like hotels or coworking spaces where users frequently change physical locations. |
| **Protocol-based VLAN** 📜 | L3 | VLANs are assigned based on **protocol type** (e.g., IPv4, IPv6, AppleTalk, IPX). | ⭐ (Rare) | Used in legacy or multi-protocol networks where different protocols (e.g., IPv4 and IPX) need separate VLANs. |
| **Subnet-based VLAN** 🌎 | L3 | Uses **IP subnet addresses** to determine VLAN membership. | ⭐⭐⭐ (Common) | Often used in large enterprises where VLANs are structured based on departments or locations (e.g., VLAN 10 for **192.168.1.0/24**, VLAN 20 for **192.168.2.0/24**). |
| **Application-based VLAN** 🎭 | L4+ | VLANs are assigned based on **application type** (e.g., Voice VLAN (Cisco Phone), FTP, VoIP, streaming). | ⭐⭐⭐ (Common) | Voice VLAN in Cisco is a type of Application-based VLAN because it assigns VLAN membership based on traffic type (VoIP) rather than ports, MAC addresses, or subnets. Used in QoS-sensitive environments like call centers, where VoIP traffic is separated from regular data traffic. |


## 🔗 **VLAN Communication: Access & Trunk Ports**  

| **Port Type**       | **Function** |
|---------------------|-------------|
| **Access Port** 🎯 | Belongs to a **single VLAN**. Connects devices like PCs and printers. |
| **Trunk Port** 🌉 | Carries **multiple VLANs** over a single link. Uses **802.1Q tagging** to distinguish VLAN traffic. |

### **Trunking Protocols:**  

- **IEEE 802.1Q** (Industry Standard)  
- **ISL (Inter-Switch Link)** (Cisco Proprietary)  


## 📡 **VLAN Management & Protocols**  

Several protocols facilitate VLAN operation and scalability:

| **Protocol** | **Function** |
|-------------|-------------|
| **IEEE 802.1Q** 🏷️ | Adds a **VLAN tag** to Ethernet frames for identification. |
| **VTP (VLAN Trunking Protocol)** 🔄 | Cisco protocol that **synchronizes VLAN configurations** across multiple switches. |
| **STP (Spanning Tree Protocol)** 🌳 | Prevents **loops** in a network with redundant links. |
| **RSTP (Rapid STP)** ⚡ | Faster convergence version of STP. |
| **MSTP (Multiple STP)** 🛠 | Supports multiple VLANs with separate **spanning trees**. |

---



















---

super huint de la manda: `show vlan internal usage` hay switches viejso que usan una vlan para algo internamente como el catañlyst 4503



---




# 🚀 VLAN Implementation  

This section covers **basic VLAN configurations** for different network scenarios, starting from a simple **PC-to-PC** connection to a **VLAN-enabled switch** with segmentation. Each scenario is described with **real-world applications** and configuration examples.  

## **🔗 NO VLAN used: `Peer-to-Peer (PC to PC Direct Connection)`**  

![image](https://github.com/user-attachments/assets/5ea9c2af-2298-4188-b675-c5e58ef9746c)

### 📝 Peer-to-Peer: **`How It Works`**  

- Two PCs are **directly connected** using an Ethernet cable.  
- In the past, **crossover cables** were required, but modern NICs support **Auto MDI-X**, allowing **straight-through cables** to work as well.  
- **No switch or hub is needed**—just assign IPs in the same subnet.  
- No **gateway or VLANs** are required since this is a **Layer 2** connection.  

### 🌍 **Real-World Example**  

This setup is useful for **temporary file transfers**, **direct device communication**, **home & labs basic setups** or **troubleshooting network issues** without a switch.  

### 🛠 **PC Configuration (vPC Example)**  

```py
## VPC CONFIGURATION:

### VPC-1
set pcname VPC-1
ip 192.168.0.1 255.255.255.0
save

### VPC-2
set pcname VPC-2
ip 192.168.0.2 255.255.255.0
save
````



## 🔄 Hub-Based Network

![image](https://github.com/user-attachments/assets/ddb6c424-205b-4f12-8c1c-9038a46c897a)

### 📝 Hub: How It Works

- A hub is used to connect multiple PCs, but it still behaves like a single collision domain.
- It does not segment traffic—all devices can hear each other, leading to potential collisions.
- Each device still needs an IP in the same subnet, and no gateway or VLANs are needed.

### 🌍 Real-World Example

- Hubs are rarely used today but may still be found in legacy environments or simple lab setups.

### 🛠 PC Configuration (vPC Example)

- No configuration are needed in the Hub, since is just used por create the single collision domain

````py
## VPC CONFIGURATION:

### VPC-1
set pcname VPC-1
ip 192.168.0.1 255.255.255.0
save

### VPC-2
set pcname VPC-2
ip 192.168.0.2 255.255.255.0
save


````



## 🖧 Switch-Based Network (Access VLANs)

![image](https://github.com/user-attachments/assets/6fe90d9d-96a6-4356-a808-77daae9bec30)

### 📝 How It Works

- A Layer 2 switch introduces VLAN segmentation, creating separate broadcast domains.
- Each VLAN operates as its own isolated network unless a router or Layer 3 switch enables inter-VLAN communication.
- Devices within the same VLAN can communicate, but different VLANs cannot without additional configuration.

### 🌍 Real-World Example

Used in enterprise networks to separate departments, such as:

- VLAN 10 (ALFA) = HR Department
- VLAN 20 (BRAVO) = IT Department
- VLAN 69 (CHARLY) = Management-VLAN

### 🛠 Switch Configuration 

````py
!## SWITCH CONFIGURATION:
!
! ### Initialize Switch:
!
enable
configure terminal
hostname SW-1
!
! ### VLAN Creation & Naming:
!
vlan 10
name VLAN-10-ALFA
vlan 20
name VLAN-20-BRAVO
exit
!
! ### Assign VLANs to Access Interfaces:
!
interface range ethernet 0/0-1
description VLAN-10-ALFA
switchport mode access
switchport access vlan 10
no shutdown
exit
!
interface range ethernet 0/2-3
description VLAN-20-BRAVO
switchport mode access
switchport access vlan 20
no shutdown
end
!
! ### Save & Verify Configuration:
!
write memory
!
show vlan
!


````

### 🛠 **PC Configuration (vPC Example)**  

````
## VPC CONFIGURATION:

### VPC-1
set pcname VPC-1
ip 192.168.10.1 255.255.255.0
save

### VPC-2
set pcname VPC-2
ip 192.168.10.2 255.255.255.0
save

### VPC-3
set pcname VPC-3
ip 192.168.20.1 255.255.255.0
save

### VPC-4
set pcname VPC-4
ip 192.168.20.2 255.255.255.0
save


````





## 📞 Switch-Based Network (Access VLANs + Voice VLANs)  

![image](https://github.com/user-attachments/assets/9a2b9d66-0544-4794-9404-b482beba27af)  

## 📝 **How It Works**  

- A **Layer 2 switch** introduces **VLAN segmentation**, creating separate **broadcast domains**.  
- Unlike a regular **Access VLAN**, a **Voice VLAN** allows two VLANs to be configured on the same **port**:  
  - **Data VLAN** → For PCs or workstations.  
  - **Voice VLAN** → Dedicated for **VoIP traffic** (IP Phones).  
- This setup behaves like a **"mini trunk"**, separating both types of traffic while maintaining **one access VLAN** for data.  
- Devices in the **same VLAN can communicate**, but different VLANs require **inter-VLAN routing**.  
- The **Voice VLAN remains consistent** across all access ports, ensuring IP Phones can always communicate regardless of the assigned **Data VLAN**.  

**Example of Dual VLAN Configuration on an Access Port**:

```py
! # Data VLAN (Access)
switchport access vlan 10
! # Voice VLAN 
switchport voice vlan 50
```

![image](https://github.com/user-attachments/assets/1c4f344a-65a2-40b4-9b7f-d67c4aa90054)

### 🌍 Real-World Example

Used in enterprise networks where VoIP (Voice over IP) must be logically separated from regular data traffic.

- **By assigning a dedicated Voice VLAN, network administrators prioritize VoIP traffic (using QoS), ensuring clear and uninterrupted voice communication.**

📌 Example VLAN Setup:

- VLAN 10 (ALFA) → HR Department (PCs & Laptops)
- VLAN 20 (BRAVO) → IT Department (PCs & Laptops)
- VLAN 50 (ECHO) → Voice VLAN (IP Phones for all departments)

### 🛠 Switch Configuration 

````py
!## SWITCH CONFIGURATION:
!
! ### Initialize Switch:
!
enable
configure terminal
hostname SW-1
!
! ### VLAN Creation & Naming:
!
vlan 10
name VLAN-10-ALFA
vlan 20
name VLAN-20-BRAVO
vlan 50
name VLAN-50-VOICE
!
! ### Assign VLANs to Access Interfaces:
!
interface range fa 0/1-2
description VLAN-10-ALFA
switchport mode access
switchport access vlan 10
switchport voice vlan 50
no shutdown
exit
!
interface range fa 0/3-4
description VLAN-20-BRAVO
switchport mode access
switchport access vlan 20
switchport voice vlan 50
no shutdown
end
!
! ### Save & Verify Configuration:
!
write memory
!
show vlan
!


````

### 🛠 VPC Configuration 

````py
## VPC CONFIGURATION:

### VPC-1 (HR Department)
set pcname VPC-1
ip 192.168.10.1 255.255.255.0
save

### VPC-2 (HR Department)
set pcname VPC-2
ip 192.168.10.2 255.255.255.0
save

### VPC-3 (IT Department)
set pcname VPC-3
ip 192.168.20.1 255.255.255.0
save

### VPC-4 (IT Department)
set pcname VPC-4
ip 192.168.20.2 255.255.255.0
save
````


### 🛠 VoIP Phone  Configuration 

````py
## VOIP PHONE CONFIGURATION: (*VoIP Phones usually uses DHCP)

### VoIP-Phone-1
set pcname IP-PHONE
ip 192.168.50.1 255.255.255.0
save

### VoIP-Phone-2
set pcname IP-PHONE
ip 192.168.50.2 255.255.255.0
save

### VoIP-Phone-3
set pcname IP-PHONE
ip 192.168.50.3 255.255.255.0
save

### VoIP-Phone-4
set pcname IP-PHONE
ip 192.168.50.4 255.255.255.0
save
````
























## 🔀 **Switch-Based Network (Trunk VLANs - 802.1Q)**  

![image](https://github.com/user-attachments/assets/f517bee4-8057-4aaa-9e7c-ff01f40b1c16)


### 📝 **How It Works**  

In this configuration, we're dealing with **VLAN trunking** using the **802.1Q encapsulation** protocol. This allows multiple VLANs to travel over a single link between switches. A **trunk link** is created between two switches, and multiple VLANs are allowed to pass through this trunk. To make this work, the trunk uses **802.1Q encapsulation**, which tags each frame with the VLAN ID to keep traffic isolated and maintain VLAN segmentation.

- **802.1Q** allows multiple VLANs to share a single physical link by tagging Ethernet frames with VLAN identifiers.
- The **native VLAN** is the default VLAN used for untagged traffic on the trunk link.
- The trunk link allows us to specify which VLANs are allowed to traverse the link, improving network performance and security.

### 🌍 **Real-World Example**

In a **typical enterprise setup**, VLAN trunking is used to allow communication between different parts of the network. For instance:

- **VLAN 10 (ALFA)** → HR Department (PCs & Laptops)
- **VLAN 20 (BRAVO)** → IT Department (PCs & Laptops)
- **VLAN 99 (NATIVE)** → Native VLAN (used for 802.1Q trunk encapsulation)

This setup ensures that traffic from different VLANs can flow between switches without leaking into other VLANs, while untagged frames (typically for management or legacy devices) go through the native VLAN.

### 📌 **Example VLAN Setup**:

- **VLAN 10 (ALFA)** for the HR Department.
- **VLAN 20 (BRAVO)** for the IT Department.
- **VLAN 50 (VOICE)** for Voice over IP (VoIP) services.
- **VLAN 99 (NATIVE)** as the Native VLAN for trunk links.

---


### 🔍 VLANs & Trunks: 802.1Q vs ISL vs Tagged/Untagged VLANs**  

In modern networking, VLANs (Virtual Local Area Networks) are **essential for traffic segmentation**, security, and scalability. However, different **vendors and standards** have implemented VLAN trunking in **different ways**, leading to various **encapsulation methods and configurations**.  

Understanding different trunking concepts:

- 🌎 Different Vendors, Different Methods – Not all networks use Cisco switches. Juniper, HP, Ruckus/Brocade, Arista, and Extreme handle VLAN trunking differently, so knowing their approach is crucial for compatibility.
- 🔗 Avoid Misconfigurations & VLAN Leaks – Each vendor uses 802.1Q, ISL, or Tagged/Untagged VLANs differently. Misunderstanding these can cause trunk failures, connectivity issues, or VLAN leaks.
- 🏢 Legacy vs Modern Networks – Older Cisco networks may still use ISL, while modern setups rely on 802.1Q. Meanwhile, non-Cisco vendors replace Trunk & Allowed VLANs with Tagged/Untagged VLANs for trunking.

**⚠️ Best Practice:** While it is possible to mix different vendors, models, and VLAN trunking methods, the **best practice is to standardize on a single vendor, model series, and VLAN trunking method whenever possible**. Even if configured correctly, mixing different vendors can introduce firmware bugs, hidden incompatibilities, or unexpected behavior that may cause network instability.


| **Feature**              | **802.1Q (Dot1Q)** 🌐 | **ISL (Inter-Switch Link)** 🔗 | **Tagged/Untagged (Non-Cisco Vendors)** 🎭 |
|--------------------------|----------------------|------------------------|--------------------------------|
| **Standard**            | IEEE **802.1Q** (Industry Standard) | Cisco Proprietary | Vendor-Specific (Implemented Differently by Juniper, Ruckus/Brocade, HP, etc.) |
| **Vendor**              | Multi-vendor (Cisco, Juniper, HP, Arista, etc.) | Cisco-only | Used by non-Cisco vendors (Ruckus/Brocade, HP, Extreme, Juniper, etc.) |
| **Year Introduced**     | **1998** | **Mid-1990s** | **Varies by vendor** |
| **Layer**               | **Layer 2 (Data Link)** | **Layer 2 (Data Link)** | **Layer 2 (Data Link)** |
| **Encapsulation Method** | Inserts a **4-byte VLAN tag** into the Ethernet frame | **Encapsulates the entire Ethernet frame** with a **26-byte header** + **4-byte trailer** | Uses **Tagged VLANs** for trunk-like behavior & **Untagged VLANs** for default/native traffic |
| **VLAN ID Range**       | **1-4094** (12-bit VLAN field) | **1-1005** (10-bit VLAN field) | **1-4094**, varies by vendor |
| **Native VLAN Support** | ✅ **Yes** (Native VLAN traffic is sent untagged) | ❌ **No** (All frames are encapsulated, no native VLAN concept) | ✅ **Yes**, but called **Untagged VLAN** instead of Native VLAN |
| **Frame Size Overhead** | **4 bytes** added to the Ethernet frame | **30 bytes** added (high overhead) | **Tagged VLANs add 4 bytes**, **Untagged VLANs remain at 1518 bytes** |
| **Maximum Frame Size**  | **1522 bytes** (1518 + 4-byte VLAN tag) | **1548 bytes** (1518 + 30-byte ISL encapsulation) | **Tagged: 1522 bytes**, **Untagged: 1518 bytes** |
| **MTU Impact**          | Slight increase due to **4-byte tag** | Significant due to **30-byte encapsulation** | Same as **802.1Q** (depends on whether frames are tagged) |
| **Trunking Support**    | ✅ **Yes** (Industry standard for multi-VLAN links) | ✅ **Yes** (Only between Cisco devices) | ✅ **Yes**, but implemented differently per vendor |
| **How VLANs Are Assigned** | Uses **Access Ports (1 VLAN)** and **Trunk Ports (Multiple VLANs)** | Uses **Trunk Ports Only** (Every frame is encapsulated) | Uses **Untagged VLAN (Similar to Native VLAN)** + **Tagged VLANs (Similar to Allowed VLANs in Cisco)** |
| **Interoperability**    | ✅ **Vendor-neutral, works across all major vendors** | ❌ **Cisco-only (Not supported by other vendors)** | ✅ **Vendor-specific implementations, but interoperable with 802.1Q** |
| **Real-World Scenario** | A **Cisco switch** communicating with a **Juniper or HP switch** over a trunk link | **Legacy Cisco networks** using older switches with ISL support | **Ruckus/Brocade, HP, and Juniper switches** use **Untagged VLAN** for default traffic & **Tagged VLANs** for multi-VLAN links |
| **Current Adoption**    | ⭐⭐⭐⭐⭐ (Industry Standard, Most Common) | ⭐ (Deprecated, replaced by 802.1Q) | ⭐⭐⭐⭐ (Common in non-Cisco environments) |


1️⃣ **802.1Q (Cisco & Multi-Vendor Standard)**
   - Uses **Native VLAN** for untagged traffic.  
   - Uses **Allowed VLANs** for tagged VLANs on trunk ports.  
   - **Industry standard, works across vendors** (Cisco, Juniper, HP, etc.).  

2️⃣ **ISL (Cisco Proprietary)**
   - Encapsulates entire Ethernet frame (**High Overhead, Deprecated**).  
   - **Cisco-only** and replaced by **802.1Q** in modern networks.  

3️⃣ **Tagged/Untagged VLANs (Ruckus, Brocade, HP, Juniper)**
   - **Untagged VLAN** = Default VLAN (Similar to Cisco’s Native VLAN).  
   - **Tagged VLANs** = VLANs allowed through the trunk (Similar to Cisco’s Allowed VLANs).  
   - **Different syntax, but functionally similar to Cisco trunking**.  


#### 🛠 Switch-1 Configuration 

````py
!## SWITCH 1 CONFIGURATION:
!
! ### Initialize Switch:
!
enable
configure terminal
hostname SW-1
!
! ### VLAN Creation & Naming:
!
vlan 10
name VLAN-10-ALFA
vlan 20
name VLAN-20-BRAVO
vlan 50
name VLAN-50-VOICE
vlan 99
name VLAN-99-NATIVE-TRUNK
!
! ### Assign VLANs to Access Interfaces:
!
interface range ethernet 0/0-1
description VLAN-10-ALFA
switchport mode access
switchport access vlan 10
switchport voice vlan 50
no shutdown
exit
!
interface range ethernet 0/2-3
description VLAN-20-BRAVO
switchport mode access
switchport access vlan 20
switchport voice vlan 50
no shutdown
exit
!
! ### Create Trunk Port & Assign Native VLAN & Allowed VLANs
!
! # TRUNK BETWEEN SWITCH 1 & SWITCH 2:
interface ethernet 1/0
description TRUNK_LINK_SW1<->SW2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,50,99
switchport trunk native vlan 99
end
!
! ### Save & Verify Configuration:
!
write memory
!
show vlan
!
!
show interfaces trunk
!
````


### 🛠 Switch-2 Configuration 

````py
!## SWITCH 2 CONFIGURATION:
!
! ### Initialize Switch:
!
enable
configure terminal
hostname SW-2
!
! ### VLAN Creation & Naming:
!
vlan 10
name VLAN-10-ALFA
vlan 20
name VLAN-20-BRAVO
vlan 50
name VLAN-50-VOICE
vlan 99
name VLAN-99-NATIVE-TRUNK
!
! ### Assign VLANs to Access Interfaces:
!
interface range ethernet 0/0-1
description VLAN-20-BRAVO
switchport mode access
switchport access vlan 20
switchport voice vlan 50
no shutdown
exit
!
interface range ethernet 0/2-3
description VLAN-10-ALFA
switchport mode access
switchport access vlan 10
switchport voice vlan 50
no shutdown
exit
!
! ### Create Trunk Port & Assign Native VLAN & Allowed VLANs
!
! # TRUNK BETWEEN SWITCH 1 & SWITCH 2:
interface ethernet 1/0
description TRUNK_LINK_SW1<->SW2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,50,99
switchport trunk native vlan 99
end
!
! ### Save & Verify Configuration:
!
write memory
!
show vlan
!
!
show interfaces trunk
!

````

### 🛠 VPC Configuration 

````py
## VPC CONFIGURATION:

### VPC-1 
set pcname VPC-1
ip 192.168.10.1 255.255.255.0
save

### VPC-2 
set pcname VPC-2
ip 192.168.10.2 255.255.255.0
save

### VPC-3 
set pcname VPC-3
ip 192.168.20.1 255.255.255.0
save

### VPC-4
set pcname VPC-4
ip 192.168.20.2 255.255.255.0
save

### VPC-5 
set pcname VPC-5
ip 192.168.20.3 255.255.255.0
save

### VPC-6 
set pcname VPC-6
ip 192.168.20.4 255.255.255.0
save

### VPC-7
set pcname VPC-7
ip 192.168.10.3 255.255.255.0
save

### VPC-8 
set pcname VPC-8
ip 192.168.10.4 255.255.255.0
save


````





















## 🌐 **Router on a Stick + InterVLAN Routing + (802.1Q Trunking)**

![image](https://github.com/user-attachments/assets/122c5282-b8af-49d0-a718-999c4827f1ac)

### 📝 **What is a Router on a Stick?**

A **Router on a Stick** is a network design where a single physical interface on a router is used to route traffic between multiple VLANs. This is achieved using **sub-interfaces** and **802.1Q trunking**. Each VLAN has a corresponding sub-interface on the router, allowing it to provide inter-VLAN routing.

This Router-on-a-Stick setup ensures efficient VLAN segmentation while providing inter-VLAN communication through the router. 

- The router is connected to the **switch via a trunk link**.
- Each VLAN is assigned a **sub-interface** on the router.
- The **router acts as the default gateway** for devices in each VLAN.

This setup is commonly used in small-to-medium-sized networks where a **Layer 3 switch is not available** to handle inter-VLAN routing.

- **PCs must now configure their gateway to enable the router to perform inter-VLAN routing. The gateway address should match the IP of the sub-interface assigned to each VLAN.**

### 🌍 **Real-World Example**

The Router-on-a-Stick method is one of the most widely used inter-VLAN routing solutions, especially in environments where a Layer 3 switch is not available or where routing is managed externally. While enterprise networks often use Layer 3 switches, many small-to-medium-sized businesses, branch offices, and even large-scale deployments rely on this method—sometimes unknowingly.

Many modern firewalls, like FortiGate, Palo Alto, and Cisco ASA, are essentially performing a Router-on-a-Stick function when they handle inter-VLAN routing via sub-interfaces. The only difference is that they add security policies and inspection on top of the routing process. However, any firewall or router can act as the Router-on-a-Stick in a network, handling both inter-VLAN routing and external internet traffic.

### 📌 **Router Configuration (Router-on-a-Stick)**

```py
!## ROUTER CONFIGURATION: ROUTER ON A STICK
!
!    # IMPORTANT NOTES!!!
!    # - The Native VLAN does not require an IP address because its primary function is to handle untagged traffic on an 802.1Q trunk.
!    # - The "encapsulation dot1Q 1 native" command is used on cisco router to a associate a subinterface to a VLAN, configured as Native on the Switch. (default is using automatic/dynamic VLAN)
!    #   * The other VLANs (non native) does not need to be tagged as "native" in the encapsulation.
!    # - If "ip routing" is not enabled, the router will not perform inter-VLAN routing and devices in different VLANs will not be able to communicate with each other (but can connect to Internet or be manually routed). 
!    #          For static routing, routing is manually configured using the ip route command, and ip routing is not required to be manually enabled.
!    #          However, for dynamic routing (such as EIGRP or OSPF), once the protocol is configured, ip routing is implicitly enabled to allow the router to exchange and process routing information.
!
! ### Initialize Router:
!
enable
configure terminal
hostname R1
!
! ### Enable Trunking on Router Interface:
!
interface ethernet 0/0
 description Trunk Link to SW-1
 no shutdown
!
! ### Create Sub-interfaces for VLANs:
!
interface ethernet 0/0.10
 description VLAN 10 - ALFA
 encapsulation dot1Q 10
 ip address 192.168.10.254 255.255.255.0
 no shutdown
 exit
!
interface ethernet 0/0.20
 description VLAN 20 - BRAVO
 encapsulation dot1Q 20
 ip address 192.168.20.254 255.255.255.0
 no shutdown
 exit
!
interface ethernet 0/0.50
 description VLAN 50 - VOICE
 encapsulation dot1Q 50
 ip address 192.168.50.254 255.255.255.0
 no shutdown
 exit
!
interface ethernet 0/0.99
 description VLAN 99 - NATIVE VLAN
 encapsulation dot1Q 99 native
 no ip address
 no shutdown
 exit
!
! ### Enable Routing / >>> InterVLAN Routing <<<
!
ip routing
!
! ### Save Configuration:
!
end
write memory
!
! ### Verify Configurations:
!
show ip interface brief
!
!


```

---




#### 🛠 Switch-1 Configuration 

````py
!## SWITCH 1 CONFIGURATION:
!
! ### Initialize Switch:
!
enable
configure terminal
hostname SW-1
!
! ### VLAN Creation & Naming:
!
vlan 10
name VLAN-10-ALFA
vlan 20
name VLAN-20-BRAVO
vlan 50
name VLAN-50-VOICE
vlan 99
name VLAN-99-NATIVE-TRUNK
!
! ### Assign VLANs to Access Interfaces:
!
interface range ethernet 0/0-1
description VLAN-10-ALFA
switchport mode access
switchport access vlan 10
switchport voice vlan 50
no shutdown
exit
!
interface range ethernet 0/2-3
description VLAN-20-BRAVO
switchport mode access
switchport access vlan 20
switchport voice vlan 50
no shutdown
exit
!
! ### Create Trunk Port & Assign Native VLAN & Allowed VLANs
!
! # TRUNK BETWEEN SWITCH 1 & SWITCH 2:
interface ethernet 1/0
description TRUNK_LINK_SW1<->SW2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,50,99
switchport trunk native vlan 99
no shutdown
exit
!
! # TRUNK BETWEEN SWITCH 1 ROUTER (ROUTER-ON-A-STICK)
interface ethernet 1/3
description TRUNK_LINK_TO_ROUTER
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,50,99
switchport trunk native vlan 99
no shutdown
end
!
! ### Save & Verify Configuration:
!
write memory
!
show vlan
!
!
show interfaces trunk
!


````


### 🛠 Switch-2 Configuration 

````py
!## SWITCH 2 CONFIGURATION:
!
! ### Initialize Switch:
!
enable
configure terminal
hostname SW-2
!
! ### VLAN Creation & Naming:
!
vlan 10
name VLAN-10-ALFA
vlan 20
name VLAN-20-BRAVO
vlan 50
name VLAN-50-VOICE
vlan 99
name VLAN-99-NATIVE-TRUNK
!
! ### Assign VLANs to Access Interfaces:
!
interface range ethernet 0/0-1
description VLAN-20-BRAVO
switchport mode access
switchport access vlan 20
switchport voice vlan 50
no shutdown
exit
!
interface range ethernet 0/2-3
description VLAN-10-ALFA
switchport mode access
switchport access vlan 10
switchport voice vlan 50
no shutdown
exit
!
! ### Create Trunk Port & Assign Native VLAN & Allowed VLANs
!
! # TRUNK BETWEEN SWITCH 1 & SWITCH 2:
interface ethernet 1/0
description TRUNK_LINK_SW1<->SW2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,50,99
switchport trunk native vlan 99
end
!
write memory
!
! ### Save & Verify Configuration:
!
write memory
!
show vlan
!
!
show interfaces trunk
!

````




### 🛠 **Switch-1 Configuration (Updated for Router-on-a-Stick)**

```
!## SWITCH 1 CONFIGURATION (UPDATED FOR ROUTER-ON-A-STICK):
!

```




### 🛠 VPC Configuration 

- **PCs must now configure their gateway to enable the router to perform inter-VLAN routing. The gateway address should match the IP of the sub-interface assigned to each VLAN.**

````py
## VPC CONFIGURATION:

### VPC-1 
set pcname VPC-1
ip 192.168.10.1 255.255.255.0 192.168.10.254
save

### VPC-2 
set pcname VPC-2
ip 192.168.10.2 255.255.255.0 192.168.10.254
save

### VPC-3 
set pcname VPC-3
ip 192.168.20.1 255.255.255.0 192.168.20.254
save

### VPC-4
set pcname VPC-4
ip 192.168.20.2 255.255.255.0 192.168.20.254
save

### VPC-5 
set pcname VPC-5
ip 192.168.20.3 255.255.255.0 192.168.20.254
save

### VPC-6 
set pcname VPC-6
ip 192.168.20.4 255.255.255.0 192.168.20.254
save

### VPC-7
set pcname VPC-7
ip 192.168.10.3 255.255.255.0 192.168.10.254
save

### VPC-8 
set pcname VPC-8
ip 192.168.10.4 255.255.255.0 192.168.10.254
save


````











































































## 🔀 VTP for automatic VLAN Database sharing

### 📝 **How It Works**

VTP (VLAN Trunking Protocol) allows switches to share VLAN information. It simplifies network management by enabling a centralized VLAN configuration on one switch (VTP Server), which then propagates to other switches (VTP Clients) in the same domain.

- **VTP does not directly depend on encapsulation (ISL or 802.1Q); its main function is to propagate VLANs between the switches in the network.** So, VTP mostly use a traditional 802.1q VLAN/Trunk base config, and share that config to other switches in the network (VTP domain)

**VTP Switch modes:**

- **VTP Server**: The switch where VLANs are created and managed. The VLAN information is propagated to all other switches in the VTP domain.
- **VTP Client**: Switches that receive VLAN information from the VTP server but cannot modify the VLAN database.

The VTP domain name and password (if configured) must match across all switches in the same VTP domain.

### 🌍 Real-World Example

📌 Example VLAN Setup:

- **VLAN 10 (ALFA)** for the HR Department.
- **VLAN 20 (BRAVO)** for the IT Department.
- **VLAN 50 (VOICE)** for Voice over IP (VoIP) services.
- **VLAN 99 (NATIVE)** as the Native VLAN for trunk links.

### 🛠 Switch-1 Configuration

```py
!## SWITCH 1 CONFIGURATION (VTP Mode Server):
!
! ### Initialize the Switch:
!
enable
configure terminal
hostname SW-1
!
! ### Configure VTP Domain:
! # Ensure the VTP domain is the same on all switches
vtp domain Fz3r0_VTP_Domain
!
! ### Configure VTP Mode (Server Mode):
! # In this mode, VLANs are created and propagated
vtp mode server
!
! ### Set VTP Password (optional but recommended):
! # The password must be the same on all switches for security
vtp password p4SSw0rd
!
! ### Create VLANs (these VLANs will be propagated):
!
vlan 10
name VLAN-10-ALFA
vlan 20
name VLAN-20-BRAVO
vlan 50
name VLAN-50-VOICE
vlan 99
name VLAN-99-NATIVE-TRUNK
!
! ### Configure Trunk Port (802.1Q encapsulation):
!
interface ethernet 1/0
description TRUNK_LINK_SW1<->SW2
switchport trunk encapsulation dot1q
switchport mode trunk

switchport trunk allowed vlan 10,20,50,99
switchport trunk native vlan 99
no shutdown
exit
end
!
! ### Save & Check Config:
!
write memory
!
show vtp status
!
!

````

### 🛠 Switch-2 Configuration

````py
!## SWITCH 2 CONFIGURATION (VTP Mode Client):
!
! ### Initialize the Switch:
!
enable
configure terminal
hostname SW-2
!
! ### Configure VTP Domain:
! # Ensure the VTP domain is the same on all switches
vtp domain Fz3r0_VTP_Domain
!
! ### Configure VTP Mode (Client Mode):
! # This switch only receives VLANs from the Server and cannot modify them
vtp mode client
!
! ### Set VTP Password (must match with server password):
! # The password must be the same on all switches for security
vtp password p4SSw0rd
!
! ### Configure Trunk Port (802.1Q encapsulation):
interface ethernet 1/0
description TRUNK_LINK_SW1<->SW2
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20,50,99
switchport trunk native vlan 99
no shutdown
exit
!
end
!
! ### Save Configuration:
write memory
!

````





