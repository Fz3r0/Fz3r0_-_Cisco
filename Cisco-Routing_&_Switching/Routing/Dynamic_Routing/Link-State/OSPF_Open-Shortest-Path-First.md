# 🚀🔄🔧 Cisco: `Dynamic Routing` > `Link State` :: `OSPF`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### **Keywords**: `Routing` `Dynamic Routing` `LSR` `Link State` `IGP` `Interior Gateway Protocol` `OSPF` `OSPFv2` `OSPFv3` `Open Shortest Path First` `RFC 1131` `RFC 2328` `RFC 5340` `Packet Tracer` `EVE-NG` `CCNA` `CCNP` `Cisco`

---

<br>

<div align="center">

**[Black screen. Silence. Suddenly, streams of neon-green code flood the void like a digital torrent.]**  

**[SYNTHETIC VOICE]:** *"Network instability detected... Routing convergence failing... Autonomous systems collapse imminent..."*  

*[A crimson flash illuminates the dark. A lone figure stands before a glowing network hologram. Packets of data swirl like embers in the void.]*  

*[Zoom into a virtual router—its interface ignites, protocols recalculating at the speed of light.]*  

**"Link-state packets flooding the grid… LSA propagation expanding… The backbone ignites."**  

*[Routers link, glowing lines weave a living, breathing network.]*  

**"It sees. It adapts. It converges. And it never stops. Fueled by a relentless algorithm and mathematical precision"**  

**"HE is the backbone. HE is the protocol. HE... is OSPF."**  

*[A distorted bass rumbles. Data pulses. Routers hum to life. The screen fades to black.]*  

*[Final message flickers in glowing green:]*  

**_INITIALIZING SEQUENCE..._** <br>  
**_OSPF ACTIVE..._** <br>  
**_SYSTEMS ONLINE..._** <br>  
**_ALL SYSTEMS GO!_**  <br>  

</div>

<div align="right">

_By Fz3r0 💀🎩_

</div>

<br> <br>


# 📑📚📖`Index`

- [**OSPF (Open Shortest Path First)**]()
    - [Why Use OSPF?]()
    - [OSPF is ideal for]()
    - [OSPF is NOT ideal for]() 
- [**OSPF Features**]()
- [**OSPF Operation**]()



# 🌐🔄🖧 OSPF (Open Shortest Path First)

**Open Shortest Path First (OSPF)** is a **link-state** routing protocol for Internet Protocol (IP) networks used to find the best path for packets _(based on the **bandwidth** of the link)_ as they pass through a set of connected networks. It uses a **Link State Routing (LSR)** algorithm and falls into the group of **Interior Gateway Protocols (IGPs)**, operating within a single autonomous system (AS). It was developed to overcome the limitations of earlier distance-vector protocols like RIP (Routing Information Protocol).

- OSPF was developed in 1987 by the IETF (Internet Engineering Task Force) as a solution to the limitations of RIP.
- The first OSPF - RFC 1131, was released in 1989.
- It was later superseded by OSPFv2 - RFC 2328 in 1998, which is the widely implemented version today.
- OSPFv3, defined in RFC 5340, introduced support for IPv6.

<p align="center"> <img src="https://github.com/user-attachments/assets/864dc06c-8b7f-4390-a44a-0a1281d26e46" height="200"> 

OSPF is an open standard and is not proprietary. It is supported by all major networking vendors like Cisco, Juniper, and others.

- **Classless Protocol**: OSPF supports VLSM (Variable Length Subnet Mask) and CIDR (Classless Inter-Domain Routing).
- **Hierarchical Design**: OSPF supports areas _(OSPF Areas)_ to minimize the size of the routing tables and to scale more efficiently.

## ❓ **Why Use OSPF?**

OSPF is a robust and efficient routing protocol designed to scale in larger networks. It is ideal for complex enterprise networks and service provider environments.

- **Efficient Routing Updates**: OSPF minimizes the amount of traffic exchanged between routers by sending updates only when there are changes in the topology.
- **Faster Convergence**: OSPF converges much faster compared to distance-vector protocols like RIP.
- **Hierarchical Structure**: By using areas, OSPF can scale in large networks with fewer routing updates and smaller routing tables.

