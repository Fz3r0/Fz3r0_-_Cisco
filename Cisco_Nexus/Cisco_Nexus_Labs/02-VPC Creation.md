# 🧠🏗️🌐 Cisco Nexus:  `NX-OS - vPC Configuration`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `CCNA` `CCNP` `Datacenter` `NX-OS` `Cisco Nexus` `Leaf & Spine` `DC Switching` `Modular Chassis` `Fixed Form Factor` `vPC` `ECMP` `FEX` `Fabric Extender` `Nexus 2000` `Nexus 5000` `Nexus 7000` `Nexus 9000`  `Port-Channel` `LAG` `VLAN` `VRF` `Overlay` `Underlay` `EVPN` `VXLAN`  `Spine Switch` `Leaf Switch` `Control Plane` `Forwarding Plane` `Supervisor Card` `Line Card`  `Redundant Power Supply` `Hot-swappable` `In-Service Software Upgrade (ISSU)` `High Availability`  `Twinax Cable` `SFP+` `QSFP28` `QSFP-DD` `10G` `25G` `40G` `100G` `400G`

---

<br>

# 📄 `Index`

[**🏗️ Cisco Nexus :: `NX-OS - vPC Configuration`**]()
- [🎯 Objectives, Features & Protocols Covered]()
- [🎥 Lab Proof of Concept (PoC) - Video]()
- [🗺️ Network Topology]()
- [📋 Network Device Inventory & IP Addressing]()
- [📝 Lab Notes]()

[**⚙️ Devices Configurations**]() 
- [1️⃣⚙️ vPC CONFIG STEP 1 :: `vPC Domain` + `Peer-Link` + `KeepAlive-Link` @ Switch-vPC-A & Switch-vPC-B]()
    - [🥇 `NX9-SWITCH-vPC-A` - (vPC-A)]()
    - [🥈 `NX9-SWITCH-vPC-B` - (vPC-B)]() 

- [2️⃣⚙️ vPC CONFIG STEP 2 :: `(vPC) Port Channels` @ vPC <<==>> Host (Trunk/SVI)]()
    - [🔛 vPC CONFIG :: vPC <<==>> L3 SVI]() 
        - [🥇 `NX9-SWITCH-vPC-A` - (vPC-A)]()
        - [🥈 `NX9-SWITCH-vPC-B` - (vPC-B)]()
        - [🔀 L3-SWITCH-3 - (Layer 2 Port Channel @ NXOS Switch L3 SVI/TRUNK)]()  <br><br>
    - [🔛 vPC CONFIG :: vPC <<==>> Switch L2]()
        - [🥇 `NX9-SWITCH-vPC-A` - (vPC-A)]()
        - [🥈 `NX9-SWITCH-vPC-B` - (vPC-B)]() 
        - [↔️ L2-SWITCH-2 - (Layer 2 Port Channel @ NXOS Switch L2 TRUNK)]()  <br><br>
    - [🖥️ CONFIG :: Hosts (Servers)]() 
        - [🖥️ SERVER-1-V10-MGMT - (Server 1)]()
        - [🖥️ SERVER-2-V10-MGMT - (Server 2)]()

[**⚠️ vPC / Port-Channel :: `Verification` & `Troubleshooting`**]()
            
[**🗃️ Resources**]()

# 🏗️ Cisco Nexus :: `NX-OS - vPC Configuration`

This lab is a hands-on deployment of a dual-Nexus vPC domain, implementing a highly available, scalable Layer-2 and Layer-3 network fabric using Cisco Nexus 9000 switches and NX-OS. This setup is ideal for data center environments requiring active/active path redundancy, zero-loop bridging, and host-side dual-homing without spanning tree blocking ports.

- You will build a vPC-based topology where: **two Nexus switches act as a single logical switch from the perspective of upstream & downstream access devices**

By completing this deployment, you'll be fully prepared to extend the fabric with advanced technologies like `VXLAN` and `EVPN`.

## 🎯 Objectives, Features & Protocols Covered

By the end of this lab, you will have implemented and verified:

⭕ vPC Design with Nexus 9K:

- Full vPC domain (vpc domain 666) across `NX9-SWITCH-vPC-A` and `NX9-SWITCH-vPC-B`. <br><br>
    - `Peer-link` (Po100)
    - `Keepalive-link` (mgmt0)
    - vPC Domain & Switches priority, role priority, auto-recovery

⭕ Po1 :: `vPC Port-Channels` + `Port Channel` @ `vPC <<==>> Switch-L3 SVI Gateway`:

