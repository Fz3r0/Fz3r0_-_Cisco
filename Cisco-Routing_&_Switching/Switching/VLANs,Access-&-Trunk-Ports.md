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



















## 📞 Switch-Based Network (Trunk VLANs)  



### 📝 **How It Works**  




### 🌍 Real-World Example



📌 Example VLAN Setup:

- VLAN 10 (ALFA) → HR Department (PCs & Laptops) = Trunk Allowed VLAN
- VLAN 20 (BRAVO) → IT Department (PCs & Laptops) = Trunk Allowed VLAN
- VLAN 99 (NATIVE) → Native VLAN (For 802.1q Trunk encapsulation) = Trunk Native VLAN

### 🛠 Switch-1 Configuration 

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
switchport mode trunk
switchport trunk encapsulation dot1q
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
switchport mode trunk
switchport trunk encapsulation dot1q
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

### VPC-5 (HR Department)
set pcname VPC-5
ip 192.168.10.3 255.255.255.0
save

### VPC-6 (HR Department)
set pcname VPC-6
ip 192.168.10.4 255.255.255.0
save

### VPC-7 (IT Department)
set pcname VPC-7
ip 192.168.20.3 255.255.255.0
save

### VPC-8 (IT Department)
set pcname VPC-8
ip 192.168.20.4 255.255.255.0
save
````










