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

- [Download Cisco Packet Tracer Fz3r0 Labs :: **Dynamic Route x5 Routers : `EIGRP`**]()

## Dynamic Routing Topology: `Fz3r0 Default 5 Ring Router Topology`

This setup consists of **eight routers** (R1 to R8) connected in a **ring topology**, each with:

- **Two WAN interfaces** using `/30 subnets` (point-to-point links between routers).
- **One LAN interface** using `/24 subnets` (local network per router).
- **Each LAN** have a PC for testing purposes.
- **There's an additional Router-WAN to simulate Internet/Google connection to 8.8.8.8**

![image](https://github.com/user-attachments/assets/926a7a83-79c5-4f01-8948-896b96947828)

## 📋 **IP Addressing Table**

| Device    | Interface | IP Address   | Subnet Mask      | Network Address with CIDR |
|-----------|-----------|--------------|------------------|---------------------------|
| **R1**    | Eth0/0     | 10.1.0.1      | 255.255.255.252 | 10.1.0.0/30               |
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
| **R6**    | Fa0/0     | 10.5.0.1      | 255.255.255.252 | 10.5.0.0/30               |
|           | Fa0/1     | 10.4.0.2      | 255.255.255.252 | 10.4.0.0/30               |
|           | Fa1/1     | 192.168.5.1   | 255.255.255.0   | 192.168.5.0/24            |
| **R7**    | Fa0/0     | 10.5.0.1      | 255.255.255.252 | 10.5.0.0/30               |
|           | Fa0/1     | 10.4.0.2      | 255.255.255.252 | 10.4.0.0/30               |
|           | Fa1/1     | 192.168.5.1   | 255.255.255.0   | 192.168.5.0/24            |
| **R8**    | Fa0/0     | 10.5.0.1      | 255.255.255.252 | 10.5.0.0/30               |
|           | Fa0/1     | 10.4.0.2      | 255.255.255.252 | 10.4.0.0/30               |
|           | Fa1/1     | 192.168.5.1   | 255.255.255.0   | 192.168.5.0/24            |
| **ISP**   | Fa0/0     | 200.1.1.2     | 255.255.255.252 | 200.1.1.1/30              |
|           | Lo0       | 8.8.8.8       | 255.255.255.255 | 8.8.8.8/32                |

- `PC-1 (Site-A)` :: 192.168.1.100/24
- `PC-2 (Site-B)` :: 192.168.2.100/24
- `PC-3 (Site-C)` :: 192.168.3.100/24
- `PC-4 (Site-D)` :: 192.168.4.100/24
- `PC-5 (Site-E)` :: 192.168.5.100/24
- `PC-6 (Site-E)` :: 192.168.6.100/24
- `PC-7 (Site-E)` :: 192.168.7.100/24
- `PC-8 (Site-E)` :: 192.168.8.100/24






  

## 🏁🔄⚙️ Routing Topology: `Init Configuration`

This configuration solely establishes IP addressing for each router’s interfaces and their respective subnets. 

- **NO dynamic or static routing configurations are applied at this stage!!!** _The only exception is a default route on the Internet router (R-WAN), ensuring it is ready when EIGRP routing is later implemented across the network. Each router is assigned appropriate WAN and LAN IP addresses, but no routing protocols are active yet._

The following setup ensures all routers have their interfaces configured and operational, providing a foundation for future any routing protocol deployment:

### ⚙️ Init Setup: `Router 4`

### ⚙️ Init Setup: `Router 5`

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
! ## INTERNET @ ISP :: IPROUTE (DEFAULT ROUTE)
!
interface ethernet 1/0
! # Interface IP
ip address 123.123.123.1 255.255.255.252
! # Default Route @ ISP (next hop interface)
ip route 0.0.0.0 0.0.0.0 123.123.123.2
no shutdown
exit
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
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
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













### ⚙️ Init Setup: `Router WAN (Internet)`

````py
! ##################
! ##  ROUTER WAN  ##
! ##################
!
enable
configure terminal
!
hostname R-WAN
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## INTERNET @ R4 :: IPROUTE (DEFAULT ROUTE)
!
interface ethernet 0/0
! # Interface IP
ip address 123.123.123.2 255.255.255.252
! # Default Route @ R4 (next hop interface)
ip route 0.0.0.0 0.0.0.0 123.123.123.1
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LOOPBACK INTERFACES
!
! # Google  DNS
interface Loopback0
ip address 8.8.8.8. 255.255.255.255
no shutdown
exit
!
! # Cloudflare DNS
interface Loopback1
ip address 1.1.1.1. 255.255.255.255
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## SAVE & CHECK CONFIGS
!
write memory
!
show ip interface brief
!






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

### ⚙️ Init Setup: `VPC (Virtual PC's)`

````py
## VPC CONFIGURATION:

### VPC1
ip 192.168.1.100 255.255.255.0 192.168.1.1
save

### VPC2
ip 192.168.2.100 255.255.255.0 192.168.2.1
save

### VPC3
ip 192.168.3.100 255.255.255.0 192.168.3.1
save

### VPC4
ip 192.168.4.100 255.255.255.0 192.168.4.1
save

### VPC5
ip 192.168.5.100 255.255.255.0 192.168.5.1
save

### VPC6
ip 192.168.6.100 255.255.255.0 192.168.6.1
save

### VPC7
ip 192.168.7.100 255.255.255.0 192.168.7.1
save

### VPC8
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








