# 🧠🏗️🌐 Cisco Nexus: `NX-OS - Tier 3 Fabric PT1 : OSPF Overlay + Essential Configs`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `CCNA` `CCNP` `Datacenter` `NX-OS` `Cisco Nexus` `Leaf & Spine` `DC Switching` `Modular Chassis` `Fixed Form Factor` `vPC` `ECMP` `FEX` `Fabric Extender` `Nexus 2000` `Nexus 5000` `Nexus 7000` `Nexus 9000`  `Port-Channel` `LAG` `VLAN` `VRF` `Overlay` `Underlay` `EVPN` `VXLAN`  `Spine Switch` `Leaf Switch` `Control Plane` `Forwarding Plane` `Supervisor Card` `Line Card`  `Redundant Power Supply` `Hot-swappable` `In-Service Software Upgrade (ISSU)` `High Availability`  `Twinax Cable` `SFP+` `QSFP28` `QSFP-DD` `10G` `25G` `40G` `100G` `400G`

---

<br>

# 📄 `Index`



[**🗃️ Resources**](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-resources)

# 🏗️ Cisco Nexus :: `NX-OS - Tier 3 Fabric PT1 : OSPF Overlay + Essential Configs`



## 🎯 Objectives, Features & Protocols Covered



## 🎥 Lab Proof of Concept (PoC) - Video