- `Port-channel-1 + vPC-1` configuration between `vPC & Switch-L3`.
- Port-channels operating in LACP active mode.
- Static routing and default gateways using L3 SVI to simulate uplink connectivity & management
- Trunk between Switch and vPC

⭕ Po2 :: `vPC Port-Channels` + `Port Channel` @ `vPC <<==>> Switch-L2`:

- `Port-channel-2 + vPC-2` configuration between `vPC & Switch-L2`.
- Port-channels operating in LACP active mode.
- Trunk between Switch and vPC

## 🎥 Lab Proof of Concept (PoC) - Video

- [**👉 Click here to go to the PoC video 👈**](https://youtu.be/RL9hBT0H7UE)

## 🗺️ Network Topology

<p align="center"> <img src="https://github.com/user-attachments/assets/c637b6f9-0539-46af-b805-a6b89d4461a5">  </p>

## 📋 Network Device Inventory & IP Addressing

| Device                                | Function          | Interface / VLAN         | IP / CIDR         | Gateway       | *Network*    | *Broadcast*    | Main Description                      |
| ------------------------------------- | ----------------- | ------------------------ | ----------------- | ------------- | ------------ | -------------- | ------------------------------------- |
| **`NX9-SWITCH-vPC-A`**<br>(NXOS)      | vPC Core Peer A   | VLAN 10 (SVI)            | `10.10.10.11/24`  | `10.10.10.1`  | *10.10.10.0* | *10.10.10.255* | Management SVI – Peer A               |
|                                       |                   | Po1 (Eth1/1)             | trunk VLAN 10     | –             | –            | –              | L2 Po to L3-SWITCH-3                  |
|                                       |                   | Po2 (Eth1/2)             | trunk VLAN 10     | –             | –            | –              | L2 Po to L2-SWITCH-2                  |
|                                       |                   | Po100 (Eth1/6-7)         | trunk VLAN ALL    | –             | –            | –              | vPC Peer-Link                         |
|                                       |                   | mgmt0 (Keepalive)        | `10.10.68.1/24`   | `10.10.68.2`  | *10.10.68.0* | *10.10.68.255* | vPC Keepalive (VRF: management)       |
| **`NX9-SWITCH-vPC-B`**<br>(NXOS)      | vPC Core Peer B   | VLAN 10 (SVI)            | `10.10.10.12/24`  | `10.10.10.1`  | *10.10.10.0* | *10.10.10.255* | Management SVI – Peer B               |
|                                       |                   | Po1 (Eth1/1)             | trunk VLAN 10     | –             | –            | –              | L2 Po to L3-SWITCH-3                  |
|                                       |                   | Po2 (Eth1/2)             | trunk VLAN 10     | –             | –            | –              | L2 Po to L2-SWITCH-2                  |
|                                       |                   | Po100 (Eth1/6-7)         | trunk VLAN ALL    | –             | –            | –              | vPC Peer-Link                         |
|                                       |                   | mgmt0 (Keepalive)        | `10.10.68.2/24`   | `10.10.68.1`  | *10.10.68.0* | *10.10.68.255* | vPC Keepalive (VRF: management)       |
| **`L3-SWITCH-3`**<br>(NXOS)           | L3 Gateway Switch | VLAN 10 (SVI)            | `10.10.10.1/24`   | –             | *10.10.10.0* | *10.10.10.255* | Default Gateway for VLAN 10           |
|                                       |                   | Po1 (Eth1/1-2)           | trunk VLAN 10     | –             | –            | –              | Uplink Po to vPC Peers (Po1 on both)  |
|                                       |                   | Eth1/7 (Access)          | access VLAN 10    | –             | –            | –              | Access port to SERVER-1               |
| **`L2-SWITCH-2`**<br>(NXOS)           | L2 Access Switch  | VLAN 10 (SVI – optional) | `10.10.10.13/24`  | `10.10.10.1`  | *10.10.10.0* | *10.10.10.255* | Management SVI (optional)             |
|                                       |                   | Po2 (Eth1/1-2)           | trunk VLAN 10     | –             | –            | –              | Uplink Po to vPC Peers (Po2 on both)  |
|                                       |                   | Eth1/7 (Access)          | access VLAN 10    | –             | –            | –              | Access port to SERVER-2               |
| **`SERVER-1-V10-MGMT`**<br>(IOS Host) | Simulated Host    | Eth0                     | `10.10.10.101/24` | `10.10.10.1`  | *10.10.10.0* | *10.10.10.255* | End-host in VLAN 10 (via L3-SWITCH-3) |
| **`SERVER-2-V10-MGMT`**<br>(IOS Host) | Simulated Host    | Eth0                     | `10.10.10.102/24` | `10.10.10.1`  | *10.10.10.0* | *10.10.10.255* | End-host in VLAN 10 (via L2-SWITCH-2) |

## 📝 Lab Notes

Credentials:

- Admin: `admin`
- Pass: `admin.cisco`

Notes:

- During Nexus Switches start-up, select `skip` during "Abort Power On Auto Provisioning (yes - continue with normal setup, skip - bypass password and basic configuration, no - continue with Power On Auto Provisioning) (yes/skip/no)[no]: `skip`"
- Check that all virtual NX devices are powered on and have no CLI issues; sometimes they can hang or freeze when too many sessions are open or multiple processes are running.
- I tried to reduce LACP timers to do the tests fasters, but it seems that in this version of NXOS is not possible. 

# **⚙️ Devices Configurations**

To fully deploy a vPC setup, there are two key configuration steps:

- 1️⃣ `Step 1` – **Core vPC Setup**: This is the foundational step where you configure two Nexus switches (vPC-A and vPC-B) to act as a single logical switch to downstream devices.

<p align="center"> <img src="https://github.com/user-attachments/assets/ea74660f-8c11-420d-a72e-00cc2a564f22" height="300">  </p>


- 2️⃣ `Step 2` – **Connecting External Devices via vPC Port-Channels**: Now that the vPC core is active, this step is about connecting downstream switches or hosts using vPC-backed Port-Channels.

![image](https://github.com/user-attachments/assets/5e54dc1f-847f-492f-8b16-6030f26be4d8)

_(Copy & Paste the configuration in each device CLI)_

## 1️⃣⚙️ vPC CONFIG STEP 1 :: `vPC Domain` + `Peer-Link` + `KeepAlive-Link` @ `Switch-vPC-A & Switch-vPC-B`

This is the foundational step in building a vPC environment. 

- You’re configuring the logical **vPC domain (Domain 666) between your two Nexus switches so they can act as one virtual switch to downstream devices**.

Once this is in place, both Nexus switches are ready to support multi-chassis port-channels, and you can proceed to **Step 2**, where downstream switches or hosts are connected to the vPC fabric using `vpc <id>` assignments on new `port-channels`.

✅ Mandatory configs that make vPC possible:

- `feature`: vpc	and lacp features must be enabled (For VPC and Port-Channel config)
- `vpc domain <id>`:	Logical group ID that binds both switches into a vPC pair
- `Peer-Link`:	L2 trunk that carries vPC state, control traffic, and data
- `Keepalive-Link`:	L3 link to ensure both switches can detect each other’s health

🧩 Optional configs (But Recommended):

- `role priority`:	Determines which switch becomes the vPC primary (lower wins)
- `auto-recovery`:	Allows a switch to come back up in split-brain scenarios
- `system-priority`:	Affects LACP system negotiation (keep consistent across peers)
- `SVI Gateway & ip route 0.0.0.0`: Basic default route for management plane connectivity & testing purposes

## 🥇 `NX9-SWITCH-vPC-A` - (vPC-A)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-A                    #
!#    ROLE  - VPC-A                               #
!#    IP    - 10.10.10.11/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY
configure terminal
hostname NX9-SWITCH-vPC-A
username admin password admin.cisco
cdp enable

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)
feature interface-vlan
vlan 10
   name VLAN10-MANAGEMENT
interface vlan 10
   no shutdown
   description ** SVI-MGMT-L3-VLAN10-MGMT **
   ip address 10.10.10.11/24 
exit
!# ip default-gateway 10.10.10.1 <- This command is deprecated.
ip route 0.0.0.0/0 10.10.10.1

!# << vPC STEP1 - FEATURES >>

feature vpc
feature lacp

!# << vPC STEP2 - MGMT INTERFACE CONFIG >>

interface mgmt 0
   description ** vPC Keepalive - SW1A --> SW1B **
   no shutdown
   vrf member management
   ip address 10.10.68.1/24
exit

!# << vPC STEP 3 - vPC Domain CONFIG >>

vpc domain 666
  peer-keepalive destination 10.10.68.2 source 10.10.68.1 vrf management
  role priority 100              
  auto-recovery                  
  system-priority 1000             
exit

!# << vPC STEP 4 - vPC PEER-LINK INTERFACE >>

!# - 3.2: Interfaces used for Peer-Link (LACP trunk)
interface ethernet 1/6-7
  description ** vPC Peer-Link to Peer **
  channel-group 100 mode active
  no shutdown
exit

!# << vPC STEP 5 - PORT CHANNEL INTERFACE (For Peer-Link) >>

interface port-channel 100
  description ** vPC Peer-Link **
  no shutdown
  switchport
  switchport mode trunk  
  vpc peer-link     
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

## 🥈 `NX9-SWITCH-vPC-B` - (vPC-B)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-B                    #
!#    ROLE  - VPC-B                               #
!#    IP    - 10.10.10.12/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY
configure terminal
hostname NX9-SWITCH-vPC-B
username admin password admin.cisco
cdp enable

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)
feature interface-vlan
vlan 10
   name VLAN10-MANAGEMENT
