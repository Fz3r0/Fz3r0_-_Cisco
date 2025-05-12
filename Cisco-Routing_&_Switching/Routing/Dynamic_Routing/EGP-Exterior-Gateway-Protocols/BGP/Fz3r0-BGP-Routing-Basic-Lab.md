# 🚀🔄🔧 Cisco: `Dynamic Routing` > `Link State` :: `OSPF`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### **Keywords**: `Routing` `Dynamic Routing` `BGP`

---




</div>




# 🌐🔄🖧 BGP Lab

## Topology


## Address Table

| **Device**       | **Interface** | **IP Address**       | **CIDR**  | **Subnet Mask**       | **Description**  | **BGP AS**  |
| ------------ | --------- | ---------------- | ----- | ----------------- | ----------------------------- | ------------------ |
| 🌐 **R1**    | Eth0      | `1.0.0.1`        | `/30` | `255.255.255.252` | WAN link to R2                |    `65001`       |
|              | Eth1.10   | `192.168.10.254`   | `/24` | `255.255.255.0`   | Gateway for VLAN 10 in Site A |               |
|              | Eth1.20   | `192.168.20.254`   | `/24` | `255.255.255.0`   | Gateway for VLAN 20 in Site A |               |
|              | Eth1.30   | `192.168.30.254`   | `/24` | `255.255.255.0`   | Gateway for VLAN 30 in Site A |               |
| 📡 **R2**    | Eth0      | `1.0.0.2`        | `/30` | `255.255.255.252` | Link to R1                    |  `65002`       |
|              | Eth1      | `2.0.0.1`        | `/30` | `255.255.255.252` | Link to R3                    |               |
| 📡 **R3**    | Eth1      | `2.0.0.2`        | `/30` | `255.255.255.252` | Link to R2                    |   `65003`     |
|              | Eth0      | `3.0.0.1`        | `/30` | `255.255.255.252` | Link to R4                    |               |
| 🌐 **R4**    | Eth0      | `3.0.0.2`        | `/30` | `255.255.255.252` | WAN link to R3                |  `65004`      |
|              | Eth1.10   | `192.168.10.253` | `/24` | `255.255.255.0`   | Gateway for VLAN 10 in Site B |               |
|              | Eth1.20   | `192.168.20.253` | `/24` | `255.255.255.0`   | Gateway for VLAN 20 in Site B |               |
|              | Eth1.30   | `192.168.30.253` | `/24` | `255.255.255.0`   | Gateway for VLAN 30 in Site B |               |
| 💻 **PC-A1** | eth0      | `192.168.10.100` | `/24` | `255.255.255.0`   | Site A - VLAN 10 PC #1        |               |
| 💻 **PC-A2** | eth0      | `192.168.10.101` | `/24` | `255.255.255.0`   | Site A - VLAN 10 PC #2        |               |
| 💻 **PC-A3** | eth0      | `192.168.20.100` | `/24` | `255.255.255.0`   | Site A - VLAN 20 PC #1        |               |
| 💻 **PC-A4** | eth0      | `192.168.20.101` | `/24` | `255.255.255.0`   | Site A - VLAN 20 PC #2        |               |
| 💻 **PC-A5** | eth0      | `192.168.30.100` | `/24` | `255.255.255.0`   | Site A - VLAN 30 PC #1        |               |
| 💻 **PC-A6** | eth0      | `192.168.30.101` | `/24` | `255.255.255.0`   | Site A - VLAN 30 PC #2        |               |
| 💻 **PC-B1** | eth0      | `192.168.10.100` | `/24` | `255.255.255.0`   | Site B - VLAN 10 PC #1        |               |
| 💻 **PC-B2** | eth0      | `192.168.10.101` | `/24` | `255.255.255.0`   | Site B - VLAN 10 PC #2        |               |
| 💻 **PC-B3** | eth0      | `192.168.20.100` | `/24` | `255.255.255.0`   | Site B - VLAN 20 PC #1        |               |
| 💻 **PC-B4** | eth0      | `192.168.20.101` | `/24` | `255.255.255.0`   | Site B - VLAN 20 PC #2        |               |
| 💻 **PC-B5** | eth0      | `192.168.30.100` | `/24` | `255.255.255.0`   | Site B - VLAN 30 PC #1        |               |
| 💻 **PC-B6** | eth0      | `192.168.30.101` | `/24` | `255.255.255.0`   | Site B - VLAN 30 PC #2        |               |