### ✅ **OSPF is ideal for:**

- Large enterprise networks
- Networks requiring fast convergence
- Environments with multiple routers and subnets

**Examples:**

- **Large Enterprise Networks**: In networks with hundreds or thousands of routers, OSPF's ability to divide the network into areas helps reduce the size of the routing tables and optimize traffic flow.
- **Data Centers**: For highly scalable networks, OSPF ensures that the routing topology remains efficient even as the network grows.

### 🚫 **OSPF is NOT ideal for:**

- **Small networks**: In small, simple networks, a protocol like **RIP** or even **Static Routing** might suffice as they are easier to configure and maintain.
- **Simple, low-overhead solution**: If you don’t need the scalability or flexibility of OSPF, **RIP** (Routing Information Protocol) might be a simpler, more appropriate option.


## ⚙️🚀 OSPF Features  

| 🏷️ Feature | 📌 Value / Description |  
|------------|---------------------|  
| 🔢 **Protocol Number** | `89` |  
| 📖 **Standard** | Open standard (**OSPFv2** – RFC 2328 for IPv4, **OSPFv3** – RFC 5340 for IPv6) |  
| 🔍 **Type** | Link-State (Uses **bandwidth** for metric calculation) |  
| 🧮 **Algorithm** | Dijkstra’s **Shortest Path First (SPF)** |  
| 🌐 **Protocol Support** | Supports **IPv4** (RFC 2328) and **IPv6** (RFC 5340) |  
| ⚖️ **Metric Calculation** | OSPF uses **cost** as its metric, which is based on **link bandwidth** (lower cost = faster link). |  
| 🎛️ **Administrative Distance (AD)** | `110` |  
| 📡 **Multicast IP (SPF updates)** | `224.0.0.5` |  
| 📡 **Multicast IP (DR/BDR updates)** | `224.0.0.6` |  
| 🔗 **Multicast MAC (SPF updates)** | `01:00:5E:00:00:05` |  
| 🔗 **Multicast MAC (DR/BDR updates)** | `01:00:5E:00:00:06` |  
| 🔄 **Equal Cost Multipath (ECMP)** | **Default:** `4`, **Maximum:** `32` |  

### 📝 Notes  

- **🔍 Link-State & Bandwidth:** OSPF determines the best path based on **link state**. While other protocols may consider multiple factors like **delay, error rate, MTU, load, and reliability**, OSPF uses **only bandwidth** to calculate the cost of a route.  
- **📂 SPF Algorithm & Databases:** OSPF **builds and maintains a database** containing information about **all routers** participating in OSPF within the network. It stores both router details and available routes, using **Dijkstra’s SPF algorithm** to determine the most efficient path based on its database.  



## 🔄🚦 OSPF Operation  

| ⚙️ Operation | 📌 Description |  
|-------------|---------------------------------------------------------------------|  
| 👋 **Hello Protocol** | Uses **HELLO packets** to establish and maintain neighbor relationships. |  
| 🌍 **Uses Areas for Scalability** | Divides large networks into **areas** to reduce routing table size and improve efficiency. |  
| 🔄 **Link-State Advertisements (LSAs)** | Routers exchange **LSAs** to share information about the network topology. |  
| 🏆 **Uses Dijkstra's Algorithm** | Uses the **SPF (Shortest Path First) algorithm** to calculate the best paths. |  
| 🌊 **Flooding of LSAs** | LSAs are **flooded** throughout the network to ensure all routers have updated information. |  
| 📂 **Supports Multiple Routing Tables** | Can support multiple **routing tables** for different address families (e.g., IPv4, IPv6). |  
| 🏛️ **Hierarchical Design with Areas** | The **hierarchical structure** allows networks to be divided into areas, minimizing routing overhead. |  
| ⚖️ **Supports Equal-Cost Multipath (ECMP)** | Supports up to **16 equal-cost paths** for load balancing. |  