interface vlan 10
   no shutdown
   description ** SVI-MGMT-L3-VLAN10-MGMT **
   ip address 10.10.10.12/24 
exit
!# ip default-gateway 10.10.10.1 <- This command is deprecated.
ip route 0.0.0.0/0 10.10.10.1

!# << vPC STEP1 - FEATURES >>

feature vpc
feature lacp

!# << vPC STEP2 - MGMT INTERFACE CONFIG >>

interface mgmt 0
   description ** vPC Keepalive - SW1B --> SW1A **
   no shutdown
   vrf member management
   ip address 10.10.68.2/24
exit

!# << vPC STEP 3 - vPC Domain CONFIG >>

vpc domain 666
  peer-keepalive destination 10.10.68.1 source 10.10.68.2 vrf management
  role priority 200              
  auto-recovery                  
  system-priority 1000             
exit

!# << vPC STEP 4 - vPC PEER-LINK INTERFACE >>

!# - 3.2: Interfaces used for Peer-Link (LACP trunk)
interface ethernet 1/6-7
  description ** vPC Peer-Link to Peer **
  channel-group 100 mode active
  no shutdown
exit

!# << vPC STEP 5 - PORT CHANNEL INTERFACE (For Peer-Link) >>

interface port-channel 100
  description ** vPC Peer-Link **
  no shutdown
  switchport
  switchport mode trunk    
  vpc peer-link     
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

