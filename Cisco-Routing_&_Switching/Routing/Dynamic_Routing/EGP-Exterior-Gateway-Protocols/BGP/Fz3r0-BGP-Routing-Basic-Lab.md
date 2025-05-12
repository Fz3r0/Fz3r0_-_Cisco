# 🚀🔄🔧 Cisco: `Dynamic Routing` > `EGP` :: `BGP`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### **Keywords**: `BGP` `eBGP` `Routing` `Dynamic Routing` `BGP Peering` `Route Advertisement` `Subnetting` `Traceroute` `Packet Capture` `Wireshark` `EVE-NG` `dot1Q` `Subinterfaces` `LAN-to-LAN Routing`

---


</div>

# 🌐🔄🖧 BGP Lab

This lab simulates a basic BGP (Border Gateway Protocol) routing scenario between two remote sites (Site A and Site B) using four routers interconnected over point-to-point links. Each edge router connects to a local Layer 2 switch, which in turn connects to multiple end devices (PCs) grouped by VLANs (10, 20, and 30). The core objective is to establish eBGP sessions across routers and enable end-to-end IP connectivity **within the same VLANs across sites**.

The simplicity of this setup allows for controlled experiments focused on:

- 🔍 Observing BGP peering and route advertisement behavior
- 🧭 Performing `traceroute` between edge hosts
- 📦 Capturing traffic at key points using Wireshark or built-in tools in **EVE-NG**
- 🧱 Practicing subinterface configuration with `dot1Q` tagging
- 🚫 Verifying VLAN isolation (no inter-VLAN routing)

Although VLANs are present locally at each site, the primary goal is **not** to implement routing between them. Instead, the emphasis is on simulating routed traffic **across** the BGP domain between matching VLANs at each site (e.g., VLAN 10 to VLAN 10).


## Topology

