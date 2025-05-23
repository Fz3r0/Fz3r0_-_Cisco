# 🚀🔄🔧 Cisco: `Dynamic Routing` > `Distance Vector` :: `EIGRP`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Dynamic Routing` `EIGRP` `Packet Tracer` `CCNA` `CCNP` `Cisco`

---

<br>

# 📖 EIGRP (Enhanced Interior Gateway Routing Protocol)

**Enhanced Interior Gateway Routing Protocol (EIGRP)** is a Cisco proprietary routing protocol that combines the best features of distance-vector and link-state protocols. It is a hybrid protocol, offering faster convergence and lower bandwidth usage compared to other distance-vector protocols like RIP.

The protocol was designed by Cisco Systems in 1992 as a proprietary protocol, available only on Cisco routers. In 2013, Cisco permitted other vendors to freely implement a limited version of EIGRP with some of its associated features such as High Availability (HA), while withholding other EIGRP features such as EIGRP stub, needed for DMVPN and large-scale campus deployment.

- **Hybrid Distance Vector Routing Protocol**: Called "hybrid" because it combines features of **distance vector** and **link-state** protocols. <br><br>
- **Successor of IGRP** (Deprecated): IGRP is no longer used because it had **slower convergence**, used **less efficient metrics** and EIGRP became open standard in 2016 (RFC 7868).  

## ⚙️ **EIGRP Features**

| Feature | Value / Description |
|---------|---------------------|
| **Protocol Number** | `88` |
| **Standard** | Originally **proprietary** (Cisco), later **open standard** (RFC 7868) |
| **Type** | **Distance Vector (Hybrid)** protocol |
| **Algorithm** | Uses **DUAL (Diffusing Update Algorithm)** for fast convergence |
| **Protocol Support** | Supports **IPv4, IPv6**, and **legacy protocols** (IPX, AppleTalk) |
| **Metric Calculation** | Based on **Bandwidth + Delay** |
| **Administrative Distance (AD)** | **Internal:** `90` → Routes **within the same AS (Autonomous System)** <br> **External:** `170` → Routes **redistributed from other protocols (OSPF, RIP, etc.)** <br> **Summary:** `5` → **Manually configured summary routes** (trusted due to admin configuration) |
| **Multicast Address** | `224.0.0.10` |
| **Multicast MAC Address** | `01:00:5E:00:00:0A` |

## 🌐 Distance Vector VS Link-State

When discussing routing protocols, two major categories come up: **Distance Vector** and **Link-State**. Both have distinct methods of determining the best path through a network, and understanding the differences is crucial.

### 🔄 What is Distance Vector?

**EIGRP** is considered a **Distance Vector** protocol, however, it's called **Hybrid** because it incorporates **Link-State-like** features

- Unlike Link-State protocols (such as OSPF), Distance Vector focuses on **hop count or cumulative distance** rather than the state of links.  
- The key factor it considers when determining the best path is **the total metric** (e.g., hop count in RIP or composite metric in EIGRP).  
- Distance Vector protocols rely on **"routing-by-rumor"**, meaning routers exchange information only with their directly connected neighbors rather than maintaining a full network topology.  