# 2️⃣⚙️ vPC CONFIG STEP 2 :: `(vPC) Port Channels` @ `vPC <<==>> Host` (Trunk/SVI)

Now that the vPC core is up and the peer-link is working, this step is about connecting downstream switches (or hosts) to the vPC domain using Port-Channels + vPC IDs.

- This configu lets a host or switch connect to two physical Nexus devices as if they were a single logical switch, **using a single Port-Channel from its perspective**.

✅ Mandatory Configuration:

- `interface ethernet X/X + channel-group <id>`:	Assign physical interfaces to Po
- `interface port-channel <id> + vpc <id>`:	Define vPC ID on both Nexus peers. **Both IDs must match (eg `Po1` + `VPC1`)**
- `switchport mode trunk`:	Allow VLANs to traverse
- `feature lacp`:	Required for dynamic Port-Channels
- `Matching config`: on downstream switch	Port-channel must match on both sides

# 🔛 vPC CONFIG :: `vPC <<==>> L3 SVI`

![image](https://github.com/user-attachments/assets/cd2e5547-efcb-45d0-9a98-99ad4e776e8b)

## 🥇 `NX9-SWITCH-vPC-A` - (vPC-A)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-A                    #
!#    ROLE  - VPC-A                               #
!#    IP    - 10.10.10.11/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# CONFIGURE TERMINAL
configure terminal

!# << Port Channel : NX9-SWITCH-vPC-A -->> L3-SWITCH-3

!# Set interface used for Host Port-Channel 1 = eth 1/1
interface ethernet 1/1
  description ** Po1 - Host Port Channel  **
  channel-group 1 mode active
  no shutdown
exit

!# Configure Port-Channel 1 on = eth 1/1
interface port-channel 1
  description ** Po1 - Host Port Channel  **
  no shutdown
  switchport
  switchport mode trunk
  duplex full
  vpc 1
exit

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-vPC-A3
copy running-config startup-config


!
!


````

## 🥈 `NX9-SWITCH-vPC-B` - (vPC-B)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-B                    #
!#    ROLE  - VPC-B                               #
!#    IP    - 10.10.10.12/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# CONFIGURE TERMINAL
configure terminal

!# << Port Channel : NX9-SWITCH-vPC-B -->> L3-SWITCH-3

!# Set interface used for Host Port-Channel 1 = eth 1/1
interface ethernet 1/1
  description ** Po1 - Host Port Channel  **
  channel-group 1 mode active
  no shutdown
exit

!# Configure Port-Channel 1 on = eth 1/1
interface port-channel 1
  description ** Po1 - Host Port Channel  **
  no shutdown
  switchport
  switchport mode trunk
  vpc 1
  duplex full
exit

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-vPC-B3
copy running-config startup-config


!
!


````

## 🥇 `L3-SWITCH-3` - (Layer 2 Port Channel @ NXOS Switch L3 SVI/TRUNK)

````py
!##################################################
!#    NEXUS - L3-SWITCH-3                         #
!#    ROLE  - HOST L3                             #
!#    IP    - 10.10.10.1/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY
configure terminal
hostname L3-SWITCH-3
username admin password admin.cisco
cdp enable

ip routing

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)
feature interface-vlan
vlan 10
   name VLAN10-MANAGEMENT