⭕ **OSPF Neighbor & Link-State Database (LSDB):**

- 📖 **Neighbor Table:** Stores information about OSPF neighbors, including their state and interface details.
- 📂 **Link-State Database (LSDB):** Maintains all the Link-State Advertisements (LSAs) received from neighbors, representing the entire network topology. This is used to build the OSPF routing table.













## ↔️🔀 OSPF: `Point-to-Point` vs `Multi-Access` Networks 

Here are the key differences between `Point-to-Point` & `Multi-Access` design for an OSPF network:

### **↔️ Point-to-Point (P2P) Networks**

- Direct router-to-router communication.
- OSPF messages use multicast IP address `224.0.0.5` (Layer 3) and multicast MAC address `01:00:5E:00:00:05` (Layer 2).
- Simple 1-to-1 communication. 

<p align="center"> <img src="https://github.com/user-attachments/assets/da47ca2b-b373-4895-a846-788096180701"> 

### **🔀 Multi-Access Networks (eg. Using Eth Switch to connect various routers):**

- OSPF messages use a "one-to-many" model, where multiple routers share the same network.
- Requires a **Designated Router (DR)** to manage communication, reducing overhead. 
- DR centralizes OSPF exchanges to minimize unnecessary message flooding.

<p align="center"> <img src="https://github.com/user-attachments/assets/bc5e304e-8cea-41c5-ab3f-780e23610f83"> 

#### **Router Roles in Multi-Access Networks:**

| **Role**                | **Description**                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------|
| **🏆 Designated Router (DR)** | Manages OSPF exchanges in multi-access networks, acting as the central point for OSPF communication.  |
| **🛡️ Backup DR (BDR)**    | A standby router that takes over if the DR fails, ensuring continuous OSPF operations. |

### 🔊 **Multicast Communication:**

- **📡 P2P Communication:** Multicast to `224.0.0.5` for direct router communication.
  
- **🔄 Multi-Access Communication:** DR sends OSPF updates to other routers using multicast, centralizing control.












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


<p align="center"> <img src="https://github.com/user-attachments/assets/210d952d-603f-4c57-8598-1d09043e7d80"> 











## 🌐 OSPF: `OSPF Areas`

OSPF is a very scalable routing protocol. Its design supports scalability by using a concept called OSPF Area. By dividing a large monolith network into multiple smaller areas, the protocol reduces complexity, limits routing table size, and minimizes the impact of network changes. 

- **OSPF divides networks into areas to optimize routing and reduce overhead.**

It is very important to understand from the beginning that the **OSPF Area is an interface property!!!**. 

- **Every interface that participates in the routing process exists in an Area.** 
- Each router (interface) **must belong to at least one OSPF area**.
- The first and most crucial area tu be used is **Area 0**, known as the **Backbone Area**.  
- A router can belong to multiple areas, but **if only 1 area is used it's must be the Area 0 (Backbone)**.

Areas are identified by 32-bit numbers, expressed either: 

- Simply in decimal
- Often in the same octet-based dot-decimal notation used for IPv4 addresses.

The area identifiers are commonly written in the dot-decimal notation, familiar from IPv4 addressing. However, they are not IP addresses and may duplicate, without conflict, any IPv4 address. By convention, backbone area is always **area 0 (zero), or 0.0.0.0**, it represents the core or backbone area of an OSPF network. While the identifications of other areas may be chosen at will, administrators often select the IP address of a main router in an area as the area identifier. These can be represented in **decimal (1, 2, 3, etc.)** or in **dot-decimal notation (0.0.0.1, 0.0.0.2, etc.)**. The area identifiers for IPv6 implementations (OSPFv3) also use 32-bit identifiers written in the same notation. 

- When dotted formatting is omitted, most implementations expand **area 1** to the area identifier **0.0.0.1**, but some have been known to expand it as **1.0.0.0**.

### 🔹 **Common OSPF Area Numbering Methods**  

