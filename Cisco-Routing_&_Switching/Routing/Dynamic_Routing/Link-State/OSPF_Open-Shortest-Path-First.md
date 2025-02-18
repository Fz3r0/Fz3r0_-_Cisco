# 🚀🔄🔧 Cisco: `Dynamic Routing` > `Link State` :: `OSPF`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Dynamic Routing` `OSPF` `Link State` `Packet Tracer` `CCNA` `CCNP` `Cisco`

---

<br>

# 📖 OSPF (Open Shortest Path First)

**Open Shortest Path First (OSPF)** is a link-state routing protocol used to find the best path for packets as they pass through a set of connected networks. It was developed to overcome the limitations of earlier distance-vector protocols like RIP (Routing Information Protocol).

- OSPF was developed in 1987 by the IETF (Internet Engineering Task Force) as a solution to the limitations of RIP.
- The first OSPF RFC, RFC 1131, was released in 1989.
- It was later superseded by RFC 2328 in 1998, which is the widely implemented version today.
- OSPFv3, defined in RFC 5340, introduced support for IPv6.

OSPF is an open standard and is not proprietary. It is supported by all major networking vendors like Cisco, Juniper, and others.

- **Classless Protocol**: OSPF supports VLSM (Variable Length Subnet Mask) and CIDR (Classless Inter-Domain Routing).
- **Hierarchical Design**: OSPF supports areas to minimize the size of the routing tables and to scale more efficiently.

## ⚙️ **OSPF Features**

| Feature | Value / Description |
|---------|---------------------|
| **Protocol Number** | `89` |
| **Standard** | Open standard (OSPF v2 RFC 2328 for IPv4, OSPF v3 RFC 5340 for IPv6) |
| **Type** | Link-State (Using Bandwidthi) |
| **Algorithm** | Dijkstra’s Shortest Path First (SPF) |
| **Protocol Support** | Supports **IPv4** (RFC 2328) and **IPv6** (RFC 5340) |
| **Metric Calculation** | OSPF uses **cost** as its metric, which is based on the bandwidth of the link (lower cost = faster link). |
| **Administrative Distance (AD)** | `110` |
| **Multicast IP (SPF)** | `224.0.0.5` |
| **Multicast IP (DR/BDR)** | `224.0.0.6` |
| **Multicast MAC (SPF)** | `01:00:5E:00:00:05` |
| **Multicast MAC (DR/BDR)** | `01:00:5E:00:00:06` |
| **Equal Cost Multipath (ECMP)** | **Default:** `4`, **Maximum:** `32` |

**Notes:**

- Type: Link-State from bandwidth: se refiere a que el router busca por la ruta con el mejor "estado", hay diferentes "states" como lo es el bandwidth, el delay, nivel de errores, MTU, carga, confiabilidad, etc... OSPF al ser link.state utiliza solo uno de esos estados y es el ancho de banda.
- SPF & databases: SPF lo que hace es guardar bases de datos con la informacion de TODOS los routers que están participando en OSPF de la red, guarda la información tanto del router como de todas sus rutas, decide cual es el mejor camino comparando los routers que tiene en su base de datos. 



## 🌐 OSPF: P2P vs Multi-Access Networks 

### 🔄 **Key Differences:**

- **↔️ Point-to-Point (P2P) Networks:**

  - Direct router-to-router communication.
  - OSPF messages use multicast IP address `224.0.0.5` (Layer 3) and multicast MAC address `01:00:5E:00:00:05` (Layer 2).
  - Simple 1-to-1 communication. 