# Configuration

## Routers

### 🌐 R1 – Site A Edge Router - AS 65001

````py
!# Enable, Config & Naming
enable
configure terminal
!
hostname R1-65001

!# WAN link to R2 (BGP peer)
interface Eth0
   description ** LINK TO R2 **
   ip address 1.0.0.1 255.255.255.252
   speed 1000
   duplex full
   no shutdown

!# Trunk to Layer 2 switch SW1
interface Eth1
   description ** TRUNK TO SWITCH SW1 **
   speed 1000
   duplex full
   no shutdown

!# Subinterface for VLAN 10 – Site A
interface Eth1.10
   encapsulation dot1Q 10
   ip address 192.168.10.254 255.255.255.0
   description ** VLAN 10 GATEWAY - SITE A **

!# Subinterface for VLAN 20 – Site A
interface Eth1.20
   encapsulation dot1Q 20
   ip address 192.168.20.254 255.255.255.0
   description ** VLAN 20 GATEWAY - SITE A **

!# Subinterface for VLAN 30 – Site A
interface Eth1.30
   encapsulation dot1Q 30
   ip address 192.168.30.254 255.255.255.0
   description ** VLAN 30 GATEWAY - SITE A **

!# BGP AS 65001 - Peering to R2 and LAN network advertisement
router bgp 65001
   bgp log-neighbor-changes
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
!# Enable, Config & Naming
enable
configure terminal
!
hostname R2-65002

!# Link to R1 (BGP peer)
interface Eth0
   description ** LINK TO R1 **
   ip address 1.0.0.2 255.255.255.252
   speed 1000
   duplex full
   no shutdown

!# Link to R3 (BGP peer)
interface Eth1
   description ** LINK TO R3 **
   ip address 2.0.0.1 255.255.255.252
   speed 1000
   duplex full
   no shutdown

!# BGP AS 65002 - Route transit between R1 and R3
router bgp 65002
   bgp log-neighbor-changes
   neighbor 1.0.0.1 remote-as 65001
   neighbor 2.0.0.2 remote-as 65003


end
wr

!
!


````


### 📡 R3 – Intermediate Router - AS 65003


````py
!# Enable, Config & Naming
enable
configure terminal
!
hostname R3-65003

!# Link to R2 (BGP peer)
interface Eth1
   description ** LINK TO R2 **
   ip address 2.0.0.2 255.255.255.252
   speed 1000
   duplex full
   no shutdown

!# Link to R4 (BGP peer)
interface Eth0
   description ** LINK TO R4 **
   ip address 3.0.0.1 255.255.255.252
   speed 1000
   duplex full
   no shutdown

!# BGP AS 65003 - Route transit between R2 and R4
router bgp 65003
   bgp log-neighbor-changes
   neighbor 2.0.0.1 remote-as 65002
   neighbor 3.0.0.2 remote-as 65004


end
wr

!
!


````

### 🌐 R4 – Site A Edge Router - AS 65004

````py
!# Enable, Config & Naming
enable
configure terminal
!
hostname R3-65004

!# WAN link to R3 (BGP peer)
interface Eth0
   description ** LINK TO R3 **
   ip address 3.0.0.2 255.255.255.252
   speed 1000
   duplex full
   no shutdown

!# Trunk to Layer 2 switch SW2
interface Eth1
   description ** TRUNK TO SWITCH SW2 **
   speed 1000
   duplex full
   no shutdown

!# Subinterface for VLAN 10 – Site B
interface Eth1.10
   encapsulation dot1Q 10
   ip address 192.168.10.253 255.255.255.0
   description ** VLAN 10 GATEWAY - SITE B **

!# Subinterface for VLAN 20 – Site B
interface Eth1.20
   encapsulation dot1Q 20
   ip address 192.168.20.253 255.255.255.0
   description ** VLAN 20 GATEWAY - SITE B **

!# Subinterface for VLAN 30 – Site B
interface Eth1.30
   encapsulation dot1Q 30
   ip address 192.168.30.253 255.255.255.0
   description ** VLAN 30 GATEWAY - SITE B **