| 🔠 **Method** | 📌 **Example** | 📝 **Description** |
|-------------|---------------|-------------------|
| 1️⃣ **Simple Decimal** | `Area 0`, `Area 1`, `Area 2` | The most common method, using sequential numbers. |
| 2️⃣ **Dot-Decimal Notation** | `0.0.0.0`, `0.0.0.1`, `0.0.0.2` | More structured, avoiding IP conflicts. |
| 3️⃣ **Geographic-Based** | `Area 10 (North America)`, `Area 20 (Europe)` | Useful for multinational companies or large networks. |
| 4️⃣ **Router-Based ID** | `Area 192.168.10.1`, `Area 10.1.1.1` | Uses a router’s IP as the area number (less common, can be confusing). |


### 🩻 Area 0 (Backbone)

- When all routers are within a single area (Area 0 or 0.0.0.0), they share a complete **Link-State Database (LSDB)**, they will be **"BD Full" (Full Database)**. 
- This means they will have the complete database of every single router in the area (Area 0)
- ⚠️ **Warning!!!:** In large networks (eg. 100 routers), a single area can lead to a huge LSDB, increasing CPU and memory usage.  
- ✅ **Solution:** Divide the network into multiple areas to reduce overhead.

<p align="center"> <img src="https://github.com/user-attachments/assets/ea339a48-dd40-4da6-b850-d2a024ad3e39"> 

### 🦴 OSPF Multi-Area Design

- All areas must connect to Area 0 (Backbone).  
- **Area Border Routers (ABRs)** connect different areas and maintain separate LSDBs for each area.  
- ABRs must be **powerful routers** because they handle multiple databases and route summarization.

<p align="center"> <img src="https://github.com/user-attachments/assets/366df1d2-83c6-4676-a358-45d2c6b4f9ab"> 



---

### 🧩 OSPF Router Types