interface vlan 10
   no shutdown
   description ** SVI-MGMT-L3-VLAN10-MGMT **
   ip address 10.10.10.1/24 
exit

!# << FEATURES >>

feature lacp

!# << Port Channel : L3-SWITCH-3 -->> NX9-SWITCH-vPC-A+B

!# Set interface used for Host Port-Channel 2 = eth 1/2
interface ethernet 1/1-2
  description ** Po1 - Host Port Channel  **
  channel-group 1 mode active
  no shutdown
exit

!# Configure Port-Channel 1 on = eth 1/2
interface port-channel 1
  description ** Po1 - Host Port Channel  **
  no shutdown
  switchport
  switchport mode trunk
exit

!# ACCESS INTERFACE TO HOST-A

!# Configure Access Interface @ Host-A - VLAN 10 mgmt
interface ethernet 1/7
  description ** Access Host A  **
  no shutdown
  switchport
  switchport mode access
  switchport access vlan 10
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  L2-SWITCH-2
+ IP        =  10.10.10.1

? LOGIN     =  admin / admin.cisco    

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

$

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-L3-SWITCH-3
copy running-config startup-config

!
!


````

# 🔛 vPC CONFIG :: `vPC <<==>> Switch L2`

![image](https://github.com/user-attachments/assets/1dcdb6b9-9674-4a8d-917e-b865e46a2556)

## 🥇 `NX9-SWITCH-vPC-A` - (vPC-A)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-A                    #
!#    ROLE  - VPC-A                               #
!#    IP    - 10.10.10.11/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# CONFIGURE TERMINAL
configure terminal

!# << Port Channel : NX9-SWITCH-vPC-A -->> L2-SWITCH-2

!# Set interface used for Host Port-Channel 2 = eth 1/2
interface ethernet 1/2
  description ** Po2 - Host Port Channel  **
  channel-group 2 mode active
  no shutdown
exit

!# Configure Port-Channel 2 on = eth 1/2
interface port-channel 2
  description ** Po2 - Host Port Channel  **
  no shutdown
  switchport
  switchport mode trunk
  vpc 2
exit

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-vPC-A2
copy running-config startup-config


!
!


````

## 🥈 `NX9-SWITCH-vPC-B` - (vPC-B)

````py
!##################################################
!#    NEXUS - NX9-SWITCH-vPC-B                    #
!#    ROLE  - VPC-B                               #
!#    IP    - 10.10.10.12/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# CONFIGURE TERMINAL
configure terminal

!# << Port Channel : NX9-SWITCH-vPC-B -->> L2-SWITCH-2

!# Set interface used for Host Port-Channel 2 = eth 1/2
interface ethernet 1/2
  description ** Po2 - Host Port Channel  **
  channel-group 2 mode active
  no shutdown
exit

!# Configure Port-Channel 2 on = eth 1/2
interface port-channel 2
  description ** Po2 - Host Port Channel  **
  no shutdown
  switchport
  switchport mode trunk
  !# Po2 = VPC2
  vpc 2
exit

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-vPC-B2
copy running-config startup-config


!
!


````

## 🥇 `L2-SWITCH-2` - (Layer 2 Port Channel @ NXOS Switch L2 TRUNK)

````py
!##################################################
!#    NEXUS - L2-SWITCH-2                         #
!#    ROLE  - HOST L2                             #
!#    IP    - 10.10.10.13/24                      #
!#    LOGIN - admin / admin.cisco                 #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY
configure terminal
hostname L2-SWITCH-2
username admin password admin.cisco
cdp enable

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)
feature interface-vlan
vlan 10
   name VLAN10-MANAGEMENT
