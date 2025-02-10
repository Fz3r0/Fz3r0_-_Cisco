# 🔥🧱🛡️ Cisco: `Dynamic Routing : EIGRP @ Packet Tracer`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Dynamic Routing` `EIGRP` `Packet Tracer`

---

<br>



- 

# CCNA Lab: `Dynamic Routing` :: `EIGRP`


## Lab Files

- [Download Cisco Packet Tracer Fz3r0 Labs :: **Dynamic Route x5 Routers : `EIGRP`**]()

# `Dynamic Routing @ Packet Tracer`

## Dynamic Routing: Topology

This setup consists of **five routers** (R1 to R5) connected in a **ring topology**, each with:

- **Two WAN interfaces** using `/30 subnets` (point-to-point links between routers).
- **One LAN interface** using `/24 subnets` (local network per router).
- **Each LAN** have a PC for testing purposes. 

![image](https://github.com/user-attachments/assets/b69ec954-6c59-462b-a89d-6539668b0b2d)

## 📋 **IP Addressing Table**

| Device  | Interface | IP Address   | Subnet Mask      | Network Address with CIDR |
|---------|-----------|--------------|------------------|---------------------------|
| **R1**  | Fa0/0    | 10.1.0.1      | 255.255.255.252  | 10.1.0.0/30               |
|         | Fa0/1    | 10.5.0.2      | 255.255.255.252  | 10.5.0.0/30               |
|         | Fa1/1    | 192.168.1.1   | 255.255.255.0    | 192.168.1.0/24            |
| **R2**  | Fa0/0    | 10.2.0.1      | 255.255.255.252  | 10.2.0.0/30               |
|         | Fa0/1    | 10.1.0.2      | 255.255.255.252  | 10.1.0.0/30               |
|         | Fa1/1    | 192.168.2.1   | 255.255.255.0    | 192.168.2.0/24            |
| **R3**  | Fa0/0    | 10.3.0.1      | 255.255.255.252  | 10.3.0.0/30               |
|         | Fa0/1    | 10.2.0.2      | 255.255.255.252  | 10.2.0.0/30               |
|         | Fa1/1    | 192.168.3.1   | 255.255.255.0    | 192.168.3.0/24            |
| **R4**  | Fa0/0    | 10.4.0.1      | 255.255.255.252  | 10.4.0.0/30               |
|         | Fa0/1    | 10.3.0.2      | 255.255.255.252  | 10.3.0.0/30               |
|         | Fa1/1    | 192.168.4.1   | 255.255.255.0    | 192.168.4.0/24            |
| **R5**  | Fa0/0    | 10.5.0.1      | 255.255.255.252  | 10.5.0.0/30               |
|         | Fa0/1    | 10.4.0.2      | 255.255.255.252  | 10.4.0.0/30               |
|         | Fa1/1    | 192.168.5.1   | 255.255.255.0    | 192.168.5.0/24            |

- `PC-1 (Site-A)` :: 192.168.1.100/24
- `PC-2 (Site-B)` :: 192.168.2.100/24
- `PC-3 (Site-C)` :: 192.168.3.100/24
- `PC-4 (Site-D)` :: 192.168.4.100/24
- `PC-5 (Site-E)` :: 192.168.5.100/24
  
---

## Init Config


### Init Setup: `Router 1`

````py
! ## ROUTER 1
!
enable
configure terminal
!
hostname R1
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.1.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.5.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.1.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````

### Init Setup: `Router 2`

````py
! ## ROUTER 2
!
enable
configure terminal
!
hostname R2
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.2.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.1.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.2.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````

### Init Setup: `Router 3`

````py
! ## ROUTER 3
!
enable
configure terminal
!
hostname R3
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.3.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.2.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.3.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````

### Init Setup: `Router 4`

````py
! ## ROUTER 4
!
enable
configure terminal
!
hostname R4
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.4.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.3.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.4.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````

### Init Setup: `Router 5`

````py
! ## ROUTER 5
!
enable
configure terminal
!
hostname R5
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.5.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.4.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.5.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````







# 🚀 EIGRP @ Packet Tracer Configuration 




## 📖 **What is EIGRP?**

**Enhanced Interior Gateway Routing Protocol (EIGRP)** is a Cisco proprietary routing protocol that combines the best features of distance-vector and link-state protocols. It is a hybrid protocol, offering faster convergence and lower bandwidth usage compared to other distance-vector protocols like RIP.

The protocol was designed by Cisco Systems in 1992 as a proprietary protocol, available only on Cisco routers. In 2013, Cisco permitted other vendors to freely implement a limited version of EIGRP with some of its associated features such as High Availability (HA), while withholding other EIGRP features such as EIGRP stub, needed for DMVPN and large-scale campus deployment.

- **Hybrid Distance Vector Routing Protocol**: Called "hybrid" because it combines features of **distance vector** and **link-state** protocols. <br><br>
- **Successor of IGRP** (Deprecated): IGRP is no longer used because it had **slower convergence**, used **less efficient metrics** and EIGRP became open standard in 2016 (RFC 7868).  

### **Why Use EIGRP?** 🌐

EIGRP is used primarily in larger, more complex networks because of its ability to scale efficiently and support advanced features, such as:

- **Fast convergence**: Reacts quickly to changes in the network.
- **Efficient use of bandwidth**: EIGRP sends updates only when a change occurs, rather than periodically.
- **Supports multiple network layer protocols**: Works with both IPv4 and IPv6.
- **Automatic summarization** (in earlier versions) and **manual summarization**.

### **When to Use EIGRP?** 🏙️

EIGRP is ideal for:

- Medium to large networks with Cisco routers.
- Networks requiring fast convergence (faster than RIP).
- Scenarios where IP address summarization and route aggregation are needed.
  
### **When NOT to Use EIGRP?** 🚫

EIGRP may not be the best choice when:

- Working in multi-vendor environments (since it's Cisco proprietary).
- You need a simple, low-overhead solution (RIP might be enough).
- Extremely small networks where simpler protocols like static routing may be sufficient.

### **Real-life Examples** 🌍

- **Enterprise Networks**: Large companies using EIGRP for internal routing across multiple office locations.
- **Data Centers**: EIGRP is widely used to ensure optimal routing and fast recovery from link failures.

## ⚙️ EIGRP Features

| Feature | Description |
|---------|------------|
| **Standard** | Originally **proprietary** (Cisco), later **open standard** |
| **Type** | **Distance Vector** protocol |
| **Algorithm** | Uses **DUAL (Diffusing Update Algorithm)** for fast convergence |
| **Protocol Support** | Supports **IPv4, IPv6**, and **legacy protocols (IPX, AppleTalk)** |
| **Metric Calculation** | Based on **Bandwidth + Delay** |
| **Administrative Distance (AD)** | Determines **trust level** of routes |

## 🔄 **EIGRP Operation**

- **Does not use TCP or UDP** – Uses its own protocol: **RTP (Reliable Transport Protocol)**.
- **Uses Autonomous System (AS) numbers** for activation.
- **Sends HELLO messages** to maintain neighbor relationships.
- **Uses separate modules for each routed protocol** (**PDM – Protocol Dependent Modules**).
- **Limited updates** – Only sends updates when changes occur in the network.
- **Maximum hop count:** `255` (`100` by default).
- **Supports manual summarization** on required interfaces.

### 🗂️ **Neighbor & Topology Tables**

- **Neighbor Table:** Stores information about directly connected routers.
- **Topology Table:** Maintains all learned routes, including successors and feasible successors.

## 🔢 EIGRP: `Autonomous System (AS)`

An Autonomous System (AS) number is a unique identifier assigned to a collection of IP networks and routers under the control of a single organization that presents a common routing policy to the internet. 
 
- NOTE: Each AS is identified by a **unique number**, called an **AS Number (ASN)**, which is used for **`inter-domain routing (BGP)`**.  

**The same AS number is used to group routers together so they can exchange routing information.** (_Different AS numbers would indicate that routers belong to separate routing domains, and they will not form neighbor relationships with each other._)

- **AS is used in routing protocols, such as `BGP (Border Gateway Protocol)` and `EIGRP (Enhanced Interior Gateway Routing Protocol)`, to identify and differentiate between different networks on the internet or within a private network.**

**Why Do We Always Use the Same AS Number?**

- In the case of EIGRP, all routers within a single organization or network that are participating in the same routing domain (group of routers exchanging routing information) must use the same AS number to form neighbor relationships and exchange routes.
- This consistency ensures that the routers recognize each other as part of the same routing domain and share routing information effectively.

**IMPORTANT**: If the AS number differs between routers, they won't recognize each other as part of the same network, and they won't exchange routing information.

### 🔹 **Administrative Distance (AD) Default Values**

| AD Type | Value | Explanation |
|---------|-------|------------|
| **Internal AD** | 90 | Routes **within the same AS (Autonomous System)** |
| **External AD** | 170 | Routes **redistributed from other protocols (OSPF, RIP, etc.)** |
| **Summary AD** | 5 | **Manually configured summary routes** (trusted due to admin configuration) |



### AS: What Other Numbers Can Be Used?

- The AS number in EIGRP can be any value between 1 and 65,535.
- 1 to 65,535: Used for internal EIGRP (within a private network).
- ASN 0 is not valid in EIGRP.

### AS: How to Choose an AS Number?

- In private networks, you can pick any AS number (e.g., router eigrp 100, router eigrp 200).
- In large enterprises, different AS numbers may be used to separate different routing domains.
- In EIGRP Named Mode, the AS number is still required but used differently.





# 🌍 **Autonomous System (AS) Numbers**

## 📌 What is an Autonomous System (AS)?  


---

## 🌍 AS Numbers

### 🏛 **2-Byte (16-bit) AS Numbers**

| **Type**     | **Range**      | **Description** |
|-------------|--------------|----------------|
| **Public AS**  | `1 - 64511`  | Used by **ISPs and large organizations** for **global** routing. _(All public ASNs in this range are already allocated.)_ |
| **Private AS** | `64512 - 65535` | Used **internally** within organizations (not advertised on the internet). |

###  🚀 **4-Byte (32-bit) AS Numbers**  

Compatible with **2-byte ASNs** and introduced to **support future growth**.

**Why were 4-byte AS numbers introduced?**  

- The **2-byte AS numbers (1 - 64511)** were **completely assigned**, making it necessary to introduce a larger range. 

| **Type**     | **Range**         | **Description** |
|-------------|-----------------|----------------|
| **Public AS**  | `65536 - 4294967296` | Used for **global internet routing** (similar to old 2-byte ASNs but with a much larger pool). |
| **Private AS** | `64512 - 65535` | Same range as in 2-byte ASNs, **reserved for internal use**. |
| **Reserved AS** | `23456` | **Placeholder ASN** used for backward compatibility between 2-byte and 4-byte ASNs. |

**Why was AS `23456` reserved?** 

- When **BGP peers do not support 4-byte ASNs**, they use **AS 23456** as a **fallback** to maintain compatibility.


## 🔹 EIGRP: **Multicast & MAC Addresses**

| Address Type | Value |
|-------------|-------|
| **Multicast Address** | `224.0.0.10` |
| **Multicast MAC Address** | `01:00:5E:00:00:0A` |










## **Basic EIGRP Configuration Steps** ⚙️

1. **Enable EIGRP routing**:
   
   - Use the `router eigrp` command followed by a unique Autonomous System (AS) number. 
   - All routers in the same EIGRP network must share the same AS number.

2. **Activate EIGRP on Interfaces**:
   
   - EIGRP needs to be enabled on each router's interfaces for it to advertise routes.

3. **Network Statements**:
   
   - Define which networks should be included in EIGRP by using `network` statements that match the router's interface IP addresses.

4. **Verify the Configuration**:
   
   - After configuring, use commands like `show ip eigrp neighbors` and `show ip route` to verify that EIGRP is working properly.

---

## ⚡ **EIGRP Configuration** 


In this example, we will configure EIGRP between five routers (R1 to R5) in the ring topology, as defined in the setup.



### **R1 Configuration:**

```py
! # ROUTER 1 : EIGRP
!
enable
configure terminal
hostname R1
!
! # Autonomous System (AS)
router eigrp 1
!
! # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
! # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!     # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!     # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!     #   - This corresponds to a /30 subnet (255.255.255.252).
!     #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.1.0.0 0.0.0.3
network 10.5.0.0 0.0.0.3
!
! # Enables EIGRP on interfaces in the 192.168.1.0/24 subnet.
!     # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.1.0 to 192.168.1.255"
network 192.168.1.0 0.0.0.255
!
! # Disables automatic summarization.
no auto-summary
!
! # exit and save
end
write memory
!

````

### **R2 Configuration:**

```py
! # ROUTER 2 : EIGRP
!
enable
configure terminal
hostname R2
!
! # Autonomous System (AS)
router eigrp 1
!
network 10.2.0.0 0.0.0.3
network 10.1.0.0 0.0.0.3
network 192.168.2.0 0.0.0.255
no auto-summary
exit
!
write memory


````


#### **R3 Configuration:**

```py
! # ROUTER 3 : EIGRP
!
enable
configure terminal
hostname R3
!
! # Autonomous System (AS)
router eigrp 1
!
network 10.3.0.0 0.0.0.3
network 10.2.0.0 0.0.0.3
network 192.168.3.0 0.0.0.255
no auto-summary
exit
!
write memory


````


#### **R4 Configuration:**

```py
! # ROUTER 4 : EIGRP
!
enable
configure terminal
hostname R4
!
! # Autonomous System (AS)
router eigrp 1
!
network 10.4.0.0 0.0.0.3
network 10.3.0.0 0.0.0.3
network 192.168.4.0 0.0.0.255
no auto-summary
exit
!
write memory


````


#### **R5 Configuration:**

```py
! # ROUTER 5 : EIGRP
!
enable
configure terminal
hostname R5
!
! # Autonomous System (AS)
router eigrp 1
!
network 10.5.0.0 0.0.0.3
network 10.4.0.0 0.0.0.3
network 192.168.5.0 0.0.0.255
no auto-summary
exit
!
write memory

````












## 🔍 **EIGRP Metric**  

EIGRP uses a **composite metric** to determine the best path to a destination.  

This metric is calculated using multiple parameters (weights **K1 - K5**) collected from **all interfaces**.  

- ✅ The **lowest metric value** determines the **best route**.  
- ✅ **Only K1 (Bandwidth) and K3 (Delay) are used by default**.  
- ✅ **Higher bandwidth & lower delay result in better routes**.  
- ✅ **Load, Reliability, and MTU are ignored unless manually enabled**.  

### ⚙ **EIGRP Metric Components: `K-values`**  

"K" simply means "coefficient" or "weighting factor" in the EIGRP metric formula.  K-values are coefficients assigned to different parameters (such as bandwidth, delay, reliability, etc.) to determine the **weight** of each when calculating the best path.

| **K-value**  | **Parameter**     | **Description** |
|-------------|------------------|----------------|
| **K1** 🔹  | ++ **Bandwidth** (bw) | The minimum bandwidth (in Kbps) along the path. **Higher bandwidth means better routes.** _(Enabled by default)_ |
| **K2** 📉  | **Load**           | Represents how busy the link is (a value between **1-255**). **Higher load means worse performance.** _(Disabled by default)_ |
| **K3** ⏳  | ++ **Delay** (DLY)    | The cumulative delay (measured in **tens of microseconds**) along the path. **Lower delay is better.** _(Enabled by default)_ |
| **K4** ✅  | **Reliability**    | A number between **1-255**, where **255 means 100% reliability** (fewer errors). _(Disabled by default)_ |
| **K5** 📦  | **MTU**            | **Maximum Transmission Unit** size. **Not used in EIGRP calculations** (set to 0). _(Disabled by default)_ |


### 🧮 **EIGRP Metric Calculation Formula**  

By default, EIGRP only considers **Bandwidth (K1)** and **Delay (K3)** in the metric calculation:

- **`Metric` = [(K1 * Bandwidth) + (K3 * Delay)] * 256**

Since **K1 = 1** and **K3 = 1** by default, the formula simplifies to:

- **`Metric` = (Bandwidth + Delay) * 256**

**The metric in EIGRP is always a large value** because it uses **256 as a multiplier** (eg. 30720).

- If all K-values were used, the complete formula would be more complex.

### 🧮 EIGRP Metric Calculation Example:

![image](https://github.com/user-attachments/assets/8f21a179-17b0-418d-9b8d-a82fb78442a6)

1. **Less Bandwidth = 100 Mbps** _(Both Routers =)_
   Convert to Kbps:  
   `100 Mbps = 100,000 Kbps`

3. **Bandwidth Metric**: _(10^8 = 100,000,000 (used for the bandwidth calculation, and it's a fixed constant)_ 
   Formula:  
   `(10,000,000 / 100,000) * 256 = 25600`

4. **Delay Metric**:  
   For each router with 1000 µs delay (= **100 ms**), the total delay is:  
   Formula:  
   `[(1000 / 10) + (1000 / 10)] * 256 = 5120`

5. **Final Metric Calculation**:  
   Add the `bandwidth` + `delay` metrics:  
   `25600 + 5120 = 30720`

RESULT = **`30720 EIGRP Metric Calculated From Router-1`**























# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=SGKAZ-3X5kI&list=PLwAU7bA502wFCIbUUDC9tvZNl-PkKOo-u
- https://en.wikipedia.org/wiki/Enhanced_Interior_Gateway_Routing_Protocol

  
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