![image](https://github.com/user-attachments/assets/da383b56-64d0-4239-83fe-5a67c2d81a0d)

| 🔠 **Acronym** | 🔄 **Router Role** | ❓ **Description** |
|--------------|------------------------------|-------------------------------------------|
| 🟢 **BR** | **Backbone Router** | Router with **at least one interface in backbone area (Area 0)**. |
| 🟡 **IR** | **Internal Router** | All interfaces are in the **same non-backbone area**. |
| ⚫ **ABR** | **Area Border Router** | **Connects two or more OSPF areas**. Maintains multiple LSDBs. |
| ⚪ **ASBR** | **Autonomous System Boundary Router** | Connects **OSPF to other routing protocols** _(eg. EIGRP, BGP, RIP)._ |
| 🏆 **DR** | **Designated Router** | _Only for multi-access networks._ Manages OSPF exchanges, acting as the central point for OSPF communication. |
| 🛡️ **BDR** | **Backup Designated Router** |  _Only for multi-access networks._ A standby router that takes over if the DR fails, ensuring continuous OSPF operations. |

### 🌉 OSPF Area Types

OSPF defines multiple area types to optimize routing and minimize unnecessary routing updates in large networks.

| 🧩 **Area Type** | 📝 **Description** | 📜 **LSA Filtering** |
|--------------------|-----------------------------------------------|--------------------|
| 🩻 **Backbone Area (Area 0 / 0.0.0.0)** | The **core** of the OSPF network. All other areas must connect to it, either **directly or via a virtual link**. | No filtering (All LSAs allowed). |
| 📌 **Regular Area** | A standard **non-backbone area** that allows all **OSPF LSAs (1-5)** and exchanges full routing information. | No filtering (All LSAs allowed). |
| 🚧 **Stub Area** | Blocks **external routes (Type-5 LSAs)**, reducing overhead. Instead, a **default route (0.0.0.0) is injected by the ABR**. | ✅ Blocks **Type-5 LSAs** (External routes) |
| 🚫 **Totally Stubby Area (TSA)** | A **vendor-specific** extension (Cisco, Juniper, etc.) that blocks **both external (Type-5) and inter-area (Type-3, Type-4) routes**, allowing only a **default route** from the ABR. | ✅ Blocks **Type-3, Type-4, Type-5 LSAs** |
| 🔀 **Not-So-Stubby Area (NSSA)** | Like a stub area, but **allows external routes from ASBRs inside the area**. External routes are **converted from Type-7 to Type-5 LSAs by the ABR** and propagated into other areas. **However, NSSA still cannot receive Type-5 LSAs from other areas.** | ✅ Blocks **Type-5 LSAs (from other areas)** 🔄 **Converts Type-7 to Type-5 at ABR** |
| 🚧 **Totally NSSA (Cisco proprietary)** | Like NSSA but **also blocks inter-area routes (Type-3, Type-4 LSAs)**, allowing only **Type-7 from ASBR and a default route from the ABR**. | ✅ Blocks **Type-3, Type-4, Type-5 LSAs** 🔄 **Converts Type-7 to Type-5 at ABR** |
| 🌉 **Transit Area** | Used in **virtual links** when an area **connects two separate OSPF areas** but is **not stubby in any way** (must allow Type-3 LSAs). | No filtering (All LSAs allowed). |

- ✅ **All areas must connect to Area 0**—either directly or via **virtual links**.  
- ✅ **Stub and Totally Stubby Areas** improve performance by filtering unnecessary routes.  
- ✅ **NSSA & Totally NSSA** allow external routes but still limit unnecessary routing updates.  
- ✅ **Transit Areas** allow OSPF traffic to pass through but cannot be stubby.  

📌 **Cisco, Juniper, Huawei, and other vendors support Totally Stubby and NSSA Totally Stubby Areas, even though they are not in the OSPF RFC.** 

---

#### 🟡 IR - Internal Router

An internal router is one whose directly connected interfaces are all assigned to the **same non-backbone area**. 

- eg. Routers `R1` through `R5` are ALL internal routers. 

![image](https://github.com/user-attachments/assets/5906db0e-0673-4ec7-a063-cc9b45f8069e)

- `R1` is internal to **area 34**.
- `R2` and `R3` are internal routers to **area 25**. 
- `R4` and `R5` are internal to **area 5**. 

**Internal routers have a single link-state database (LSDB)** because they belong to only one area. 

- `R1` has **only one LSDB** where it stores **all LSAs flooded in Area 34**.

![image](https://github.com/user-attachments/assets/046733f1-1409-4d68-8054-aedd741a80ed)

Notice that all LSAs (Types 1, 2, and 3) are in **Area 34**. This implies that the device is internal to **Area 34**:

![image](https://github.com/user-attachments/assets/f71ff7e6-ea77-4789-9047-e267d3dc227d)

---

#### ⚫ ABR - Area Border Router

When a router has interfaces in one or more areas and at least one interface connected to the backbone area, it is called an Area Border Router (ABR). 

![image](https://github.com/user-attachments/assets/0fce46be-d67a-4ed7-a4c5-134b4e0b0fa0)

- eg1. `ABR1` is an ABR with interfaces in **Area 0**, **Area 34**, and **Area 25**. 
- eg2. `ABR2` is also an ABR with interfaces in **Area 0** and **Area 5.5.5.5**. 

It is essential to remember that a router must be connected to Area 0 (the backbone) to be considered an ABR. For example, if ABR1 loses connection to the backbone area, it won't provide connectivity between Areas 25 and 34 even though its interfaces are up/up in both areas.

![image](https://github.com/user-attachments/assets/c582c23b-9a91-49d6-a82e-fc17ab86861a)

Notice that ABR1 has LSAs Type 1,2, and 3 for all the areas it connects to - 0, 25, and 34.

This can also be checked using the following command:

![image](https://github.com/user-attachments/assets/761dc3c9-98bb-4d3b-b19d-403c67d0a9d5)

It clearly shows that the `ABR1` has **three LSDB databases** and the number of LSAs inside each database.

---

#### 🟢 BR - Backbone Router

A router that is **internal to Area 0** is considered a **backbone router**. 

- **All interfaces of a backbone device are assigned only to area 0.**

![image](https://github.com/user-attachments/assets/62867517-bf3f-4a10-98cd-a36c7b298865)

For example, devices BB1 and BB2 are considered backbone routers. They have only a single link-state database (LSDB).

---

#### ⚪ ASBR - Autonomous System Border Routers

When a router **redistributes another routing protocol into the OSPF domain**, it is called an **Autonomous System Border Router (ASBR)**. 

An **ASBR connects to multiple Autonomous Systems (AS)** and exchanges routing information between them. Hence, the **ASBR runs OSPF and another routing protocol or routing process** of the same protocol (eg. OSPF & BGP).

![image](https://github.com/user-attachments/assets/1e0b272c-622d-4289-a143-7550923a53e5)

For example, ASBR1 redistributes BGP into OSPF. Every router within the network knows how to reach the ASBR, which runs BGP and knows how to reach external networks. 
















## 🗂️ OSPF: Databases & Tables

OSPF maintains multiple databases to ensure efficient and reliable routing. These databases work together using the **Shortest Path First (SPF) algorithm** to build the best possible routing paths.

## 🗂 OSPF Databases & Corresponding Tables

OSPF utilizes **two primary databases** and their corresponding tables, while the third, **Forwarding Database (Routing Table)**, is ultimately derived from the **Link-State Database (LSDB)**.

| **Database (DB)**        | **Table**         | **Purpose** | **Command to View** |
|--------------------------|------------------|------------|---------------------|
| 1️⃣ **Adjacency DB**         | **Neighbor Table** | Lists all directly connected OSPF neighbors that have established bidirectional communication. | `show ip ospf neighbor` |
| 2️⃣ **Link-State DB (LSDB)** | **Topology Table** | Stores the complete OSPF network topology for the area the router belongs to. Contains all routers and their links. | `show ip ospf database` |
| 3️⃣ **Forwarding DB**        | **Routing Table** | Contains the best paths computed by SPF from the Link-State Database. These are the actual routes used for packet forwarding. | `show ip route ospf` |

### 🔍 Understanding Each Database

1️⃣ **Adjacency Database (Neighbor Table)**

- Tracks all OSPF neighbors that have successfully formed an adjacency.
- Ensures that routers have at least a two-way communication before exchanging LSAs.
- Can be inspected using `show ip ospf neighbor`.

2️⃣ **Link-State Database (LSDB) (Topology Table)**

- Contains detailed information about all OSPF routers, links, and network topology.
- Every router in the same area maintains an identical LSDB.
- Used by the SPF algorithm to compute the best paths.
- Viewable with `show ip ospf database`.

3️⃣ **Forwarding Database (Routing Table)**

- The **Routing Table** lists the best routes selected by SPF from the LSDB.
- This is the final product of OSPF’s path calculations.
- Unlike the LSDB, which stores all possible paths, this table only includes the **best routes**.
- Can be viewed using `show ip route ospf`.

🧐 **Key Observations**

- **Not all paths from the LSDB make it into the Routing Table**—only the best routes computed by SPF.
- The **Topology Table (LSDB) retains information about every router and link**, even those not selected for forwarding.
- If a route is missing from the Routing Table, it may still be seen in the **Topology Table**.

✅ **Pro Tip:** If a route isn't appearing in the Routing Table, check the **Topology Table** (`show ip ospf database`) to see if it's present but not selected as the best path.
























# 📡 OSPF: **Message Types & Functions**  

OSPF relies on five message types for **neighbor discovery, topology exchange, and route calculation**. 

Each message includes a **header** with:  

- **OSPF Version**  
- **Message Type**  
- **Packet Length**  
- **Source OSPF Router ID**  
- **Area ID**  
- **Authentication Data**  

### 📨 **OSPF Message Types & Details**  

| **Message Type**                      | **Type** | **Purpose & Key Information**                                                                                                                                  | **Transmission Method**    | **Reliability**   | **Sequence Number** |
|---------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|-------------------|---------------------|
| **👋 Hello** | `1` | - Sent **every 10 seconds** (default) to establish and maintain neighbor relationships. <br> - Includes the **subnet mask** (not the IP address) of the network where OSPF is enabled. <br> - Contains: **Router Priority, Hello Interval, Designated Router (DR), Backup Designated Router (BDR), and other parameters**. | **Multicast (224.0.0.5)** | **Unreliable** | N/A |
| **🗣️ Database Description (DBD)** | `2` | - Used to summarize the **Link-State Database (LSDB)** when establishing adjacency. <br> - Contains the **known network topology** of the sending router. <br> - Important fields: <br> &nbsp; &nbsp; 🔹 **Init (1)** → First message, may be multiple. <br> &nbsp; &nbsp; 🔹 **More (1)** → More messages will follow. <br> &nbsp; &nbsp; 🔹 **Master (1)** → Indicates the router initiating the exchange. <br> - After the first message, subsequent messages contain actual **Link-State Advertisements (LSAs)**. | **Unicast** | **Reliable** | **Assigned** |
| **📜 Link-State Request (LSR)** | `3` | - Sent to request specific **LSAs** from a neighbor. <br> - Initiates **prefix exchange** between routers. <br> - Effectively asks: **"Send me an update with the prefixes you know"** (e.g., LAN subnets each router has learned). | **Unicast** | **Reliable** | **Assigned** |
| **📑 Link-State Update (LSU)** | `4` | - Sent in response to an LSR, carrying detailed **LSAs**. <br> - Contains the **known prefixes, their links, the router ID announcing them, and any enabled OSPF features**. <br> - Used to propagate topology changes across the OSPF area. | **Unicast & Multicast (224.0.0.5, 224.0.0.6)** | **Reliable** | **Assigned** |
| **✅ Link-State Acknowledgment (LSAck)** | `5` | - Confirms receipt of LSUs. <br> - Prevents unnecessary retransmissions. | **Unicast** | **Unreliable** | N/A |


### 🛠 **OSPF Message Flow Example**  

1️⃣ **Hello** messages are exchanged to discover neighbors.  
2️⃣ Routers send **DBD** messages to compare topology knowledge.  
3️⃣ If a router needs more details, it sends an **LSR**.  
4️⃣ The requested information is provided in an **LSU**.  
5️⃣ The receiving router sends an **LSAck** to confirm.  

This structured process ensures **efficient, loop-free, and scalable** routing across OSPF-enabled networks. 🌎🚀

- `Reliable` means the message is guaranteed to reach its destination, and the sender knows when it has been received.
- `Unreliable` means there's no guarantee the message will reach the destination, and the sender doesn't know if it was received.
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

### OSPF LSA Types








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

























# ⚙️ OSPF Configuration @ `EVE-NG`

This lab focuses on configuring and validating OSPF () in a x ring topology using EVE-NG. 

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










Validacion

`show ip ospf neighbor`
`show ip ospf database`
`show ip route ospf`


````

### 🔎 Validation: `PC Side`

PC-side validation uses basic connectivity tests like ping and tracert to confirm end-to-end reachability.

````sh
ping 8.8.8.8

tracert 8.8.8.8

````

![image](https://github.com/user-attachments/assets/5949e316-3f8c-48d6-b4df-066503cc7b17)


# Cheatsheet

![image](https://github.com/user-attachments/assets/87b03950-89c1-44f2-b5c2-c62359bab36e)

---

![image](https://github.com/user-attachments/assets/fc025835-ee93-4cba-97e5-1b660f02c4fe)







# 📚🗂️🎥 Resources

- [Distance Vector VS Link State](https://www.routexp.com/2020/03/routing-basics-distance-vector-vs-link.html)
- [OSPF a fondo](https://youtu.be/Izm-8BQLRiI?si=TgYYhEefuvJQMJFY)
- [OSPF animado](https://www.youtube.com/watch?v=Zi1imlhYhFI)
- [Multi-Area OSPF - Design Terms](https://www.networkacademy.io/ccna/ospf/multi-area-ospf-design-terms)
- [OSPF (Wikipedia)](https://en.wikipedia.org/wiki/Open_Shortest_Path_First)
  
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