![image](https://github.com/user-attachments/assets/7cbc2d83-63b8-4c5d-991f-eb8c05f9e894)



## Address Table

| **Device**   | **BGP AS** | **Interface** | **IP Address**   | **CIDR** | **Subnet Mask**   | **Description**               |
| ------------ | ---------- | ------------- | ---------------- | -------- | ----------------- | ----------------------------- |
| 🌐 **R1**    | `65001`    | Eth0/0        | `1.0.0.1`        | `/30`    | `255.255.255.252` | WAN link to R2                |
|              |            | Eth0/1.10     | `192.168.10.254` | `/24`    | `255.255.255.0`   | Gateway for VLAN 10 in Site A |
|              |            | Eth0/1.20     | `192.168.20.254` | `/24`    | `255.255.255.0`   | Gateway for VLAN 20 in Site A |
|              |            | Eth0/1.30     | `192.168.30.254` | `/24`    | `255.255.255.0`   | Gateway for VLAN 30 in Site A |
| 📡 **R2**    | `65002`    | Eth0/0        | `1.0.0.2`        | `/30`    | `255.255.255.252` | Link to R1                    |
|              |            | Eth0/1        | `2.0.0.1`        | `/30`    | `255.255.255.252` | Link to R3                    |
| 📡 **R3**    | `65003`    | Eth0/1        | `2.0.0.2`        | `/30`    | `255.255.255.252` | Link to R2                    |
|              |            | Eth0/0        | `3.0.0.1`        | `/30`    | `255.255.255.252` | Link to R4                    |
| 🌐 **R4**    | `65004`    | Eth0/0        | `3.0.0.2`        | `/30`    | `255.255.255.252` | WAN link to R3                |
|              |            | Eth0/1.40     | `192.168.40.254` | `/24`    | `255.255.255.0`   | Gateway for VLAN 40 in Site B |
|              |            | Eth0/1.50     | `192.168.50.254` | `/24`    | `255.255.255.0`   | Gateway for VLAN 50 in Site B |
|              |            | Eth0/1.60     | `192.168.60.254` | `/24`    | `255.255.255.0`   | Gateway for VLAN 60 in Site B |
| 💻 **PC-A1** |            | eth0          | `192.168.10.100` | `/24`    | `255.255.255.0`   | Site A - VLAN 10 PC #1        |
| 💻 **PC-A2** |            | eth0          | `192.168.10.101` | `/24`    | `255.255.255.0`   | Site A - VLAN 10 PC #2        |
| 💻 **PC-A3** |            | eth0          | `192.168.20.100` | `/24`    | `255.255.255.0`   | Site A - VLAN 20 PC #1        |
| 💻 **PC-A4** |            | eth0          | `192.168.20.101` | `/24`    | `255.255.255.0`   | Site A - VLAN 20 PC #2        |
| 💻 **PC-A5** |            | eth0          | `192.168.30.100` | `/24`    | `255.255.255.0`   | Site A - VLAN 30 PC #1        |
| 💻 **PC-A6** |            | eth0          | `192.168.30.101` | `/24`    | `255.255.255.0`   | Site A - VLAN 30 PC #2        |
| 💻 **PC-B1** |            | eth0          | `192.168.40.100` | `/24`    | `255.255.255.0`   | Site B - VLAN 40 PC #1        |
| 💻 **PC-B2** |            | eth0          | `192.168.40.101` | `/24`    | `255.255.255.0`   | Site B - VLAN 40 PC #2        |
| 💻 **PC-B3** |            | eth0          | `192.168.50.100` | `/24`    | `255.255.255.0`   | Site B - VLAN 50 PC #1        |
| 💻 **PC-B4** |            | eth0          | `192.168.50.101` | `/24`    | `255.255.255.0`   | Site B - VLAN 50 PC #2        |
| 💻 **PC-B5** |            | eth0          | `192.168.60.100` | `/24`    | `255.255.255.0`   | Site B - VLAN 60 PC #1        |
| 💻 **PC-B6** |            | eth0          | `192.168.60.101` | `/24`    | `255.255.255.0`   | Site B - VLAN 60 PC #2        |




# Configuration

## Routers

### 🌐 R1 – Site A Edge Router - AS 65001

````py
enable
configure terminal
hostname R1-65001

interface Eth0/0
   description ** LINK TO R2 **
   ip address 1.0.0.1 255.255.255.252
   speed 1000
   duplex full
   no shutdown

interface Eth0/1
   description ** TRUNK TO SWITCH SW1 **
   speed 1000
   duplex full
   no shutdown

interface Eth0/1.10
   encapsulation dot1Q 10
   ip address 192.168.10.254 255.255.255.0
   description ** VLAN 10 GATEWAY - SITE A **

interface Eth0/1.20
   encapsulation dot1Q 20
   ip address 192.168.20.254 255.255.255.0
   description ** VLAN 20 GATEWAY - SITE A **

interface Eth0/1.30
   encapsulation dot1Q 30
   ip address 192.168.30.254 255.255.255.0
   description ** VLAN 30 GATEWAY - SITE A **

router bgp 65001
   bgp log-neighbor-changes
   no auto-summary
   neighbor 1.0.0.2 remote-as 65002
   network 192.168.10.0 mask 255.255.255.0
   network 192.168.20.0 mask 255.255.255.0
   network 192.168.30.0 mask 255.255.255.0

end
wr

!
!

````

### 📡 R2 – Intermediate Router - AS 65002


````py
enable
configure terminal
hostname R2-65002

interface Eth0/0
   description ** LINK TO R1 **
   ip address 1.0.0.2 255.255.255.252
   speed 1000
   duplex full
   no shutdown

interface Eth0/1
   description ** LINK TO R3 **
   ip address 2.0.0.1 255.255.255.252
   speed 1000
   duplex full
   no shutdown

router bgp 65002
   bgp log-neighbor-changes
   no auto-summary
   neighbor 1.0.0.1 remote-as 65001
   neighbor 2.0.0.2 remote-as 65003
   neighbor 2.0.0.2 next-hop-self
   network 1.0.0.0 mask 255.255.255.252
   network 2.0.0.0 mask 255.255.255.252

end
wr

!
!


````


### 📡 R3 – Intermediate Router - AS 65003


````py
enable
configure terminal
hostname R3-65003

interface Eth0/1
   description ** LINK TO R2 **
   ip address 2.0.0.2 255.255.255.252
   speed 1000
   duplex full
   no shutdown

interface Eth0/0
   description ** LINK TO R4 **
   ip address 3.0.0.1 255.255.255.252
   speed 1000
   duplex full
   no shutdown

router bgp 65003
   bgp log-neighbor-changes
   no auto-summary
   neighbor 2.0.0.1 remote-as 65002
   neighbor 3.0.0.2 remote-as 65004
   neighbor 3.0.0.2 next-hop-self
   network 2.0.0.0 mask 255.255.255.252
   network 3.0.0.0 mask 255.255.255.252

end
wr
!
!


````

### 🌐 R4 – Site A Edge Router - AS 65004

````py
enable
configure terminal
hostname R4-65004

interface Eth0/0
   description ** LINK TO R3 **
   ip address 3.0.0.2 255.255.255.252
   speed 1000
   duplex full
   no shutdown

interface Eth0/1
   description ** TRUNK TO SWITCH SW2 **
   speed 1000
   duplex full
   no shutdown

interface Eth0/1.40
   encapsulation dot1Q 40
   ip address 192.168.40.254 255.255.255.0
   description ** VLAN 10 GATEWAY - SITE B **

interface Eth0/1.50
   encapsulation dot1Q 50
   ip address 192.168.50.254 255.255.255.0
   description ** VLAN 20 GATEWAY - SITE B **

interface Eth0/1.60
   encapsulation dot1Q 60
   ip address 192.168.60.254 255.255.255.0
   description ** VLAN 30 GATEWAY - SITE B **

router bgp 65004
   bgp log-neighbor-changes
   no auto-summary
   neighbor 3.0.0.1 remote-as 65003
   network 192.168.40.0 mask 255.255.255.0
   network 192.168.50.0 mask 255.255.255.0
   network 192.168.60.0 mask 255.255.255.0

end
wr

!
!


````

## Switches

### 🧠 SW1 – Site A Layer 2 Switch


````py
!# Enable, Config & Naming
enable
configure terminal
!
hostname SW1-Site-A

!# VLAN definitions for Site A
vlan 10
   name VLAN10-SITEA
vlan 20
   name VLAN20-SITEA
vlan 30
   name VLAN30-SITEA

!# Access ports for VLAN 10 : PC-A1, PC-A2
interface range Eth0/0 - 1
   description ** VLAN 10 ACCESS - PC-A1 / PC-A2 **
   switchport mode access
   switchport access vlan 10
   duplex full
   no shutdown

!# Access ports for VLAN 20 : PC-A3, PC-A4
interface range Eth0/2 - 3
   description ** VLAN 20 ACCESS - PC-A3 / PC-A4 **
   switchport mode access
   switchport access vlan 20
   duplex full
   no shutdown

!# Access ports for VLAN 30 : PC-A5, PC-A6
interface range Eth1/0 - 1
   description ** VLAN 30 ACCESS - PC-A5 / PC-A6 **
   switchport mode access
   switchport access vlan 30
   duplex full
   no shutdown

!# Trunk port to R1 (Router)
interface Eth2/3
   description ** TRUNK TO R1 ROUTER **
   switchport mode trunk
   switchport trunk encapsulation dot1q
   duplex full
   no shutdown


end
wr

!
!


````



### 🧠 SW2 – Site B Layer 2 Switch


````py
!# Enable, Config & Naming
enable
configure terminal
!
hostname SW1-Site-B

!# VLAN definitions for Site B
vlan 40
   name VLAN40-SITEB
vlan 50
   name VLAN50-SITEB
vlan 60
   name VLAN60-SITEB

!# Access ports for VLAN 40 : PC-B1, PC-B2
interface range Eth0/0 - 1
   description ** VLAN 40 ACCESS - PC-B1 / PC-B2 **
   switchport mode access
   switchport access vlan 40
   duplex full
   no shutdown

!# Access ports for VLAN 50 : PC-B3, PC-B4
interface range Eth0/2 - 3
   description ** VLAN 50 ACCESS - PC-B3 / PC-B4 **
   switchport mode access
   switchport access vlan 50
   duplex full
   no shutdown

!# Access ports for VLAN 60 : PC-B5, PC-B6
interface range Eth1/0 - 1
   description ** VLAN 60 ACCESS - PC-B5 / PC-B6 **
   switchport mode access
   switchport access vlan 60
   duplex full
   no shutdown

!# Trunk port to R4 (Router)
interface Eth2/3
   description ** TRUNK TO R4 ROUTER **
   switchport mode trunk
   switchport trunk encapsulation dot1q
   duplex full
   no shutdown


end
wr

!
!


````


## Hosts

### PC-A1

````py
!# PC Name & Static IP
set pcname PC-A1
ip 192.168.10.100/24 192.168.10.254
save

!


````

### PC-A2

````py
!# PC Name & Static IP
set pcname PC-A2
ip 192.168.10.101/24 192.168.10.254
save

!


````

### PC-A3

````py
!# PC Name & Static IP
set pcname PC-A3
ip 192.168.20.100/24 192.168.20.254
save

!


````

### PC-A4

````py
!# PC Name & Static IP
set pcname PC-A4
ip 192.168.20.101/24 192.168.20.254
save

!


````

### PC-A5

````py
!# PC Name & Static IP
set pcname PC-A5
ip 192.168.30.100/24 192.168.30.254
save

!


````

### PC-A6

````py
!# PC Name & Static IP
set pcname PC-A6
ip 192.168.30.101/24 192.168.30.254
save

!


````

---

### PC-B1

````py
!# PC Name & Static IP
set pcname PC-B1
ip 192.168.40.100/24 192.168.40.254
save

!


````

### PC-B2

````py
!# PC Name & Static IP
set pcname PC-B2
ip 192.168.40.101/24 192.168.40.254
save

!


````

### PC-B3

````py
!# PC Name & Static IP
set pcname PC-B3
ip 192.168.50.100/24 192.168.50.254
save

!


````

### PC-B4

````py
!# PC Name & Static IP
set pcname PC-B4
ip 192.168.50.101/24 192.168.50.254
save

!


````

### PC-B5

````py
!# PC Name & Static IP
set pcname PC-B5
ip 192.168.60.100/24 192.168.60.254
save

!


````

### PC-B6

````py
!# PC Name & Static IP
set pcname PC-B6
ip 192.168.60.101/24 192.168.60.254
save

!


````





# Traceroute Lab

















# 📚🗂️🎥 Resources

- 
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