- [**👉 Click here to go to the PoC video 👈**](https://youtu.be/RL9hBT0H7UE)

## 🗺️ Network Topology

![image](https://github.com/user-attachments/assets/0cf3b58f-4f13-44cb-8bb1-6117a97c8ca9)





## VPC Creation

````py
!##############################################
!#   vPC CONFIGURATION – Nexus BGW-1A & 1B    #
!#   Devices: NX9-BGW-1A / NX9-BGW-1B          #
!#   Purpose: Border Gateways – vPC Peering    #
!#   Mode: Trunk-based Peer-Link               #
!##############################################

!---------------------------------------------
! STEP 1: Enable Required Features (Both Nodes)
!---------------------------------------------
feature vpc                          ! Enables vPC functionality
feature lacp                         ! Enables LACP for Port-Channels

!---------------------------------------------------
! STEP 2: Configure MGMT Interface – Peer Keepalive (BGW-1A)
!---------------------------------------------------
interface mgmt0
  description ** vPC Keepalive – to BGW-1B **
  no shutdown
  ip address 10.10.68.1/24
  vrf member management              ! Use mgmt VRF (out-of-band)
exit

!---------------------------------------------------
! STEP 2: Configure MGMT Interface – Peer Keepalive (BGW-1B)
!---------------------------------------------------
interface mgmt0
  description ** vPC Keepalive – to BGW-1A **
  no shutdown
  ip address 10.10.68.2/24
  vrf member management
exit

!---------------------------------------------------
! STEP 3: Configure Peer-Link Interfaces (Both Nodes)
!---------------------------------------------------

! Interfaces used for Peer-Link (LACP trunk)
interface ethernet 1/6
  description ** vPC Peer-Link to Peer **
  no shutdown
  channel-group 1 mode active        ! Active LACP
exit

interface ethernet 1/7
  description ** vPC Peer-Link to Peer **
  no shutdown
  channel-group 1 mode active
exit

! Create the actual Port-Channel interface for the peer-link
interface port-channel 1
  description ** vPC Peer-Link **
  no shutdown
  switchport
  switchport mode trunk             ! Must be trunk mode
  spanning-tree port type network   ! Best practice for peer-links
  vpc peer-link                      ! Designate as vPC peer-link
  mtu 9216                          ! Recommended: Jumbo frames (if fabric-wide)
exit

!---------------------------------------------------
! STEP 4: Configure vPC Domain – BGW-1A
!---------------------------------------------------
vpc domain 1
  peer-keepalive destination 10.10.68.2 source 10.10.68.1 vrf management
  role priority 100                 ! Lower = Primary vPC peer
  auto-recovery                     ! Optional: Recovers vPC after reload
  system-priority 1000             ! Used for consistency; not mandatory
  peer-link port-channel 1         ! Explicitly bind peer-link (best practice)
exit

!---------------------------------------------------
! STEP 4: Configure vPC Domain – BGW-1B
!---------------------------------------------------
vpc domain 1
  peer-keepalive destination 10.10.68.1 source 10.10.68.2 vrf management
  role priority 200
  auto-recovery
  system-priority 1000
  peer-link port-channel 1
exit

!---------------------------------------------------
! STEP 5: (Optional) Native VLAN Configuration
!---------------------------------------------------
! It’s recommended to configure same native VLAN on both ends to avoid STP loops

interface port-channel 1
  switchport trunk native vlan 999
exit

vlan 999
  name vPC-NATIVE
exit

!---------------------------------------------------
! Verification Commands
!---------------------------------------------------

show vpc brief                        ! Check vPC status
show vpc peer-keepalive              ! Check keepalive status
show port-channel summary            ! LACP/Po status
show interface port-channel 1 trunk  ! Verify trunking
ping 10.10.68.2 vrf management       ! Test keepalive reachability
````


Ping between vpcA: ping 10.10.68.2 vrf management
Ping between vpcB: ping 10.10.68.1 vrf management
Check vPC stgats: show vpc


````
#! Config Port Channel between Switch1-A+Switch1-B & Switch 2
````







## 🥇 `NX9-SWITCH-vPC-A` - (vPC-A)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-A                    #
!#    ROLE  - VPC-A                               #
!#    IP    - 10.10.10.11/24                      #
!#    LOGIN - admin / admin.cisco                 #
!#                                                #
!#    UNDERLAY = OSPF                             #
!#    OSPF 1 / AREA 0                             #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY
configure terminal
hostname NX9-SWITCH-vPC-A
username admin password admin.cisco
cdp enable

!# << vPC STEP1 - FEATURES >>

feature lacp
feature vpc

!# << vPC STEP2 - MGMT INTERFACE CONFIG >>

interface mgmt 0
   description ** vPC Keepalive - SW1A --> SW1B **
   no shutdown
   ip address 10.10.68.1/24
   vrf member management              
exit

!# << vPC STEP3 - vPC PEER-LINK INTERFACE >>

!# - Interfaces used for Peer-Link (LACP trunk)
default interface ethernet 1/6-7
interface ethernet 1/6-7
  description ** vPC Peer-Link to Peer **
  no shutdown
  channel-group 1 mode active
exit

!# - Create the actual Port-Channel interface for the peer-link
interface port-channel 1
  description ** vPC Peer-Link **
  no shutdown
  switchport
  switchport mode trunk    
  spanning-tree port type network  
  vpc peer-link     
  mtu 9216   
exit

!# << vPC STEP 4 - vPC Domain >>

vpc domain 1
  peer-keepalive destination 10.10.68.2 source 10.10.68.1 vrf management
  role priority 100              
  auto-recovery                  
  system-priority 1000             
exit

!# << vPC STEP 5 - NATIVE VLAN FOR VPC PEER (Optional) >>

vlan 99
  name vPC-PEER-NATIVE
exit
interface port-channel 1
  switchport trunk native vlan 99
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-SWITCH-vPC-A
+ IP        =  10.10.10.11

? LOGIN     =  admin / admin.cisco    

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

$

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-vPC-A
copy running-config startup-config

!
!


````


## 🥇 `NX9-SWITCH-vPC-B` - (vPC-B)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-B                    #
!#    ROLE  - VPC-B                               #
!#    IP    - 10.10.10.12/24                      #
!#    LOGIN - admin / admin.cisco                 #
!#                                                #
!#    UNDERLAY = OSPF                             #
!#    OSPF 1 / AREA 0                             #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY
configure terminal
hostname NX9-SWITCH-vPC-B
username admin password admin.cisco
cdp enable

!# << vPC STEP1 - FEATURES >>

feature lacp
feature vpc

!# << vPC STEP2 - MGMT INTERFACE CONFIG >>

interface mgmt 0
   description ** vPC Keepalive - SW1B --> SW1A **
   no shutdown
   ip address 10.10.68.2/24
   vrf member management              
exit

!# << vPC STEP3 - vPC PEER-LINK INTERFACE >>

!# - Interfaces used for Peer-Link (LACP trunk)
default interface ethernet 1/6-7
interface ethernet 1/6-7
  description ** vPC Peer-Link to Peer **
  no shutdown
  channel-group 1 mode active
exit

!# - Create the actual Port-Channel interface for the peer-link
interface port-channel 1
  description ** vPC Peer-Link **
  no shutdown
  switchport
  switchport mode trunk    
  spanning-tree port type network  
  vpc peer-link     
  mtu 9216   
exit

!# << vPC STEP 4 - vPC Domain >>

vpc domain 1
  peer-keepalive destination 10.10.68.1 source 10.10.68.2 vrf management
  role priority 200              
  auto-recovery                  
  system-priority 1000           
exit

!# << vPC STEP 5 - NATIVE VLAN FOR VPC PEER (Optional) >>

vlan 99
  name vPC-PEER-NATIVE
exit
interface port-channel 1
  switchport trunk native vlan 99
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-SWITCH-vPC-B
+ IP        =  10.10.10.12

? LOGIN     =  admin / admin.cisco    

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

$

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-vPC-B
copy running-config startup-config

!
!


````
















# 🗃️ Resources

- https://www.youtube.com/watch?v=fWCH39T-_fU

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