![image](https://github.com/user-attachments/assets/da47ca2b-b373-4895-a846-788096180701)
  
- **🔀 Multi-Access Networks (e.g., Using eth Switch to connect various routers):**

  - OSPF messages use a "one-to-many" model, where multiple routers share the same network.
  - Requires a **Designated Router (DR)** to manage communication, reducing overhead. 🧑‍💼
  - DR centralizes OSPF exchanges to minimize unnecessary message flooding. ⚙️

![image](https://github.com/user-attachments/assets/5e9d181a-87cf-4625-ab5e-fa6dad9a9785)

### **Roles:**

| **Role**                | **Description**                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------|
| **🏆 Designated Router (DR)** | Manages OSPF exchanges in multi-access networks, acting as the central point for OSPF communication.  |
| **🛡️ Backup DR (BDR)**    | A standby router that takes over if the DR fails, ensuring continuous OSPF operations. |

### **Multicast Communication 🔊:**

- **📡 P2P Communication:**
  - Multicast to `224.0.0.5` for direct router communication.

- **🔄 Multi-Access Communication:**
  - DR sends OSPF updates to other routers using multicast, centralizing control.


## 🌐 OSPF: Equal-Cost MultiPath (ECMP / Load Balancing)

When OSPF has multiple equal-cost routes to the same destination, and both (or more) paths have the same bandwidth, OSPF can perform **load balancing**. Technically, the traffic could be divided 50/50 between the two routes, optimizing resource utilization. 

| 🚀 Feature / Scenario     | 📝 Description                                                                             |
|-----------------------|-------------------------------------------------------------------------------------------------|
| 🔄 Load Balancing     | Splits traffic across multiple equal-cost paths for better bandwidth use and redundancy.       |
| ⚡ Maximum Paths      | Supports up to 4 paths by default, configurable up to 32 (32 are not seen in real-world scenarios).                                       |
| 🔧 Route Calculation  | Installs routes only if they have identical costs.                                              |
| 🛡️ Redundancy        | Provides automatic failover by rerouting traffic when a path fails.                            |
| 📊 Load Distribution  | Prevents congestion by spreading traffic over multiple links.                                   |
| 🧑‍💻 Router Behavior   | Uses methods like round-robin or hashing to distribute traffic over equal-cost routes.         |
| 🔍 Limitations        | Older hardware may not support ECMP, or may have lower path limits.                            |


![image](https://github.com/user-attachments/assets/210d952d-603f-4c57-8598-1d09043e7d80)








## 🌐 OSPF: Areas

Open Shortest Path First (OSPF) is a link-state routing protocol that divides networks into areas to optimize routing and reduce overhead. 

- Each router must belong to at least one OSPF area.  
- The first and most crucial area tu be used is **Area 0**, known as the **Backbone Area**.  
- A router can belong to multiple areas, but if only 1 area is used it's must be the Area 0 (Backbone).


### 🩻 OSPF Area 0 (Backbone)

- When all routers are within a single area (Area 0), they share a complete **Link-State Database (LSDB)**, they will be "BD Full" (Full Database). 
- This means they will have the complete database of every single router in the area (Area 0)
- 💀 **Warning!!!:** In large networks (e.g., 100 routers), a single area can lead to a huge LSDB, increasing CPU and memory usage.  
- Solution: **Divide the network into multiple areas** to reduce overhead.

![image](https://github.com/user-attachments/assets/ea339a48-dd40-4da6-b850-d2a024ad3e39)

### 🦴 OSPF Multi-Area Design

- All areas must connect to Area 0 (Backbone).  
- **Area Border Routers (ABRs)** connect different areas and maintain separate LSDBs for each area.  
- ABRs must be **powerful routers** because they handle multiple databases and route summarization.

![image](https://github.com/user-attachments/assets/366df1d2-83c6-4676-a358-45d2c6b4f9ab)

### 🧩 OSPF Router Types and Features by Area

| 🛡️ Router Type        | 📝 Description |
|-----------------------|---------------|
| 🟠 **Backbone Router (BR)** | Router with at least one interface in Area 0. |
| 🟡 **Internal Router (IR)** | All interfaces are in the same OSPF area. |
| 🔵 **Area Border Router (ABR)** | Connects two or more OSPF areas. Maintains multiple LSDBs. |
| 🟣 **Autonomous System Boundary Router (ASBR)** | Connects OSPF to other routing protocols (e.g., EIGRP, BGP, RIP). |

### 🌉 OSPF Area Types

| 🧩 Area Type       | 📝 Description |
|--------------------|---------------|
| **Backbone Area (Area 0)** | Core of the OSPF network. All other areas must connect here. |
| **Standard Area**  | Typical OSPF area with full routing information. |
| **Stub Area**      | Blocks external routes, reducing overhead. |
| **Totally Stubby Area** | Blocks both external and inter-area routes, only default route is allowed. |
| **NSSA (Not-So-Stubby Area)** | Allows external routes (from ASBR) but still reduces overhead. |





























## 🌐 Distance Vector VS Link-State

When discussing routing protocols, two major categories come up: **Distance Vector** and **Link-State**. Both have distinct methods of determining the best path through a network, and understanding the differences is crucial.

### 🔄 What is Link-State?

**OSPF** is considered a **Link-State** protocol

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

### 🔄 What is Distance Vector?

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


## 🌐 **Why Use OSPF?**

OSPF is a robust and efficient routing protocol designed to scale in larger networks. It is ideal for complex enterprise networks and service provider environments.

- **Efficient Routing Updates**: OSPF minimizes the amount of traffic exchanged between routers by sending updates only when there are changes in the topology.
- **Faster Convergence**: OSPF converges much faster compared to distance-vector protocols like RIP.
- **Hierarchical Structure**: By using areas, OSPF can scale in large networks with fewer routing updates and smaller routing tables.

### 🌟 **OSPF is ideal for:**

- Large enterprise networks
- Networks requiring fast convergence
- Environments with multiple routers and subnets

### 🛠️ **Examples:**

- **Large Enterprise Networks**: In networks with hundreds or thousands of routers, OSPF's ability to divide the network into areas helps reduce the size of the routing tables and optimize traffic flow.
- **Data Centers**: For highly scalable networks, OSPF ensures that the routing topology remains efficient even as the network grows.

### 🚫 **When NOT to Use OSPF?**

OSPF may not be the best choice when:

- **Small networks**: In small, simple networks, a protocol like **RIP** or even **static routing** might suffice as they are easier to configure and maintain.
- **Simple, low-overhead solution**: If you don’t need the scalability or flexibility of OSPF, **RIP** (Routing Information Protocol) might be a simpler, more appropriate option.





## 🔄 **OSPF Operation**

| Operation                                   | Description                                                                 |
|-------------------------------------------|-----------------------------------------------------------------------------|
| **Hello Protocol**                         | OSPF uses HELLO packets to establish and maintain neighbor relationships.   |
| **Uses Areas for Scalability**             | OSPF divides large networks into areas to reduce routing table size and improve efficiency. |
| **Link-State Advertisements (LSAs)**       | OSPF routers exchange LSAs to share information about the network topology. |
| **Uses Dijkstra's Algorithm**              | OSPF uses the SPF (Shortest Path First) algorithm to calculate the best paths. |
| **Flooding of LSAs**                       | OSPF floods LSAs throughout the network to ensure all routers have updated information. |
| **Supports Multiple Routing Tables**       | OSPF can support multiple routing tables for different address families (e.g., IPv4, IPv6). |
| **Hierarchical Design with Areas**         | OSPF’s hierarchical structure allows dividing networks into areas to minimize routing overhead. |
| **Supports Equal-Cost Multipath (ECMP)**   | OSPF can support up to 16 equal-cost paths for load balancing. |

⭕ **OSPF Neighbor & Link-State Database (LSDB):**

- **Neighbor Table:** Stores information about OSPF neighbors, including their state and interface details.
- **Link-State Database (LSDB):** Maintains all the Link-State Advertisements (LSAs) received from neighbors, representing the entire network topology. This is used to build the OSPF routing table.






## 📡 **OSPF Message Types**

| **Message Type**     | **Description**                                                                                     | **Transmission**       | **Reliability**    | **Opcode** | **Sequence Number** |
|----------------------|-----------------------------------------------------------------------------------------------------|------------------------|--------------------|------------|---------------------|
| **👋 Hello**          | Sent periodically (**default = 10 seconds**) to discover and maintain neighbor relationships.       | Multicast              | Unreliable         | N/A        | N/A                 |
| **🗣️ Database Description (DBD)** | Sent to exchange summary information about the LSDB between neighbors.                                | Unicast                | Reliable           | N/A        | Assigned            |
| **📜 Link-State Request (LSR)**  | Sent to request more detailed information about specific LSAs from neighbors.                         | Unicast                | Reliable           | N/A        | Assigned            |
| **📑 Link-State Update (LSU)**   | Sent to provide detailed LSA information to neighbors in response to an LSR.                        | Unicast or Multicast   | Reliable           | N/A        | Assigned            |
| **✅ Link-State Acknowledgment (LSAck)** | Sent to acknowledge the receipt of LSUs.                                                           | Unicast                | Unreliable         | N/A        | N/A                 |

- `Reliable` means the message is guaranteed to reach its destination, and the sender knows when it has been received.
- `Unreliable` means there's no guarantee the message will reach the destination, and the sender doesn't know if it was received.
- `Opcode` is not directly applicable in OSPF as the messages are categorized by their type and functionality, unlike EIGRP, which assigns specific opcodes to each message.
- `Sequence Number`: OSPF messages use sequence numbers in the LSU and LSR to track and ensure the correct order and acknowledgment of link-state advertisements.

### Hello Message

- Sent from each router on a regular interval to establish and maintain neighbor relationships.
    - The router assumes that as long as it is receiving Hello packets from a neighbor, the neighbor is still reachable and the routes remain valid.
- The interval depends on the interface bandwidth:
    - For slower connections (e.g., Frame Relay or legacy interfaces), the default Hello interval is 60 seconds with a hold time of 180 seconds.
    - For interfaces with bandwidths greater than 1.544 Mbps (such as Ethernet or T1), the default Hello interval is 10 seconds with a hold time of 40 seconds.
        - The hold time is the maximum amount of time the router will wait to receive the next Hello packet before declaring the neighbor unreachable.
        - The default hold time is 4 times the Hello interval (e.g., 10 sec Hello = 40 sec Hold Time).
        - If the hold time expires, the router considers the neighbor down and starts recalculating routes.
        - OSPF will then search for alternative routes in the LSDB to maintain network stability.

### Frame Exchange

![image](https://github.com/user-attachments/assets/ead52c1b-363b-4468-bd11-defdfef74b5c)





















































## 🔢 **OSPF: Router ID (RID)**

An **OSPF Router ID (RID)** is a unique 32-bit identifier assigned to each router within an OSPF domain. This identifier helps OSPF routers to identify and differentiate themselves in the OSPF network and plays a critical role in OSPF's routing and topology calculations.

- NOTE: The **Router ID (RID)** is used by **OSPF** to identify routers uniquely within the routing domain, enabling them to exchange routing information and maintain topology tables.

⭕ **The RID is used to uniquely identify each OSPF router within a given OSPF area or domain.** (_Different Router IDs would indicate that routers belong to separate OSPF routing instances and cannot form neighbor relationships with each other._)

⭕ **Why Do We Always Use the Same Router ID?**

- In the case of OSPF, all routers within a single organization or network that are participating in the same OSPF domain (group of routers exchanging routing information) must use the same Router ID format to form neighbor relationships and exchange routes.
- This consistency ensures that the routers recognize each other as part of the same OSPF domain and share routing information effectively.

⚠️ **IMPORTANT**: If the Router ID differs between routers, they won't recognize each other as part of the same OSPF domain, and they won't exchange routing information.

⭕ **How to Choose a Router ID?**

- The Router ID can be configured manually or automatically selected based on the router's interface IP addresses.
    - **Manual Configuration**: The router administrator can specify the Router ID.
    - **Automatic Configuration**: The router will automatically pick the highest IP address of any of its active interfaces (with the exception of loopback interfaces).
    - **Loopback Interface Preference**: If a loopback interface exists, OSPF will prefer the highest IP address of any loopback interfaces over physical interfaces.
    
### 🌍 **Router ID: Types**

- **IPv4 Router ID**: The Router ID is a 32-bit number, expressed in the same format as an IPv4 address (e.g., 192.168.1.1).
  
**Important**: **Router ID 0.0.0.0** is not valid in OSPF.

#### 🏛 **Router ID Selection Process**

| **Priority**     | **Description**                                  |
|------------------|--------------------------------------------------|
| **1st**          | **Manually configured Router ID** (highest priority). |
| **2nd**          | **Highest IP address on a loopback interface** (if loopback interfaces are configured). |
| **3rd**          | **Highest IP address on any active physical interface** (if no loopback interfaces are configured). |
| **4th**          | **Router ID 0.0.0.0** if no other ID is configured (invalid in practice). |


## 🔹 **OSPF: Administrative Distance (AD) Default Values**

| AD Type            | Value | Explanation                                                                 |
|--------------------|-------|-----------------------------------------------------------------------------|
| **Internal AD**     | 110   | Routes **within the same OSPF area** (within the same OSPF domain).          |
| **External AD**     | 170   | Routes **redistributed from other protocols** (e.g., EIGRP, RIP).            |
| **Summary AD**      | 5     | **Manually configured summary routes** (trusted due to admin configuration). |

## 🔹 **OSPF: Multicast & MAC Addresses**

| Address Type          | Value             |
|-----------------------|-------------------|
| **Multicast Address**  | `224.0.0.5`       |
| **Multicast MAC Address** | `01:00:5E:00:00:05` |

















































## 🔍 **OSPF Metric**  










## 🔍 OSPF Metric  

OSPF uses a **cost-based metric** to determine the best path to a destination.  

- ✅ The **lowest cost** determines the **best route**.  
- ✅ **Cost is based on interface bandwidth** (higher bandwidth = lower cost).  
- ✅ **Uses Dijkstra’s Shortest Path First (SPF) algorithm** for path selection.  
- ✅ **By default, OSPF does not consider delay, reliability, or load** like EIGRP does.

### ⚙ OSPF Interface Default Costs  

| **Bandwidth (Mbps)** | **Cost (Default Reference: 100 Mbps)** |
|----------------------|----------------------------------|
| **10 Gbps**         | 1 |
| **1 Gbps**          | 1 |
| **100 Mbps**        | 1 |
| **10 Mbps**         | 10 |
| **1.544 Mbps (T1)** | 64 |
| **768 Kbps**        | 133 |
| **512 Kbps**        | 195 |
| **256 Kbps**        | 390 |
| **128 Kbps**        | 780 |
| **64 Kbps**         | 1560 |

💡 **OSPF Cost = Reference Bandwidth / Interface Bandwidth**  
_Default Reference Bandwidth = 100 Mbps (can be changed with `auto-cost reference-bandwidth`)_

---

## 🧮 OSPF Metric Calculation Formula  

The OSPF cost is calculated as:  

**`Cost = Reference Bandwidth / Interface Bandwidth`**  

- The **Reference Bandwidth is 100 Mbps by default** (`100,000,000 bps`).
- Lower cost means a better path.
- If the result is a decimal, OSPF rounds **down** to the nearest integer.

### 🔢 OSPF Cost Calculation Example  

1. **GigabitEthernet (1 Gbps) link:**  
   - `Cost = 100 Mbps / 1000 Mbps = 0.1 → Rounds to 1`

2. **FastEthernet (100 Mbps) link:**  
   - `Cost = 100 Mbps / 100 Mbps = 1`

3. **T1 (1.544 Mbps) link:**  
   - `Cost = 100 Mbps / 1.544 Mbps ≈ 64`

4. **512 Kbps link:**  
   - `Cost = 100 Mbps / 0.512 Mbps ≈ 195`

💡 If high-speed links (like 1 Gbps or 10 Gbps) always show a cost of **1**,  
consider increasing the **reference bandwidth** (`auto-cost reference-bandwidth`).

---

## 📊 Metric Comparison Between Routing Protocols  



![image](https://github.com/user-attachments/assets/73f8072a-3f69-4ed5-ba3e-4ec614b0f1f8)



| **Protocol** | **Metric Used** | **Best Path Decision Criteria** |
|-------------|----------------|--------------------------------|
| **RIP**     | Hop Count      | Lowest number of hops (≤ 15) |
| **EIGRP**   | Composite Metric | Bandwidth + Delay (by default) |
| **OSPF**    | Cost          | Highest Bandwidth (Lowest Cost) |
| **IS-IS**   | Cost          | Configurable (default = 10 per link) |
| **BGP**     | Path Attributes | Complex, based on policies |

🔹 **RIP** → Prefers paths with the fewest **hops** (max **15**).  
🔹 **EIGRP** → Uses **Bandwidth & Delay** (by default) for best path selection.  
🔹 **OSPF** → Chooses path based on the **lowest cost (higher bandwidth wins)**.  
🔹 **IS-IS** → Similar to OSPF, but cost values are configurable.  
🔹 **BGP** → Uses **AS-path, Local Preference, MED, and other policies** for path selection.  

---

























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

**Hello message customization results:**
![image](https://github.com/user-attachments/assets/7811579b-9241-4d36-a771-66f57e6c4a2c)
















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
debug eigrp packet
!
!


````

### 🔎 Validation: `PC Side`

PC-side validation uses basic connectivity tests like ping and tracert to confirm end-to-end reachability.

````sh
ping 8.8.8.8

tracert 8.8.8.8

````


# Cheatsheet

![image](https://github.com/user-attachments/assets/87b03950-89c1-44f2-b5c2-c62359bab36e)

---

![image](https://github.com/user-attachments/assets/fc025835-ee93-4cba-97e5-1b660f02c4fe)







# 📚🗂️🎥 Resources

- [Distance Vector VS Link State](https://www.routexp.com/2020/03/routing-basics-distance-vector-vs-link.html)
- [OSPF a fondo](https://youtu.be/Izm-8BQLRiI?si=TgYYhEefuvJQMJFY)
  
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





