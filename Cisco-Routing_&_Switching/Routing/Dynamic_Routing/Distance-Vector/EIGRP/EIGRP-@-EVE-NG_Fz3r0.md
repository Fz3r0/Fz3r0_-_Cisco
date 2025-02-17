# 🚀🔄🔧 Cisco: `Dynamic Routing` > `Distance Vector` :: `EIGRP`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Dynamic Routing` `EIGRP` `Distance Vector` `Eve NG` `CCNA` `CCNP` `Cisco`

---

<br>

# 📖 EIGRP :: `EVE NG Config`



This lab focuses on configuring and validating EIGRP (Enhanced Interior Gateway Routing Protocol) in a eight-router ring topology using Eve NG

## Lab Files

- [Fz3r0 :: EVE NG :: **8 Router Ring (BEFORE ANY ROUTING) : `EIGRP`**](https://github.com/user-attachments/files/18816617/Fz3r0-8-Router-Ring-BEFORE-ANY-ROUTING.zip)

## Dynamic Routing Topology: `Fz3r0 Default 5 Ring Router Topology`

This is a network topology consisting of **8 internal routers** (R1 to R8) and one **ISP router** simulating Internet access.  

**Topology Overview:**

- **Two WAN interfaces** using `/30 subnets` (point-to-point links between routers).
- **One LAN interface** using `/24 subnets` (local network per router).
- **Each LAN** have a PC for testing purposes.
- **Router 5 (ISP)** connects the internal network to the **ISP router**, simulating Internet access.  
- Currently, there is **NO dynamic routing**. Only **default routes** are used between ISP & R5.  
- The **ISP router** has **loopback interfaces** representing public DNS servers (Google and Cloudflare).  

![image](https://github.com/user-attachments/assets/45f94b26-d9c6-4929-9f24-ab75c9190e05)

---

### ⚙ **EIGRP Metric Components: `K-values`**  

"K" simply means "coefficient" or "weighting factor" in the EIGRP metric formula. K-values are coefficients assigned to different parameters (such as bandwidth, delay, reliability, etc.) to determine the **weight** of each when calculating the best path.

![image](https://github.com/user-attachments/assets/df818ae5-c321-4b6f-80ee-8d86b0165f24)

| **K-value** | **Parameter**       | **Description**                                                                 | **Default State**         | **Default K-value** | **Actual Value from `show interfaces ethernet 0/0`** |
|-------------|---------------------|---------------------------------------------------------------------------------|---------------------------|---------------------|-----------------------------------------------------|
| **K1** 🔹   | ++ **Bandwidth** (bw) | The minimum bandwidth (in Kbps) along the path. **Higher bandwidth means better routes.** | **Enabled by default** | **1**               | **10,000 Kbps** (10 Mbps)                           |
| **K2** 📉   | **Load**             | Represents how busy the link is (a value between **1-255**). **Higher load means worse performance.** | _Disabled by default_     | **0**               | **1/255** (minimal load, indicates low traffic)      |
| **K3** ⏳   | ++ **Delay** (DLY)   | The cumulative delay (measured in **tens of microseconds**) along the path. **Lower delay is better.** | **Enabled by default** | **1**               | **1000 microseconds** (1 millisecond)               |
| **K4** ✅   | **Reliability**      | A number between **1-255**, where **255 means 100% reliability** (fewer errors). | _Disabled by default_     | **0**               | **255/255** (maximum reliability, perfect link)     |
| **K5** 📦   | **MTU**              | **Maximum Transmission Unit** size. **Not used in EIGRP calculations** (set to 0). | _Disabled by default_     | **0**               | **1500 bytes** (standard Ethernet MTU)              |

## 📋 **IP Addressing Table**  

| 🖥️ **Device** | 🌐 **Interface** | 🏷️ **IP Address** | 🧮 **Subnet Mask** | 🛜 **Network Address (CIDR)** |
|---------------|-----------------|-------------------|-------------------|-----------------------------|
| **R1**       | Eth0/0         | 10.1.0.1         | 255.255.255.252 | 10.1.0.0/30              |
|              | Eth0/1         | 10.8.0.2         | 255.255.255.252 | 10.8.0.0/30              |
|              | Eth0/3         | 192.168.1.1      | 255.255.255.0   | 192.168.1.0/24           |
| **R2**       | Eth0/0         | 10.2.0.1         | 255.255.255.252 | 10.2.0.0/30              |
|              | Eth0/1         | 10.1.0.2         | 255.255.255.252 | 10.1.0.0/30              |
|              | Eth0/3         | 192.168.2.1      | 255.255.255.0   | 192.168.2.0/24           |
| **R3**       | Eth0/0         | 10.3.0.1         | 255.255.255.252 | 10.3.0.0/30              |
|              | Eth0/1         | 10.2.0.2         | 255.255.255.252 | 10.2.0.0/30              |
|              | Eth0/3         | 192.168.3.1      | 255.255.255.0   | 192.168.3.0/24           |
| **R4 (ISP)** | Eth1/0         | 123.123.123.1    | 255.255.255.252 | 123.123.123.0/30         |
|              | Eth0/0         | 10.4.0.1         | 255.255.255.252 | 10.4.0.0/30              |
|              | Eth0/1         | 10.3.0.2         | 255.255.255.252 | 10.3.0.0/30              |
|              | Eth0/3         | 192.168.4.1      | 255.255.255.0   | 192.168.4.0/24           |
| **R5**       | Eth0/0         | 10.5.0.1         | 255.255.255.252 | 10.5.0.0/30              |
|              | Eth0/1         | 10.4.0.2         | 255.255.255.252 | 10.4.0.0/30              |
|              | Eth0/3         | 192.168.5.1      | 255.255.255.0   | 192.168.5.0/24           |
| **R6**       | Eth0/0         | 10.6.0.1         | 255.255.255.252 | 10.6.0.0/30              |
|              | Eth0/1         | 10.5.0.2         | 255.255.255.252 | 10.5.0.0/30              |
|              | Eth0/3         | 192.168.6.1      | 255.255.255.0   | 192.168.6.0/24           |
| **R7**       | Eth0/0         | 10.7.0.1         | 255.255.255.252 | 10.7.0.0/30              |
|              | Eth0/1         | 10.6.0.2         | 255.255.255.252 | 10.6.0.0/30              |
|              | Eth0/3         | 192.168.7.1      | 255.255.255.0   | 192.168.7.0/24           |
| **R8**       | Eth0/0         | 10.8.0.1         | 255.255.255.252 | 10.8.0.0/30              |
|              | Eth0/1         | 10.7.0.2         | 255.255.255.252 | 10.7.0.0/30              |
|              | Eth0/3         | 192.168.8.1      | 255.255.255.0   | 192.168.8.0/24           |
| **R-ISP**    | Eth0/0         | 123.123.123.2    | 255.255.255.252 | 123.123.123.0/30         |
|              | Loopback0      | 8.8.8.8          | 255.255.255.255 | 8.8.8.8/32 (Google DNS)  |
|              | Loopback1      | 1.1.1.1          | 255.255.255.255 | 1.1.1.1/32 (Cloudflare)  |

### 💻 **VPC Addressing Table**  

| 🖥️ **VPC** | 🏷️ **IP Address**     | 🧮 **Subnet Mask**   | 🚀 **Default Gateway**  | 🛜 **Network (CIDR)**   |
|------------|---------------------|---------------------|-----------------------|-----------------------|
| **VPC1**   | 192.168.1.100       | 255.255.255.0     | 192.168.1.1         | 192.168.1.0/24       |
| **VPC2**   | 192.168.2.100       | 255.255.255.0     | 192.168.2.1         | 192.168.2.0/24       |
| **VPC3**   | 192.168.3.100       | 255.255.255.0     | 192.168.3.1         | 192.168.3.0/24       |
| **VPC4**   | 192.168.4.100       | 255.255.255.0     | 192.168.4.1         | 192.168.4.0/24       |
| **VPC5**   | 192.168.5.100       | 255.255.255.0     | 192.168.5.1         | 192.168.5.0/24       |
| **VPC6**   | 192.168.6.100       | 255.255.255.0     | 192.168.6.1         | 192.168.6.0/24       |
| **VPC7**   | 192.168.7.100       | 255.255.255.0     | 192.168.7.1         | 192.168.7.0/24       |
| **VPC8**   | 192.168.8.100       | 255.255.255.0     | 192.168.8.1         | 192.168.8.0/24       |

## 🏁🔄⚙️ Routing Topology: `Init Configuration`

This configuration focuses exclusively on **IP addressing** for each router’s interfaces and their respective subnets.  

- 🚫 **No dynamic or static routing protocols are configured at this stage!** 🚫 _(The only exception is a **default route** on the **ISP router (R-ISP)** to simulate Internet connectivity.)_

Each router is assigned the appropriate **WAN and LAN IP addresses**, but **no routing protocols** (such as OSPF, EIGRP, or BGP) are active yet.  

💡 **Purpose of this Initial Setup:**  

- ✅ Ensure all router interfaces are properly **configured and operational**.  
- ✅ Provide a solid foundation for **future routing protocol deployments**.  
- ✅ Allow **end-to-end connectivity testing** (e.g., using `ping` between routers or VPCs in the same subnet).  

In subsequent configurations, **dynamic routing protocols** (e.g., **EIGRP**) can be implemented to establish full network connectivity and routing between all subnets. 

### ⚙️ Init Setup: `Router 1`

````py
! ################
! ##  ROUTER 1  ##
! ################
!
enable
configure terminal
!
hostname R1
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.1.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.8.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.1.1 255.255.255.0
no shutdown
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!


````

### ⚙️ Init Setup: `Router 2`

````py
! ################
! ##  ROUTER 2  ##
! ################
!
enable
configure terminal
!
hostname R2
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.2.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.1.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.2.1 255.255.255.0
no shutdown
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!


````

### ⚙️ Init Setup: `Router 3`

````py
! ################
! ##  ROUTER 3  ##
! ################
!
enable
configure terminal
!
hostname R3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.3.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.2.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.3.1 255.255.255.0
no shutdown
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!


````

### ⚙️ Init Setup: `Router 4`

````py
! ################
! ##  ROUTER 4  ##
! ################
!
enable
configure terminal
!
hostname R4
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## INTERNET / ISP
!
interface ethernet 1/0
ip address 123.123.123.1 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.4.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.3.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.4.1 255.255.255.0
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## INTERNET @ ISP :: IPROUTE (DEFAULT ROUTE)
!
! # Default Route @ ISP (next hop interface)
ip route 0.0.0.0 0.0.0.0 123.123.123.2
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!


````

### ⚙️ Init Setup: `Router 5`

````py
! ################
! ##  ROUTER 5  ##
! ################
!
enable
configure terminal
!
hostname R5
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.5.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.4.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.5.1 255.255.255.0
no shutdown
end
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
! 
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
show ip route
!


````

### ⚙️ Init Setup: `Router 6`

````py
! ################
! ##  ROUTER 6  ##
! ################
!
enable
configure terminal
!
hostname R6
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.6.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.5.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.6.1 255.255.255.0
no shutdown
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!


````

### ⚙️ Init Setup: `Router 7`

````py
! ################
! ##  ROUTER 7  ##
! ################
!
enable
configure terminal
!
hostname R7
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.7.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.6.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.7.1 255.255.255.0
no shutdown
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!


````

### ⚙️ Init Setup: `Router 8`

````py
! ################
! ##  ROUTER 8  ##
! ################
!
enable
configure terminal
!
hostname R8
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## WAN SIDE
!
interface ethernet 0/0
ip address 10.8.0.1 255.255.255.252
no shutdown
exit
!
interface ethernet 0/1
ip address 10.7.0.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LAN SIDE
!
interface ethernet 0/3
ip address 192.168.8.1 255.255.255.0
no shutdown
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!


````

### ⚙️ Init Setup: `Router ISP (Internet)`

````py
! ##################
! ##  ROUTER ISP  ##
! ##################
!
enable
configure terminal
!
hostname R-ISP
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## INTERNET @ R4 :: IPROUTE (DEFAULT ROUTE)
!
interface ethernet 0/0
ip address 123.123.123.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LOOPBACK INTERFACES
!
! # Google  DNS
interface Loopback0
ip address 8.8.8.8 255.255.255.255
exit
!
! # Cloudflare DNS
interface Loopback1
ip address 1.1.1.1 255.255.255.255
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## INTERNET @ R4 :: IPROUTE (DEFAULT ROUTE)
!
! # Default Route @ R4 (next hop interface)
ip route 0.0.0.0 0.0.0.0 123.123.123.1
end
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
show ip route
!


````

### ⚙️ Init Setup: `VPC (Virtual PC's)`

````py
## VPC CONFIGURATION:

### VPC1
set pcname VPC1
ip 192.168.1.100 255.255.255.0 192.168.1.1
save

### VPC2
set pcname VPC2
ip 192.168.2.100 255.255.255.0 192.168.2.1
save

### VPC3
set pcname VPC3
ip 192.168.3.100 255.255.255.0 192.168.3.1
save

### VPC4
set pcname VPC4
ip 192.168.4.100 255.255.255.0 192.168.4.1
save

### VPC5
set pcname VPC5
ip 192.168.5.100 255.255.255.0 192.168.5.1
save

### VPC6
set pcname VPC6
ip 192.168.6.100 255.255.255.0 192.168.6.1
save

### VPC7
set pcname VPC7
ip 192.168.7.100 255.255.255.0 192.168.7.1
save

### VPC8
set pcname VPC8
ip 192.168.8.100 255.255.255.0 192.168.8.1
save

````



## 🚀🔄⚙️ **EIGRP Configuration**

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
5. Advertise a default route if required.
6. Implement manual route summarization when needed.

In this example, we will configure EIGRP between five routers (R1 to R5) in the ring topology, as defined in the setup:

### ⚙️ **R1 Configuration:**

```py
! #######################################
! ##  ROUTER 1 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R1
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.8.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.1.0.0 0.0.0.3
network 10.8.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.1.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.1.0 to 192.168.1.255"
network 192.168.1.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```

### ⚙️ **R2 Configuration:**

```py
! #######################################
! ##  ROUTER 2 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R2
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.2.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.1.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.2.0.0 0.0.0.3
network 10.1.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.2.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.2.0 to 192.168.2.255"
network 192.168.2.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```

### ⚙️ **R3 Configuration:**

```py
! #######################################
! ##  ROUTER 3 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.3.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.2.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.3.0.0 0.0.0.3
network 10.2.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.3.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.3.0 to 192.168.3.255"
network 192.168.3.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```

### ⚙️ **R4 Configuration:**

```py
! #######################################
! ##  ROUTER 4 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R4
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.4.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.3.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.4.0.0 0.0.0.3
network 10.3.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.4.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.4.0 to 192.168.4.255"
network 192.168.4.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
ip route 0.0.0.0 0.0.0.0 123.123.123.2
router eigrp 666
redistribute static
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```

### ⚙️ **R5 Configuration:**

```py
! #######################################
! ##  ROUTER 5 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R5
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.4.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.5.0.0 0.0.0.3
network 10.4.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.5.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.5.0 to 192.168.5.255"
network 192.168.5.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```

### ⚙️ **R6 Configuration:**

```py
! #######################################
! ##  ROUTER 6 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R6
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.6.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.5.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.6.0.0 0.0.0.3
network 10.5.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.6.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.6.0 to 192.168.6.255"
network 192.168.6.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```

### ⚙️ **R7 Configuration:**

```py
! #######################################
! ##  ROUTER 7 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R7
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.7.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.6.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.7.0.0 0.0.0.3
network 10.6.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.7.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.7.0 to 192.168.7.255"
network 192.168.7.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```

### ⚙️ **R8 Configuration:**

```py
! #######################################
! ##  ROUTER 8 :: EIGRP CONFIGURATION  ##
! #######################################
!
enable
configure terminal
!
hostname R8
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 1: Set Autonomous System (AS)
router eigrp 666
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 2: Disable automatic summarization.
no auto-summary
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 3: Enable EIGRP & Messages on each Interface (except WAN/Internet if available)
!
!     ## WAN:
!     # enable EIGRP on any interface with an IP address that falls within the 10.8.0.0/30 subnet.
!     # enable EIGRP on any interface with an IP address that falls within the 10.7.0.0/30 subnet.
!         # IMPORTANT: EIGRP can be used with wildcard or subnet mask, it will recognize both. 
!         # 0.0.0.3 → This is a wildcard mask, which works as the inverse of a subnet mask.
!         # 0.0.0.3 means: "Only match the first 30 bits of the IP address."
!           #   - This corresponds to a /30 subnet (255.255.255.252).
!           #   - A /30 subnet supports two usable hosts (typically used for point-to-point links).
network 10.8.0.0 0.0.0.3
network 10.7.0.0 0.0.0.3
!
!     ## LAN:
!     # Enables EIGRP on interfaces in the 192.168.8.0/24 subnet.
!         # 0.0.0.255 → Wildcard mask meaning "Match any IP in the range 192.168.8.0 to 192.168.8.255"
network 192.168.8.0 0.0.0.255
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 4: Disable EIGRP messaging on LAN access interfaces (Switches/PCs).
passive-interface ethernet 0/3
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Step 5: Inject a default route in the border router (if there is a WAN/Internet connection)
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! 6. # Step 6: Manually summarize routes when needed.
! NOT NEEDED!!!
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! # Exit & Save Configurations
end
write memory
!
!

```


## EIGRP Load Balancing (Equal Metrics)

- **Equal Metrics**: EIGRP performs load balancing when multiple routes have **identical metrics** and **administrative distance**.

![image](https://github.com/user-attachments/assets/cc6e9b28-08ba-42e6-a403-b6f3b20838b7)
  
### Example from Router 8:

- **Routes for 0.0.0.0/0**:

  - `10.7.0.1` (via Ethernet0/1)
  - `10.8.0.2` (via Ethernet0/0)
  
- **Metric**: Both routes have the same metric (`384000`).
- **Administrative Distance**: Both routes have the same EIGRP distance (`170`).

![image](https://github.com/user-attachments/assets/ee737d36-fc52-40f0-a8b4-34f550ceb126)


### EIGRP Behavior:

- **Load Balancing**: Since both routes have the same metric and distance, EIGRP distributes the outbound traffic equally between them.
- **Ping Test**: `ping` requests and responses alternate between `10.7.0.1` and `10.8.0.2`, indicating successful load balancing.
- **By Default**: EIGRP allows up to 4 equal-cost routes to be used in load balancing.

If ping is sent from R8 (same distance, metric, hops) it will balance each packet:

![image](https://github.com/user-attachments/assets/d584810c-847a-4f5b-90de-0f0ab569f389)

If ping is sent from PC8 (same distance, metric, hops) it will balance IP session, all pings will be sent and receive using same interface:

![image](https://github.com/user-attachments/assets/fa0d46c4-d5e1-4570-8077-7eb734ef763d)












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
!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
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

````









# 📚🗂️🎥 Resources

- [Distance Vector VS Link State](https://www.routexp.com/2020/03/routing-basics-distance-vector-vs-link.html)
  
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