!# BGP AS 65004 - Peering to R3 and LAN network advertisement
router bgp 65004
   bgp log-neighbor-changes
   neighbor 3.0.0.1 remote-as 65003
   network 192.168.10.0 mask 255.255.255.0
   network 192.168.20.0 mask 255.255.255.0
   network 192.168.30.0 mask 255.255.255.0


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

!# Access ports for VLAN 10 – PC-A1, PC-A2
interface range Eth0/0 - 1
   description ** VLAN 10 ACCESS - PC-A1 / PC-A2 **
   switchport mode access
   switchport access vlan 10
   speed 1000
   duplex full
   no shutdown

!# Access ports for VLAN 20 – PC-A3, PC-A4
interface range Eth0/2 - 3
   description ** VLAN 20 ACCESS - PC-A3 / PC-A4 **
   switchport mode access
   switchport access vlan 20
   speed 1000
   duplex full
   no shutdown

!# Access ports for VLAN 30 – PC-A5, PC-A6
interface range Eth1/0 - 1
   description ** VLAN 30 ACCESS - PC-A5 / PC-A6 **
   switchport mode access
   switchport access vlan 30
   speed 1000
   duplex full
   no shutdown

!# Trunk port to R1 (Router)
interface Eth2/3
   description ** TRUNK TO R1 ROUTER **
   switchport mode trunk
   switchport trunk encapsulation dot1q
   speed 1000
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
vlan 10
   name VLAN10-SITEB
vlan 20
   name VLAN20-SITEB
vlan 30
   name VLAN30-SITEB

!# Access ports for VLAN 10 – PC-B1, PC-B2
interface range Eth0/0 - 1
   description ** VLAN 10 ACCESS - PC-B1 / PC-B2 **
   switchport mode access
   switchport access vlan 10
   speed 1000
   duplex full
   no shutdown

!# Access ports for VLAN 20 – PC-B3, PC-B4
interface range Eth0/2 - 3
   description ** VLAN 20 ACCESS - PC-B3 / PC-B4 **
   switchport mode access
   switchport access vlan 20
   speed 1000
   duplex full
   no shutdown

!# Access ports for VLAN 30 – PC-B5, PC-B6
interface range Eth1/0 - 1
   description ** VLAN 30 ACCESS - PC-B5 / PC-B6 **
   switchport mode access
   switchport access vlan 30
   speed 1000
   duplex full
   no shutdown

!# Trunk port to R4 (Router)
interface Eth2/3
   description ** TRUNK TO R4 ROUTER **
   switchport mode trunk
   switchport trunk encapsulation dot1q
   speed 1000
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
ip 192.168.10.100/24 192.168.10.1

!


````

### PC-A2

````py
!# PC Name & Static IP
set pcname PC-A2
ip 192.168.10.101/24 192.168.10.1

!


````

### PC-A3

````py
!# PC Name & Static IP
set pcname PC-A3
ip 192.168.20.100/24 192.168.20.1

!


````

### PC-A4

````py
!# PC Name & Static IP
set pcname PC-A4
ip 192.168.20.101/24 192.168.20.1

!


````

### PC-A5

````py
!# PC Name & Static IP
set pcname PC-A5
ip 192.168.30.100/24 192.168.30.1

!


````

### PC-A6

````py
!# PC Name & Static IP
set pcname PC-A6
ip 192.168.30.101/24 192.168.30.1

!


````

---

### PC-B1

````py
!# PC Name & Static IP
set pcname PC-B1
ip 192.168.10.102/24 192.168.10.1

!


````

### PC-B2

````py
!# PC Name & Static IP
set pcname PC-B2
ip 192.168.10.103/24 192.168.10.1

!


````

### PC-B3

````py
!# PC Name & Static IP
set pcname PC-B3
ip 192.168.20.102/24 192.168.20.1

!


````

### PC-B4

````py
!# PC Name & Static IP
set pcname PC-B4
ip 192.168.20.103/24 192.168.20.1

!


````

### PC-B5

````py
!# PC Name & Static IP
set pcname PC-B5
ip 192.168.30.102/24 192.168.30.1

!


````

### PC-B6

````py
!# PC Name & Static IP
set pcname PC-B6
ip 192.168.30.103/24 192.168.30.1

!


````




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