![image](https://github.com/user-attachments/assets/1026df31-adaf-45b8-a9b7-cb6aec0eb24f)

RIP is a classic **Distance Vector / Dynamic Routing** protocol, which means:  

- Each router only knows the **next-hop and cost** to reach a destination but does not have a complete network view.  
- Routers periodically send updates to their **direct neighbors**, which propagate throughout the network.  
- **Slower convergence** and **prone to routing loops**, mitigated by features like **split horizon, route poisoning, and hold-down timers**.  

### 🛠️ Key Characteristics of Distance Vector Protocols:  

| Feature | Distance Vector |
|---------|----------------|
| **Routing Updates** | Periodic updates to neighbors only |
| **Network Knowledge** | Limited (routers only know next-hop info) |
| **Convergence Time** | Slower than Link-State |
| **Loop Prevention** | Split horizon, route poisoning, hold-down timers |
| **Example Protocols** | RIP, EIGRP (Hybrid) |

**NOTE:** EIGRP is technically a **Distance Vector protocol**, but it includes **Link-State-like optimizations**, such as advanced metric calculations (**K-values**) and topology tables, which is why it's called **Hybrid**.

### 🔄 What is Link-State?

- Unlike Distance Vector protocols (such as RIP), Link-State focuses on the **state of the links** rather than the distance.
- The key factor it considers when determining the best path is **`Bandwidth (BW)`** value. (_EIGRP can use up to 5 values, including bandwidth, while OSPF only uses bandwidth. That's why EIGRP has a lower Administrative Distance (AD) than OSPF._)  
- This means that if there are two possible paths, it will prefer the one with the highest total bandwidth, even if it has more physical hops.

![image](https://github.com/user-attachments/assets/876ec9eb-db13-4eb9-8174-097fb1b22e06)

OSPF is considered a **Link State / Dynamic Routing** protocol, it means that **each router in a network keeps an updated map of the entire network in a database, including each router and each route of the network**. 

- When a change occurs, like a new route or a failure, routers share this updated information with all others, so every router has the same view of the network.
- This helps routers calculate the best paths efficiently and react quickly to changes.

### 🛠️ Key Characteristics of Link-State Protocols:

| Feature                  | Link-State                                 |
|--------------------------|--------------------------------------------|
| **Routing Updates**       | Immediate updates sent to all routers in the network |
| **Network Knowledge**     | Full (routers have a complete map of the network) |
| **Convergence Time**      | Faster than Distance Vector                |
| **Loop Prevention**       | Uses SPF (Shortest Path First) algorithm to calculate best paths |
| **Example Protocols**     | OSPF, IS-IS                               |

**NOTE:** EIGRP is considered a **Distance Vector** protocol because it relies on routing-by-rumor, meaning routers exchange information only with their directly connected neighbors rather than having a complete network topology like OSPF. However, it's called **Hybrid** because it incorporates **Link-State-like** features, for example: Supporting advanced metrics (weights K1-K5) for better path selection.



## 🌐 **Why Use EIGRP?** 

EIGRP is used primarily in larger, more complex networks because of its ability to scale efficiently and support advanced features, such as:

- **Fast convergence**: Reacts quickly to changes in the network.
- **Efficient use of bandwidth**: EIGRP sends updates only when a change occurs, rather than periodically.
- **Supports multiple network layer protocols**: Works with both IPv4 and IPv6.
- **Automatic summarization** (in earlier versions) and **manual summarization**.

⭕ **EIGRP is ideal for:**

- Medium to large networks with Cisco routers.
- Networks requiring fast convergence (faster than RIP).
- Scenarios where IP address summarization and route aggregation are needed.

⭕ **Examples:**

- **Enterprise Networks**: Large companies using EIGRP for internal routing across multiple office locations.
- **Data Centers**: EIGRP is widely used to ensure optimal routing and fast recovery from link failures.
  
### 🚫 **When NOT to Use EIGRP?** 

EIGRP may not be the best choice when:

- Working in **multi-vendor environments** (since it's Cisco proprietary).
- You need a simple, low-overhead solution (**RIPv2** might be enough).
- Extremely small networks where simpler protocols like **static routing** may be sufficient.



## 🔄 **EIGRP Operation**

| Operation                                   | Description                                                                 |
|-------------------------------------------|-----------------------------------------------------------------------------|
| **RTP (Reliable Transport Protocol)**     | EIGRP does not use TCP or UDP. Uses its own protocol (RTP).                 |
| **Uses Autonomous System (AS) numbers**   | Uses AS numbers for activation.                                        |
| **Sends HELLO messages**                  | Sends HELLO messages to maintain neighbor relationships.               |
| **Uses separate modules for each routed protocol** | EIGRP uses PDM (Protocol Dependent Modules) for different protocols. |
| **Limited updates**                       | Sends updates only when changes occur in the network.                  |
| **Maximum hop count**                     | Supports up to 255 hops (100 by default).                             |
| **Supports manual summarization**         | Supports manual summarization on required interfaces.                 |

⭕ **Neighbor & Topology Tables:**

- **Neighbor Table:** Stores information about directly connected routers.
- **Topology Table:** Maintains all learned routes, including successors and feasible successors.

## EIGRP Packets and Frame Exchange

### 📡 **EIGRP Message Types**

| **Message Type** | **Number** | **Description**                                                                                     | **Transmission**       | **Reliability**    | **Opcode** | **Sequence Number** |
|------------------|------------|-----------------------------------------------------------------------------------------------------|------------------------|--------------------|------------|---------------------|
| **🆙 Update**     | 1          | Sent to share routing information with neighbors. It includes new routes or updates to existing ones. | Multicast or Unicast    | Reliable           | 1          | Assigned            |
| **❓ Query**      | 3          | Sent to request routing information from a neighbor when there is no valid route in the routing table. | Multicast or Unicast    | Reliable           | 3          | Assigned            |
| **💬 Reply**      | 4          | Sent in response to a Query message, providing the requested routing information.                     | Unicast                | Reliable           | 4          | Assigned            |
| **👋 Hello**      | 5          | Sent periodically (**default = 5 seconds**) to maintain and establish neighbor relationships. It includes information about the router and its capabilities. | Multicast              | Unreliable         | 5          | N/A                 |
| **✅ ACK**        | 8          | Acknowledgment message sent to confirm the receipt of routing updates, queries, or replies.           | Unicast                | Unreliable         | 5          | N/A                 |

![image](https://github.com/user-attachments/assets/15b3dd04-d28f-42fd-ae8f-c5c5fd2b244b)

- `Reliable` means the message is guaranteed to reach its destination, and the sender knows when it has been received.
- `Unreliable` means there's no guarantee the message will reach the destination, and the sender doesn't know if it was received.
- `Opcode` is a number that identifies the type of EIGRP message being sent. Each type of message (Update, Query, Reply, Hello/ACK) has a specific opcode number to distinguish it from others.
- `Sequence Number` indicates the order or tracking of messages in EIGRP. Assigned means that the message has a unique identifier (sequence number) to track it. This helps ensure the messages are processed in the correct order and can be re-ordered if necessary. For example, Hello and ACK messages don't need sequence numbers because they are not part of the ordered flow of information like Update, Query, or Reply messages.

### Hello Message

- Sent from each reuter on regular interval
    - reouter asseumes that as long its receiven heelo packets from neighbot trhe neightbo and routes still viable 
- The intergavc depen on interface bandwith
    - low bandwitdg (frame relay or other legacy interfaces) = 60 secons hello interval / defaul hold time = 180 sec
    - greater than 1.544mbs like ethernet or T1  = 5 secons hello interval / defaul hold time = 15 sec
        - hold time  is the maxuimum time the router shoudl wait to recienve the next hello before declaring that neighbor as unrechable
        - The default hodl time is 3 times the hello interfave, eg, 5 sec = 15 sec hold (5*3)
        - If hold time expires EIGRP declares route as down
        - DUAL () searches for a new path in the topology table or byu sending out queries. 

### Frame Exchange

![image](https://github.com/user-attachments/assets/f2fbd6c4-92aa-4217-a69f-066fa8d346c9)


## 🔢 EIGRP: `Autonomous System (AS) Number (ASN)`

An **Autonomous System (AS) Number (ASN)** is a unique identifier assigned to a collection of IP networks and routers that participate in **BGP** or **EIGRP**, under the control of a single organization that presents a common routing policy to the internet. These numbers allow different networks to communicate with each other on the internet by enabling routing decisions between them. 
 
- NOTE: Each AS is identified by a **unique number**, called an **AS Number (ASN)**, which is used for **BRP** or **EIGRP**, to identify and differentiate between different networks on the internet or within a private network.**.  

⭕ **The same AS number is used to group routers together so they can exchange routing information.** (_Different AS numbers would indicate that routers belong to separate routing domains, and they will not form neighbor relationships with each other._)

⭕ **Why Do We Always Use the Same AS Number?**

- In the case of EIGRP, all routers within a single organization or network that are participating in the same routing domain (group of routers exchanging routing information) must use the same AS number to form neighbor relationships and exchange routes.
- This consistency ensures that the routers recognize each other as part of the same routing domain and share routing information effectively.

⚠️ **IMPORTANT**: If the AS number differs between routers, they won't recognize each other as part of the same network, and they won't exchange routing information.

⭕ How to Choose an AS Number?

- In private networks, you can pick any AS number that you want (e.g., router eigrp 100 or router eigrp 200 or router eigrp 666).
- In large enterprises, different AS numbers may be used to separate different routing domains.
- In EIGRP Named Mode, the AS number is still required but used differently.

### 🌍 ASN: Types

- **2-byte AS numbers**: The original AS numbers (1 - 64511) have been **completely assigned**, necessitating the introduction of a larger range. 
- **4-byte AS numbers**: These are **compatible with 2-byte ASNs** and were introduced to **support future growth**.

**Note**: AS **23456** is reserved. When **BGP peers do not support 4-byte ASNs**, they use **AS 23456** as a **fallback** to maintain compatibility. 

**Important**: **ASN 0 is not valid in EIGRP.**

#### 🏛 **2-Byte (16-bit) AS Numbers**

| **Type**     | **Range**      | **Description** |
|-------------|--------------|----------------|
| **Public AS**  | `1 - 64511`  | Used by **ISPs and large organizations** for **global** routing. _(All public ASNs in this range are already allocated.)_ |
| **Private AS** | `64512 - 65535` | Used **internally** within organizations (not advertised on the internet). |

####  🚀 **4-Byte (32-bit) AS Numbers**  

| **Type**     | **Range**         | **Description** |
|-------------|-----------------|----------------|
| **Public AS**  | `65536 - 4294967296` | Used for **global internet routing** (similar to old 2-byte ASNs but with a much larger pool). |
| **Private AS** | `64512 - 65535` | Same range as in 2-byte ASNs, **reserved for internal use**. |
| **Reserved AS** | `23456` | **Placeholder ASN** used for backward compatibility between 2-byte and 4-byte ASNs. |

## 🔹 **Administrative Distance (AD) Default Values**

| AD Type | Value | Explanation |
|---------|-------|------------|
| **Internal AD** | 90 | Routes **within the same AS (Autonomous System)** |
| **External AD** | 170 | Routes **redistributed from other protocols (OSPF, RIP, etc.)** |
| **Summary AD** | 5 | **Manually configured summary routes** (trusted due to admin configuration) |





## 🔍 **EIGRP Metric**  

EIGRP uses a **composite metric** to determine the best path to a destination.  

This metric is calculated using multiple parameters (weights **K1 - K5**) collected from **all interfaces**.  

- ✅ The **lowest metric value** determines the **best route**.  
- ✅ **Only K1 (Bandwidth) and K3 (Delay) are used by default**.  
- ✅ **Higher bandwidth & lower delay result in better routes**.  
- ✅ **Load, Reliability, and MTU are ignored unless manually enabled**.

### EIGRP Interface Default Metrics

![image](https://github.com/user-attachments/assets/2cb1adfd-2efb-444b-9c89-9eac6ec520a5)
  
### ⚙ **EIGRP Metric Components: `K-values`**  

"K" simply means "coefficient" or "weighting factor" in the EIGRP metric formula.  K-values are coefficients assigned to different parameters (such as bandwidth, delay, reliability, etc.) to determine the **weight** of each when calculating the best path.

| **K-value** | **Parameter**       | **Description**                                                                 | **Default State**         | **Default K-value** |
|-------------|---------------------|---------------------------------------------------------------------------------|---------------------|-------------|
| **K1** 🔹   | ++ **Bandwidth** (bw) | The minimum bandwidth (in Kbps) along the path. **Higher bandwidth means better routes.** | **Enabled by default** | **1**       |
| **K2** 📉   | **Load**             | Represents how busy the link is (a value between **1-255**). **Higher load means worse performance.** | _Disabled by default_ | **0**       |
| **K3** ⏳   | ++ **Delay** (DLY)   | The cumulative delay (measured in **tens of microseconds**) along the path. **Lower delay is better.** | **Enabled by default** | **1**       |
| **K4** ✅   | **Reliability**      | A number between **1-255**, where **255 means 100% reliability** (fewer errors). | _Disabled by default_ | **0**       |
| **K5** 📦   | **MTU**              | **Maximum Transmission Unit** size. **Not used in EIGRP calculations** (set to 0). | _Disabled by default_ | **0**       |

![image](https://github.com/user-attachments/assets/1dd630c4-692c-4b81-a5c1-80ee72783a45)

### 🧮 **EIGRP Metric Calculation Formula**  

By default, EIGRP only considers **Bandwidth (K1)** and **Delay (K3)** in the metric calculation:

- **`Metric` = [(K1 * Bandwidth) + (K3 * Delay)] * 256**
Since **K1 = 1** and **K3 = 1** by default, the formula simplifies to:

- **`Metric` = (Bandwidth + Delay) * 256**
**The metric in EIGRP is always a large value** because it uses **256 as a multiplier** (eg. 30720).

- If all K-values were used, the complete formula would be more complex.

![image](https://github.com/user-attachments/assets/849393f5-83a2-4b3b-b747-435808ec2aa5)

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






# ⚙️ EIGRP Configuration @ `Packet Tracer`

This lab focuses on configuring and validating EIGRP (Enhanced Interior Gateway Routing Protocol) in a five-router ring topology using Cisco Packet Tracer. 

## Lab Files

- [Download Cisco Packet Tracer Fz3r0 Labs :: **Dynamic Route x5 Routers : `EIGRP`**]()

## Dynamic Routing Topology: `Fz3r0 Default 5 Ring Router Topology`

This setup consists of **five routers** (R1 to R5) connected in a **ring topology**, each with:

- **Two WAN interfaces** using `/30 subnets` (point-to-point links between routers).
- **One LAN interface** using `/24 subnets` (local network per router).
- **Each LAN** have a PC for testing purposes.
- **There's an additional Router-WAN to simulate Internet/Google connection to 8.8.8.8**

![image](https://github.com/user-attachments/assets/92c1d2a6-53be-4a8a-8adb-9dbca0a0edd5)

## 📋 **IP Addressing Table**

| Device    | Interface | IP Address   | Subnet Mask      | Network Address with CIDR |
|-----------|-----------|--------------|------------------|---------------------------|
| **R1**    | Fa0/0     | 10.1.0.1      | 255.255.255.252 | 10.1.0.0/30               |
|           | Fa0/1     | 10.5.0.2      | 255.255.255.252 | 10.5.0.0/30               |
|           | Fa1/1     | 192.168.1.1   | 255.255.255.0   | 192.168.1.0/24            |
| **R2**    | Fa0/0     | 10.2.0.1      | 255.255.255.252 | 10.2.0.0/30               |
|           | Fa0/1     | 10.1.0.2      | 255.255.255.252 | 10.1.0.0/30               |
|           | Fa1/1     | 192.168.2.1   | 255.255.255.0   | 192.168.2.0/24            |
| **R3**    | Fa0/0     | 10.3.0.1      | 255.255.255.252 | 10.3.0.0/30               |
|           | Fa0/1     | 10.2.0.2      | 255.255.255.252 | 10.2.0.0/30               |
|           | Fa1/0     | 200.1.1.1     | 255.255.255.252 | 200.1.1.1/30              |
|           | Fa1/1     | 192.168.3.1   | 255.255.255.0   | 192.168.3.0/24            |
| **R4**    | Fa0/0     | 10.4.0.1      | 255.255.255.252 | 10.4.0.0/30               |
|           | Fa0/1     | 10.3.0.2      | 255.255.255.252 | 10.3.0.0/30               |
|           | Fa1/1     | 192.168.4.1   | 255.255.255.0   | 192.168.4.0/24            |
| **R5**    | Fa0/0     | 10.5.0.1      | 255.255.255.252 | 10.5.0.0/30               |
|           | Fa0/1     | 10.4.0.2      | 255.255.255.252 | 10.4.0.0/30               |
|           | Fa1/1     | 192.168.5.1   | 255.255.255.0   | 192.168.5.0/24            |
| **R-WAN** | Fa0/0     | 200.1.1.2     | 255.255.255.252 | 200.1.1.1/30              |
|           | Lo0       | 8.8.8.8       | 255.255.255.255 | 8.8.8.8/32                |

- `PC-1 (Site-A)` :: 192.168.1.100/24
- `PC-2 (Site-B)` :: 192.168.2.100/24
- `PC-3 (Site-C)` :: 192.168.3.100/24
- `PC-4 (Site-D)` :: 192.168.4.100/24
- `PC-5 (Site-E)` :: 192.168.5.100/24
  

## Routing Topology: `Init Configuration`

This configuration solely establishes IP addressing for each router’s interfaces and their respective subnets. 

- **NO dynamic or static routing configurations are applied at this stage!!!** _The only exception is a default route on the Internet router (R-WAN), ensuring it is ready when EIGRP routing is later implemented across the network. Each router is assigned appropriate WAN and LAN IP addresses, but no routing protocols are active yet._

The following setup ensures all routers have their interfaces configured and operational, providing a foundation for future any routing protocol deployment:

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
! ## INTERNET
!
interface fa 1/0
ip address 200.1.1.1 255.255.255.252
no shutdown
exit
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

### Init Setup: `Router WAN (Internet)`

````py
! ## ROUTER WAN (Internet)
!
enable
configure terminal
!
hostname R-WAN
!
! ## No sumarize
!
! no sumarize
!
! ## WAN SIDE
!
interface fa 0/0
ip address 200.1.1.2 255.255.255.252
no shutdown
exit
!
! ## Loopback Interface (Simulating Google)
!
interface Loopback0
ip address 8.8.8.8 255.255.255.255
exit
!
! ! ## Default Route (To reach EIGRP network (next hop interface))
!
ip route 0.0.0.0 0.0.0.0 200.1.1.1
!
! ## Save & Check Configuration
!
end
write memory
!
show ip interface brief
!


````


## ⚙️ **EIGRP Configuration**

### ⚙️✅ **Basic EIGRP Configuration Steps:**

1. 🛠 **Create the EIGRP process by defining an Autonomous System Number (ASN).**  

   - This number identifies the EIGRP routing domain and must match across all routers in the same EIGRP network.

2. 🚫 **Disable automatic summarization.**  

   - By default, EIGRP summarizes routes at major network boundaries (classful networks).  
   - Disabling this ensures EIGRP advertises subnet information correctly.

3. 🌐 **Declare the IP addresses or networks for EIGRP messaging.**  

   - These are the networks where EIGRP will send and receive routing updates.  
   - Usually, you specify the subnet, e.g., `10.10.0.0/30`, to include all relevant interfaces.  
   - ⚠️ **`IMPORTANT`**: You can use the trick of `network 0.0.0.0` command and EIGRP will "magically" set up on all "up" interfaces! _(Use only in controlled environments)_

4. 📡 **Disable EIGRP messaging on LAN access interfaces (Switches/PCs).**  

   - ⚠️ **`IMPORTANT`**: EIGRP must be enabled on LAN interfaces, just messages will be turned off for LAN devices.  
   - This is a best practice because LAN devices do not participate in routing, only the router interface 😉.  
   - Disabling it saves bandwidth and prevents security risks, such as unauthorized prefix injection or traffic redirection.

5. 🌍 **Inject a default route in the border router (if there is a WAN/Internet connection).**  

   - This is useful for routers that need a default path to external networks.  
   - Example: `0.0.0.0/0` with a next-hop neighbor.

6. 📏 **Manually summarize routes when needed.**  

   - Manual summarization can help optimize routing tables and reduce unnecessary updates.

---

### ⚙️🔀 **Optional EIGRP Configuration Adjustments:**

- ⏳ **Adjust Hello (default: 5 sec) and Hold (default: 15 sec) timers.**  

  - **Purpose:** Controls how often routers send Hello packets and how long they wait before declaring a neighbor down.  
  - **Use case:** Reducing Hello intervals (e.g., from 5s to 1s) allows for faster neighbor failure detection, useful in high-speed or critical networks. Increasing timers can reduce control traffic in stable environments.

- ⚖️ **Modify K-values (default weights).**  

  - **Purpose:** Adjusts how EIGRP calculates the best path using different metrics like bandwidth, delay, reliability, and load.  
  - **Use case:** Tweaking K-values can prioritize certain paths based on delay instead of bandwidth, or ignore reliability/load in path selection. ⚠️ **Must be consistent across all routers** to avoid neighbor mismatches.

- 🔢 **Set the maximum number of hops (default: 100).**  

  - **Purpose:** Defines how far EIGRP routes can propagate before being considered unreachable.  
  - **Use case:** Increasing this value allows larger networks to function properly, while lowering it can restrict route propagation in controlled environments.

- 🚦 **Limit the percentage of bandwidth used per interface (default: 50%).**  

  - **Purpose:** Prevents EIGRP from consuming too much link bandwidth, ensuring normal traffic is not disrupted.  
  - **Use case:** Reducing the bandwidth percentage (e.g., from 50% to 25%) is useful on slow or shared links to prevent routing updates from congesting the network.

- 🔐 **Authenticate EIGRP messages between neighbors (security feature).**  

  - **Purpose:** Ensures only trusted routers can exchange EIGRP messages, preventing unauthorized devices from injecting false routes.  
  - **Use case:** Essential in enterprise and ISP networks to prevent routing attacks or misconfigurations. Uses MD5 or SHA authentication.



## ⚡ EIGRP Configuration: `Basic / Mandatory`

Before diving into more advanced optimizations, we must first establish a functional EIGRP configuration. This section outlines the essential steps required to set up EIGRP across multiple routers, ensuring proper routing communication within the network.

To ensure a stable and predictable network, we will follow these fundamental steps:

1. Define an EIGRP Autonomous System Number (ASN).
2. Disable automatic route summarization.
3. Enable EIGRP on relevant interfaces.
4. Suppress EIGRP messages on LAN-facing interfaces.
5.  Advertise a default route if required.
6. Implement manual route summarization when needed.

In this example, we will configure EIGRP between five routers (R1 to R5) in the ring topology, as defined in the setup:

### **R1 Configuration:**

```py
! # ROUTER 1 : EIGRP
!
enable
configure terminal
hostname R1
!
! # Step 1: Autonomous System (AS)
router eigrp 666
!
! # Step 2: Disables automatic summarization.
no auto-summary
!
! # Step 3: Enable EIGRP on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.1.0.0 0.0.0.3
network 10.5.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.1.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.1.0 to 192.168.1.255"
network 192.168.1.0 0.0.0.255
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface fa 1/1
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! # Exit & Save Configurations
end
write memory
!
!

```

### **R2 Configuration:**

```py
! # ROUTER 2 : EIGRP
!
enable
configure terminal
hostname R2
!
! # Step 1: Autonomous System (AS)
router eigrp 666
!
! # Step 2: Disables automatic summarization.
no auto-summary
!
! # Step 3: Enable EIGRP on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.2.0.0 0.0.0.3
network 10.1.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.1.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.1.0 to 192.168.1.255"
network 192.168.2.0 0.0.0.255
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface fa 1/1
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! # Exit & Save Configurations
end
write memory
!
!

```


#### **R3 Configuration:**

```py
! # ROUTER 3 : EIGRP
!
enable
configure terminal
hostname R3
!
! # Step 1: Autonomous System (AS)
router eigrp 666
!
! # Step 2: Disables automatic summarization.
no auto-summary
!
! # Step 3: Enable EIGRP on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.3.0.0 0.0.0.3
network 10.2.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.1.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.1.0 to 192.168.1.255"
network 192.168.3.0 0.0.0.255
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface fa 1/1
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
ip route 0.0.0.0 0.0.0.0 200.1.1.2
router eigrp 666
redistribute static
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! # Exit & Save Configurations
end
write memory
!
!

```


#### **R4 Configuration:**

```py
! # ROUTER 4 : EIGRP
!
enable
configure terminal
hostname R4
!
! # Step 1: Autonomous System (AS)
router eigrp 666
!
! # Step 2: Disables automatic summarization.
no auto-summary
!
! # Step 3: Enable EIGRP on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.4.0.0 0.0.0.3
network 10.3.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.1.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.1.0 to 192.168.1.255"
network 192.168.4.0 0.0.0.255
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface fa 1/1
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! # Exit & Save Configurations
end
write memory
!
!

```


#### **R5 Configuration:**

```py
! # ROUTER 5 : EIGRP
!
enable
configure terminal
hostname R5
!
! # Step 1: Autonomous System (AS)
router eigrp 666
!
! # Step 2: Disables automatic summarization.
no auto-summary
!
! # Step 3: Enable EIGRP on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.5.0.0 0.0.0.3
network 10.4.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.1.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.1.0 to 192.168.1.255"
network 192.168.5.0 0.0.0.255
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface fa 1/1
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! # Exit & Save Configurations
end
write memory
!
!


```

## ⚡ EIGRP Configuration: `Optional`

While EIGRP works efficiently with its default settings, certain adjustments can enhance stability, security, and performance based on network requirements. 

- The following optional configurations allow for **better control over neighbor relationships, routing calculations, bandwidth usage, and security**. 

Proper tuning ensures EIGRP adapts to various environments, from high-speed critical networks to large-scale enterprise deployments.

### EIGRP Optional Configuration: `Hello Message Timers`

- **This must be done on both sides, e.g., `R5` & `R4`, as they need to be consistent on the same link**

````py
! ## EIGRP :: Optional Configurations :: Hello Message Customization
!
enable
configure terminal
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!     ## SELECT INTERFACE:
interface fa 0/0
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!     ## MODIFY HELLO MESSAGE:
!
!      # EIGRP Best Practices recommend setting Hello to 1 sec and Hold to 3 sec.
!      # (This must be done on both sides, e.g., R5 & R4, as they need to be consistent on the same link).
!
! ## Modify Hello interval (Default 5 seconds >> New 1 second)
ip hello-interval eigrp 666 1
!
! ## Modify Hold time (Default 15 seconds >> New 3 seconds (3x the Hello time))
ip hold-time eigrp 666 3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!     ## VERIFY CONFIGURATION AND SAVE:
!
end
!
write memory
!
show run interface fa 0/0
!

````

















## ✅ Validation

Verifying EIGRP functionality is crucial to maintaining a stable and efficient routing environment. Validation steps help confirm neighbor relationships, routing table accuracy, traffic statistics, and overall protocol performance.

### 🔎 Validation: `Router Side`

Router-side validation includes essential show and debug commands to inspect EIGRP neighbors, interfaces, topology, and route propagation.

````py
! ## EIGRP Validation @ Router Side by Fz3r0
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show ip eigrp neighbors
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show ip eigrp interfaces
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show ip eigrp traffic
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show ip eigrp topology
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show ip route eigrp
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show ip protocols
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show ip route
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
show run | se router eigrp
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
! # Check load balance for 0.0.0.0/0 (Internet) :: Router 8 = load balance
show ip cef 0.0.0.0/0
!
! # Check route to 0.0.0.0/0 (Internet) :: Router 8 = 2 routes same metric
show ip route 0.0.0.0
!
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
! # Debug EIGRP:
!debug eigrp packet
!no debug eigrp packet
!




````

### 🔎 Validation: `PC Side`

PC-side validation uses basic connectivity tests like ping and tracert to confirm end-to-end reachability.

````sh
ping 8.8.8.8

tracert 8.8.8.8

![image](https://github.com/user-attachments/assets/aa4061c6-bb30-4ec3-b086-8bd00668cd5c)


````






# Cheatsheet

![image](https://github.com/user-attachments/assets/a80dbeab-b993-4790-afc3-1cdc35353081)



# 📚🗂️🎥 Resources

- [EIGRP Explained | Step by Step](https://www.youtube.com/watch?v=QyymlFWDEgM)
- https://www.youtube.com/watch?v=SGKAZ-3X5kI&list=PLwAU7bA502wFCIbUUDC9tvZNl-PkKOo-u
- https://en.wikipedia.org/wiki/Enhanced_Interior_Gateway_Routing_Protocol
- https://www.youtube.com/watch?v=Ih4vSDbhRc4&t=240s
- [EIGRP Packets](https://www.youtube.com/watch?v=Vf7sJVXRkSo)
- [EIGRP Metrics](https://www.youtube.com/watch?v=bGGJYPng6jQ)
- https://ipcisco.com/lesson/eigrp-configuration-with-packet-tracer-ccnp/
- [Tabla de Topología EIGRP](https://www.youtube.com/watch?v=95l2pArcgac&list=PLwAU7bA502wFCIbUUDC9tvZNl-PkKOo-u&index=7)

  
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