interface vlan 10
   no shutdown
   description ** SVI-MGMT-L3-VLAN10-MGMT **
   ip address 10.10.10.13/24 
exit
!# ip default-gateway 10.10.10.1 <- This command is deprecated.
ip route 0.0.0.0/0 10.10.10.1

!# << FEATURES >>

feature lacp

!# << Port Channel : L2-SWITCH-2 -->> NX9-SWITCH-vPC-B

!# Set interface used for Host Port-Channel 2 = eth 1/2
interface ethernet 1/1-2
  description ** Po2 - Host Port Channel  **
  channel-group 2 mode active
  no shutdown
exit

!# Configure Port-Channel 2 on = eth 1/2
interface port-channel 2
  description ** Po2 - Host Port Channel  **
  no shutdown
  switchport
  switchport mode trunk
exit

!# ACCESS INTERFACE TO HOST-B

!# Configure Access Interface @ Host-B - VLAN 10 mgmt
interface ethernet 1/7
  description ** Access Host B  **
  no shutdown
  switchport
  switchport mode access
  switchport access vlan 10
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  L2-SWITCH-2
+ IP        =  10.10.10.13

? LOGIN     =  admin / admin.cisco    

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

$

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-L2-SWITCH-2
copy running-config startup-config

!
!


````

# 🖥️ CONFIG :: `Hosts` (Servers)

This section adds basic L3 endpoints (servers) to simulate north-south traffic flows through the fabric. These devices are not running any routing protocol, just a static IP and default route. They help verify:

- ✅ End-to-end IP connectivity
- ✅ vPC Port-Channel forwarding
- ✅ SVI gateway reachability
- ✅ LACP behavior during link failures

Each server is configured as a simple host with a static IP and default gateway (e.g. 10.10.10.1), representing real-world clients or services connected to your access layer.

_These configs are intentionally minimal, just enough to test pings, routing, and basic traffic across the vPC fabric._

## 🖥️ `SERVER-1-V10-MGMT` - (Server 1)

````py
! ############
! # SERVER-1 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-1-V10-MGMT

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 10.10.10.101 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 10.10.10.1

! # Exit & Save
end
write memory


!
!


````

## 🖥️ `SERVER-2-V10-MGMT` - (Server 2)

````py
! ############
! # SERVER-2 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-2-V10-MGMT

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 10.10.10.102 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 10.10.10.1

! # Exit & Save
end
write memory


!
!


````

# ⚠️ vPC / Port-Channel :: `Verification` & `Troubleshooting`

💡 Replace <id> with the relevant Port-Channel number (100 for peer-link, 1, 2, etc. for host Po), and <slot/port> with actual interface (e.g. ethernet 1/2).

## ✅ vPC Core, Domain & Peer Status

````py
!# Overall vPC status and adjacency
show vpc                               

!# Role assignment: primary / secondary
show vpc role                          

!# Summary of all vPCs and port-channels
show vpc brief                         

!# Status of vPC keepalive link
show vpc peer-keepalive                

!# Global consistency checks
show vpc consistency-parameters global 

!# Per-vPC consistency
show vpc consistency-parameters interface port-channel <id>

!# Check mgmt0 interface IP and up/down
show ip interface brief vrf management          

!# Ping from vPC-A to vPC-B
ping 10.10.68.2 vrf management                  

!# Ping from vPC-B to vPC-A
ping 10.10.68.1 vrf management  

````

## ✅ Port-Channel (Po) Status & Interfaces

````py
!# Summary of all port-channels and members
show port-channel summary

!# Detailed info for specific Po (e.g. Po1, Po2, Po100)
show interface port-channel <id>       

!# Trunk status, allowed VLANs
show interface port-channel <id> trunk 

!# Physical interface status (e.g. Eth1/2)
show interface ethernet <slot/port>    

!# LACP Neighbors
show lacp neighbor interface port-channel <id>  !# Check LACP status per Po

````

# 🗃️ Resources

- https://www.cisco.com/c/es_mx/support/docs/switches/nexus-9000-series-switches/218333-understand-and-configure-nexus-9000-vpc.html
- https://www.youtube.com/watch?v=fWCH39T-_fU
- https://youtu.be/kOb0r_-26Lw?si=XM5VzoYBd4AHofUf

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
