# 🧠🏗️🌐 Cisco Nexus: `NX-OS - DC Collapsed Core (HA)`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `CCNA` `CCNP` `Datacenter` `NX-OS` `Cisco Nexus` `Leaf & Spine` `DC Switching` `Modular Chassis` `Fixed Form Factor` `vPC` `ECMP` `FEX` `Fabric Extender` `Nexus 2000` `Nexus 5000` `Nexus 7000` `Nexus 9000`  `Port-Channel` `LAG` `VLAN` `VRF` `Overlay` `Underlay` `EVPN` `VXLAN`  `Spine Switch` `Leaf Switch` `Control Plane` `Forwarding Plane` `Supervisor Card` `Line Card`  `Redundant Power Supply` `Hot-swappable` `In-Service Software Upgrade (ISSU)` `High Availability`  `Twinax Cable` `SFP+` `QSFP28` `QSFP-DD` `10G` `25G` `40G` `100G` `400G`

---

<br>

# 📄 `Index`

[**🏗️ Cisco Nexus :: NX-OS - DC Collapsed Core  (HA)**](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-cisco-nexus--nx-os---dc-collapsed-core-ha)  
- [🎯 Objectives, Features & Protocols Covered](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-objectives-features--protocols-covered)  
- [🗺️ Network Topology](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-network-topology)  
- [📋 Network Device Inventory & IP Addressing](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-network-device-inventory--ip-addressing)  
- [📝 Lab Notes](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-lab-notes)  

[**⚙️ Devices Configurations**](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-devices-configurations)
- [🥇 NX9-1-CR-ACT - (Switch NX9-1 – CORE: ACTIVE HSRP (Priority 200))](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-nx9-1-cr-act---switch-nx9-1---core-active-hsrp-priority-200)
- [🥈 NX9-2-CR-STB - (Switch NX9-2 – CORE: STAND-BY HSRP (Priority 100))](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-nx9-2-cr-stb---switch-nx9-2---core-stand-by-hsrp-priority-100)
- [↔️ NX9-11-ACCESS - (Switch NX9-11 – ACCESS)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-nx9-11-access---switch-nx9-11---access)
- [↔️ NX9-12-ACCESS - (Switch NX9-12 – ACCESS)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-nx9-12-access---switch-nx9-12---access)
- [↔️ NX9-13-ACCESS - (Switch NX9-13 – ACCESS)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-nx9-13-access---switch-nx9-13---access)
- [↔️ NX9-14-ACCESS - (Switch NX9-14 – ACCESS)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-nx9-14-access---switch-nx9-14---access)
- [🔀 RT-1-EDGE - (Router 1 Edge)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-rt-1-edge---router-1-edge)
- [🔀 RT-2-EDGE - (Router 2 Edge)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-rt-2-edge---router-2-edge)
- [🌎 WAN-1 - (ACTIVE Internet Circuit)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-wan-1---active-internet-circuit)
- [🌎 WAN-2 - (STAND-BY Internet Circuit)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-wan-2---stand-by-internet-circuit)
- [🕸️ MPLS-1 - (ACTIVE MPLS Circuit)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-mpls-1---active-mpls-circuit)
- [🕸️ MPLS-2 - (STAND-BY MPLS Circuit)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-mpls-2---stand-by-mpls-circuit)
- [🖥️ SERVER-1-V10BLUE - (Server 1)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-server-1-v10blue---server-1)
- [🖥️ SERVER-2-V20RED - (Server 2)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-server-2-v20red---server-2)
- [🖥️ SERVER-3-V30GREEN - (Server 3)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-server-3-v30green---server-3)
- [🖥️ SERVER-4-V10BLUE - (Server 4)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-server-4-v10blue---server-4)
- [🖥️ SERVER-5-V20RED - (Server 5)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-server-5-v20red---server-5)
- [🖥️ SERVER-6-V30GREEN - (Server 6)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#%EF%B8%8F-server-6-v30green---server-6)
- [📡 CP-OOB-1 - (Cradlepoint Out-Of-Band Management 1)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-cp-oob-1---cradlepoint-out-of-band-management-1)
- [📡 CP-OOB-2 - (Cradlepoint Out-Of-Band Management 2)](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-cp-oob-2---cradlepoint-out-of-band-management-2)

[**🎥 Resources**](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco_Nexus/Cisco_Nexus_Labs/01-F0-Nexus_DC-CollapsedCore-HA.md#-resources)

# 🏗️ Cisco Nexus :: `NX-OS - DC Collapsed Core (HA)`

This lab is a hands-on, end-to-end deployment of a fully functional, two-tier collapsed Core/Distribution Nexus data center topology. It is designed for engineers migrating from IOS/Catalyst to NX-OS/Nexus platforms. You will build a resilient, high-availability environment that includes:

- ✅ **Core Layer (Collapsed Core/Distribution):** Two Cisco Nexus 9000 Series switches (NX9-1 and NX9-2) running NX-OS, providing both Layer-3 routing (SVIs, HSRP, OSPF) and Layer-2 switching (Rapid PVST+, LACP port-channels) functions. <br><br>
- ✅ **Edge Routers:**  Two Cisco IOS routers (RT-1-EDGE and RT-2-EDGE) simulating data-center border routers. Each edge router:  
    - Peers with both Nexus cores over OSPF point-to-point links (primary and secondary paths with different OSPF costs).  
    - Peers with its sibling edge router over an OSPF “edge-to-edge” link for redundancy.  
    - Connects to a simulated WAN circuit via NAT overload (Internet) and to a simulated MPLS circuit via static routes. <br><br>
- ✅ **Access Layer:** Four Cisco Nexus 9000 Series switches (NX9-11, NX9-12, NX9-13, NX9-14) running NX-OS in pure Layer-2 mode. Each access switch hosts one or two Linux end-hosts or servers in VLANs 10 (blue), 20 (red), and 30 (green). <br><br>
- ✅ **End-Hosts:** Six Linux VMs (Server-1 through Server-6) acting as PC/Server endpoints in their respective VLANs, each with a default gateway pointing at the HSRP VIPs on the Nexus cores. <br><br>
- ✅ **Management & Out-Of-Band (OOB):** Two Cradlepoint devices (CP-OOB-1 and CP-OOB-2) connected to the dedicated management ports of the Nexus cores, each in a separate “management” VRF for true out-of-band administration.

Throughout this lab you will learn NX-OS CLI conventions, feature activation, and new configuration paradigms, while still applying many familiar IOS-style commands and procedures. 

- 🏆 **You’ll end with a fully operational, high-availability data center fabric and upon completion, you’ll be ready to tackle advanced features like `VRF`, `VPC`, `VXLAN` or `EVPN`.**

## 🎯 Objectives, Features & Protocols Covered

By the end of this lab, you will have configured and verified:

1. ⭕️ **NX-OS Feature Management & Naming:**
   - Enable/disable NX-OS features (`feature interface-vlan`, `feature hsrp`, `feature ospf`, `feature lacp`, `feature lldp`, etc.).
   - Understand NX-OS hostname and user/role configuration (password strength, custom roles).

2. ⭕️ **High-Availability Gateway (HSRP v2):**
   - Create SVIs for VLANs 10, 20, and 30 on both NX9 cores.
   - Configure HSRP groups (v2) with matching VIPs across NX9-1 (priority 200) and NX9-2 (priority 100).
   - Validate active/standby states and seamless failover for end-host default gateways.

3. ⭕️ **Dynamic Layer-3 Connectivity (OSPF Area 0):**
   - Enable and tune OSPF on NX9 cores and IOS edge routers.
   - Build full-mesh OSPF adjacencies:  
     - Core ↔ Edge links (cost manipulation for primary/backup paths).  
     - Edge ↔ Edge link for resilience.
   - Advertise VLAN networks (`192.168.10.0/24`, `192.168.20.0/24`, `192.168.30.0/24`), WAN circuits (`123.1.1.0/30`, `123.2.2.0/30`), and MPLS static circuits (`10.100.0.0/30`).
   - Inject default route into OSPF and redistribute static MPLS routes.

4. ⭕️ **Layer-2 Resiliency & Spanning-Tree (Rapid PVST+):**
   - Enable Rapid PVST+ globally.
   - Designate root-primary and root-secondary on VLANs 10, 20, 30 across NX9 cores.
   - Configure edge ports (PortFast) and network ports (Bridge Assurance) on access layer.

5. ⭕️ **Port-Channel (LACP) Bundling:**
   - Create Layer-2 port-channels on NX9 cores and access switches for trunk uplinks.
   - Create Layer-3 port-channels for core-to-core and core-to-edge interconnects.
   - Set LACP parameters (`max-bundle`, `min-links`, `port-priority`, `load-balance`).

6. ⭕️ **SVI Gateway Implementation:**
   - Build SVIs as logical L3 interfaces for VLANs 10, 20, and 30.
   - Assign IPs, enable OSPF on SVIs, and integrate HSRP for gateway redundancy.

7. ⭕️ **Static & Default Routing:**
   - Configure static default routes on NX9 cores toward WAN interfaces.
   - Configure static MPLS routes on IOS routers to simulate remote networks.
   - Redistribute static routes into OSPF on edge routers.

8. ⭕️ **NAT Overload (PAT) on Edge Routers:**
   - Designate WAN facing interfaces as `ip nat outside`.
   - Designate SVIs and core links as `ip nat inside`.
   - Create ACLs to match internal subnets for NAT.
   - Overload internal traffic to a single public IP.

9. ⭕️ **Out-Of-Band (OOB) Management:**
   - Configure dedicated `mgmt0` interfaces on both NX9 cores.
   - Place management interfaces in a separate VRF (`management`).
   - Assign IP addresses (`192.168.0.1/24`) and default routes pointing to Cradlepoint.
   - Secure and verify OOB connectivity (ping from management VRF, `show vrf interfaces`).

10. ⭕️ **Telnet & SSH Access Hardening:**
    - Enable Telnet and SSH features under NX-OS.
    - Assign an L2 or L3 interface for admin access.
    - Harden VTY lines (session limits, timeouts, ACLs).

11. ⭕️ **Discovery Protocols (CDP & LLDP):**
    - Enable CDP globally and per-interface for Cisco neighbor discovery.
    - Enable LLDP feature and fine-tune LLDP transmit/receive on individual ports.

12. ⭕️ **File Management & Saving:**
    - Use `show tech-support`, `show file`, `dir bootflash:`.
    - Save configurations to startup-config, take checkpoints (`checkpoint <name>`), and roll back if required.

13. ⭕️ **Collapsed Core/Distribution Architecture:**
    - Understand the benefits of collapsing core and distribution onto Nexus 9Ks.
    - Integrate Layer-2 access switches directly to the dual core/distribution pair via LACP port-channels.
    - Maintain a clear separation of VLAN domains and routing domains.

## 🗺️ Network Topology

![image](https://github.com/user-attachments/assets/9b00d3ab-fe67-4c33-a86f-b93bb7a59a5f)

## 📋 Network Device Inventory & IP Addressing

| Device                                 | Function                    | Interface / VLAN                             | IP (Mask)           | VIP / Gateway    | *Network*        | *Broadcast*        | Main Description                                 |
| -------------------------------------- | --------------------------- | --------------------------------------------- | ------------------- | ---------------- | ---------------- | ------------------ | ------------------------------------------------ |
| **`NX9-1-CR-ACT`**<br>(NXOS)           | Core L3/L2 (Active HSRP)    | VLAN 10 (SVI)                                 | `192.168.10.254/24` | `192.168.10.1`   | *192.168.10.0*   | *192.168.10.255*   | SVI GW VLAN 10–BLUE                              |
|                                        |                             | VLAN 20 (SVI)                                 | `192.168.20.254/24` | `192.168.20.1`   | *192.168.20.0*   | *192.168.20.255*   | SVI GW VLAN 20–RED                               |
|                                        |                             | VLAN 30 (SVI)                                 | `192.168.30.254/24` | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | SVI GW VLAN 30–GREEN-MGT                         |
|                                        |                             | Po1 (E: 1/3, 1/5, 1/6) (L3 Port-Channel)       | `10.50.0.1/30`      | –                | *10.50.0.0*      | *10.50.0.3*        | L3 Po to NX9-2 (OSPF, cost 100)                  |
|                                        |                             | Eth1/1 (P2P OSPF)                             | `10.10.0.2/30`      | –                | *10.10.0.0*      | *10.10.0.3*        | OSPF primary link to RT-1 (cost 1)               |
|                                        |                             | Eth1/2 (P2P OSPF)                             | `10.40.0.1/30`      | –                | *10.40.0.0*      | *10.40.0.3*        | OSPF backup link to RT-2 (cost 100)              |
|                                        |                             | Eth1/4, Eth1/7 (trunks VLAN 10,20,30)          | trunk VLAN 10,20,30 | –                | –                | –                  | L2 uplinks to Access Layer                       |
|                                        |                             | Management (mgmt0)                            | `192.168.0.1/24`    | `192.168.0.2`    | *192.168.0.0*    | *192.168.0.255*    | OOB Mgmt via Cradlepoint (OOB)                   |
| **`NX9-2-CR-STB`**<br>(NXOS)           | Core L3/L2 (Standby HSRP)   | VLAN 10 (SVI)                                 | `192.168.10.253/24` | `192.168.10.1`   | *192.168.10.0*   | *192.168.10.255*   | SVI GW VLAN 10–BLUE                              |
|                                        |                             | VLAN 20 (SVI)                                 | `192.168.20.253/24` | `192.168.20.1`   | *192.168.20.0*   | *192.168.20.255*   | SVI GW VLAN 20–RED                               |
|                                        |                             | VLAN 30 (SVI)                                 | `192.168.30.253/24` | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | SVI GW VLAN 30–GREEN-MGT                         |
|                                        |                             | Po1 (E: 1/3, 1/5, 1/6) (L3 Port-Channel)       | `10.50.0.2/30`      | –                | *10.50.0.0*      | *10.50.0.3*        | L3 Po to NX9-1 (OSPF, cost 100)                  |
|                                        |                             | Eth1/1 (P2P OSPF)                             | `10.20.0.2/30`      | –                | *10.20.0.0*      | *10.20.0.3*        | OSPF primary link to RT-2 (cost 1)               |
|                                        |                             | Eth1/2 (P2P OSPF)                             | `10.30.0.2/30`      | –                | *10.30.0.0*      | *10.30.0.3*        | OSPF backup link to RT-1 (cost 100)              |
|                                        |                             | Eth1/4, Eth1/7 (trunks VLAN 10,20,30)          | trunk VLAN 10,20,30 | –                | –                | –                  | L2 uplinks to Access Layer                       |
|                                        |                             | Management (mgmt0)                            | `192.168.0.2/24`    | `192.168.0.1`    | *192.168.0.0*    | *192.168.0.255*    | OOB Mgmt via Cradlepoint (OOB)                   |
| **`NX9-11-ACCESS`**<br>(NXOS)          | L2 Access Switch            | VLAN 30 (SVI mgmt)                            | `192.168.30.11/24`  | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Management – default-route to HSRP Core          |
|                                        |                             | Po1 (E: 1/1, 1/2, 1/3) (L2 Port-Channel)       | trunk VLAN 10,20,30 | –                | –                | –                  | Redundant uplink(s) to Core via LACP             |
|                                        |                             | Eth1/4 (trunk)                                | trunk VLAN 10,20,30 | –                | –                | –                  | Additional uplink to Core                        |
|                                        |                             | Eth1/5 (access VLAN 10)                       | `192.168.10.x/24`   | `192.168.10.1`   | *192.168.10.0*   | *192.168.10.255*   | Access port VLAN 10                              |
|                                        |                             | Eth1/6 (access VLAN 20)                       | `192.168.20.x/24`   | `192.168.20.1`   | *192.168.20.0*   | *192.168.20.255*   | Access port VLAN 20                              |
|                                        |                             | Eth1/7 (access VLAN 30)                       | `192.168.30.x/24`   | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Access port VLAN 30                              |
| **`NX9-12-ACCESS`**<br>(NXOS)          | L2 Access Switch            | VLAN 30 (SVI mgmt)                            | `192.168.30.12/24`  | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Management – default-route to HSRP Core          |
|                                        |                             | Po1 (E: 1/1, 1/2, 1/3) (L2 Port-Channel)       | trunk VLAN 10,20,30 | –                | –                | –                  | Redundant uplink #1 to Core via LACP             |
|                                        |                             | Po2 (E: 1/4, 1/5, 1/6) (L2 Port-Channel)       | trunk VLAN 10,20,30 | –                | –                | –                  | Redundant uplink #2 to Core via LACP             |
|                                        |                             | Eth1/7 (trunk)                                | trunk VLAN 10,20,30 | –                | –                | –                  | Additional uplink to Core                        |
| **`NX9-13-ACCESS`**<br>(NXOS)          | L2 Access Switch            | VLAN 30 (SVI mgmt)                            | `192.168.30.13/24`  | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Management – default-route to HSRP Core          |
|                                        |                             | Po1 (E: 1/1, 1/2, 1/3) (L2 Port-Channel)       | trunk VLAN 10,20,30 | –                | –                | –                  | Redundant uplink #1 to Core via LACP             |
|                                        |                             | Po2 (E: 1/4, 1/5, 1/6) (L2 Port-Channel)       | trunk VLAN 10,20,30 | –                | –                | –                  | Redundant uplink #2 to Core via LACP             |
|                                        |                             | Eth1/7 (trunk)                                | trunk VLAN 10,20,30 | –                | –                | –                  | Additional uplink to Core                        |
| **`NX9-14-ACCESS`**<br>(NXOS)          | L2 Access Switch            | VLAN 30 (SVI mgmt)                            | `192.168.30.14/24`  | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Management – default-route to HSRP Core          |
|                                        |                             | Po1 (E: 1/1, 1/2, 1/3) (L2 Port-Channel)       | trunk VLAN 10,20,30 | –                | –                | –                  | Redundant uplink to Core via LACP                |
|                                        |                             | Eth1/4 (trunk)                                | trunk VLAN 10,20,30 | –                | –                | –                  | Additional uplink to Core                        |
|                                        |                             | Eth1/5 (access VLAN 10)                       | `192.168.10.x/24`   | `192.168.10.1`   | *192.168.10.0*   | *192.168.10.255*   | Access port VLAN 10                              |
|                                        |                             | Eth1/6 (access VLAN 20)                       | `192.168.20.x/24`   | `192.168.20.1`   | *192.168.20.0*   | *192.168.20.255*   | Access port VLAN 20                              |
|                                        |                             | Eth1/7 (access VLAN 30)                       | `192.168.30.x/24`   | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Access port VLAN 30                              |
| **`RT-1-EDGE`**<br>(IOS)               | Edge Router (WAN/MPLS/Core) | Eth0/0 (WAN-1)                               | `123.1.1.2/30`      | –                | *123.1.1.0*      | *123.1.1.3*        | NAT outside → default via `123.1.1.1`            |
|                                        |                             | Eth0/1 (P2P OSPF to NX9-1)                    | `10.10.0.1/30`      | –                | *10.10.0.0*      | *10.10.0.3*        | OSPF cost 1                                     |
|                                        |                             | Eth0/2 (P2P OSPF to NX9-2)                    | `10.30.0.1/30`      | –                | *10.30.0.0*      | *10.30.0.3*        | OSPF cost 100                                   |
|                                        |                             | Eth0/3 (P2P OSPF to RT-2)                     | `10.60.0.1/30`      | –                | *10.60.0.0*      | *10.60.0.3*        | OSPF cost 200 (Edge-to-Edge)                    |
|                                        |                             | Eth1/0 (MPLS-1)                              | `10.100.0.2/30`     | –                | *10.100.0.0*     | *10.100.0.3*       | Connected to MPLS-1                             |
|                                        |                             | OSPF process                                | router-id `3.3.3.3` | –                | –                | –                  | Redistributes static routes (MPLS)              |
| **`RT-2-EDGE`**<br>(IOS)               | Edge Router (WAN/MPLS/Core) | Eth0/0 (WAN-2)                               | `123.2.2.2/30`      | –                | *123.2.2.0*      | *123.2.2.3*        | NAT outside → default via `123.2.2.1`            |
|                                        |                             | Eth0/1 (P2P OSPF to NX9-2)                    | `10.20.0.1/30`      | –                | *10.20.0.0*      | *10.20.0.3*        | OSPF cost 1                                     |
|                                        |                             | Eth0/2 (P2P OSPF to NX9-1)                    | `10.40.0.2/30`      | –                | *10.40.0.0*      | *10.40.0.3*        | OSPF cost 100                                   |
|                                        |                             | Eth0/3 (P2P OSPF to RT-1)                     | `10.60.0.2/30`      | –                | *10.60.0.0*      | *10.60.0.3*        | OSPF cost 200 (Edge-to-Edge)                    |
|                                        |                             | Eth1/0 (MPLS-2)                              | `10.100.0.6/30`     | –                | *10.100.0.4*     | *10.100.0.7*       | Connected to MPLS-2                             |
|                                        |                             | OSPF process                                | router-id `4.4.4.4` | –                | –                | –                  | Redistributes static routes (MPLS)              |
| **`WAN-1 (IOS)`**<br>(IOS)             | Simulated WAN Router        | Eth0/0                                       | `123.1.1.1/30`      | –                | *123.1.1.0*      | *123.1.1.3*        | Loopbacks `8.8.8.8/32`, `8.8.4.4/32`            |
| **`WAN-2 (IOS)`**<br>(IOS)             | Simulated WAN Router        | Eth0/0                                       | `123.2.2.1/30`      | –                | *123.2.2.0*      | *123.2.2.3*        | Loopbacks `8.8.8.8/32`, `8.8.4.4/32`            |
| **`MPLS-1 (IOS)`**<br>(IOS)            | Simulated MPLS Router       | Eth0/0                                       | `10.100.0.1/30`     | –                | *10.100.0.0*     | *10.100.0.3*       | Loopbacks `10.200.0.100/32`, `10.210.0.100/32`   |
| **`MPLS-2 (IOS)`**<br>(IOS)            | Simulated MPLS Router       | Eth0/0                                       | `10.100.0.5/30`     | –                | *10.100.0.4*     | *10.100.0.7*       | Loopbacks `10.200.0.100/32`, `10.210.0.100/32`   |
| **`Server-1`**<br>(Linux)              | Host VLAN 10                | Eth0/0                                       | `192.168.10.101/24` | `192.168.10.1`   | *192.168.10.0*   | *192.168.10.255*   | Default-route to HSRP VIP Core                 |
| **`Server-2`**<br>(Linux)              | Host VLAN 20                | Eth0/0                                       | `192.168.20.101/24` | `192.168.20.1`   | *192.168.20.0*   | *192.168.20.255*   | Default-route to HSRP VIP Core                 |
| **`Server-3`**<br>(Linux)              | Host VLAN 30                | Eth0/0                                       | `192.168.30.101/24` | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Default-route to HSRP VIP Core                 |
| **`Server-4`**<br>(Linux)              | Host VLAN 10                | Eth0/0                                       | `192.168.10.102/24` | `192.168.10.1`   | *192.168.10.0*   | *192.168.10.255*   | Default-route to HSRP VIP Core                 |
| **`Server-5`**<br>(Linux)              | Host VLAN 20                | Eth0/0                                       | `192.168.20.102/24` | `192.168.20.1`   | *192.168.20.0*   | *192.168.20.255*   | Default-route to HSRP VIP Core                 |
| **`Server-6`**<br>(Linux)              | Host VLAN 30                | Eth0/0                                       | `192.168.30.102/24` | `192.168.30.1`   | *192.168.30.0*   | *192.168.30.255*   | Default-route to HSRP VIP Core                 |
| **`CP-OOB-1`**<br>(Cradlepoint)         | OOB Management Device       | Eth0/0 (mgmt)                                | `192.168.0.2/24`    | `192.168.0.1`    | *192.168.0.0*    | *192.168.0.255*    | Connected to NX9-1 mgmt0 (OOB)                 |
| **`CP-OOB-2`**<br>(Cradlepoint)         | OOB Management Device       | Eth0/0 (mgmt)                                | `192.168.0.3/24`    | `192.168.0.2`    | *192.168.0.0*    | *192.168.0.255*    | Connected to NX9-2 mgmt0 (OOB)                 |

## 📝 Lab Notes

Credentials:

- Admin: `admin`
- Pass: `Adm1n.C1sc0`

Notes:

- During Nexus Switches start-up, select `skip` during "Abort Power On Auto Provisioning (yes - continue with normal setup, skip - bypass password and basic configuration, no - continue with Power On Auto Provisioning) (yes/skip/no)[no]: `skip`"
- The NX9-1 and NX9-2 must be powered on and configured before the edge routers, otherwise OSPF process 1 could bug/error.
- Check that all virtual NX devices are powered on and have no CLI issues; sometimes they can hang or freeze when too many sessions are open or multiple processes are running.

# **⚙️ Devices Configurations**

- Copy & Paste the configuration in each device CLI

## 🥇 `NX9-1-CR-ACT` - (Switch NX9-1 - CORE: ACTIVE HSRP (Priority 200))

````py
!##################################################
!#    NEXUS - NX9-1-CORE-ACTIVE                   #
!#    IP    - 192.168.30.254/24                   #
!#          - 192.168.30.1 (VIP-HSRP)             #
!#    LAYER 3 + LAYER 2                           #
!#    OSPF -> LAN+P2P @ Area 0 / OSPF 1           #
!#    HSRP = ACTIVE @ x.x.x.254 / Priority 200    #
!#              VIP @ x.x.x.1                     #
!#    STP  = PRIMARY / Root Bridge                #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname NX9-1-CR-ACT
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

!# FEATURES

feature interface-vlan
feature ospf
feature hsrp
feature telnet
feature ssh
feature lldp
feature lacp

!# VLANs

vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
vlan 99
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native

!# RPVSTP+ (ROOT-PRIMARY)

spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30 root primary

!# Enable OSPF 1
router ospf 1
   router-id 1.1.1.1
   log-adjacency-changes
exit

#! SVIs (GATEWAY L3) {OSPF AREA 0}

interface vlan 10
   no shutdown
   description ** SVI+GW-L3-VLAN10-BLUE **
   ip address 192.168.10.254/24
   ip router ospf 1 area 0
exit
interface vlan 20
   no shutdown
   description ** SVI+GW-L3-VLAN20-RED **
   ip address 192.168.20.254/24
   ip router ospf 1 area 0
exit
interface vlan 30
   no shutdown
   description ** SVI+GW-L3-VLAN30-GREEN **
   ip address 192.168.30.254/24
   ip router ospf 1 area 0
exit

!# L3 PORT CHANNELS (no switchport)

default interface ethernet 1/5-6,ethernet 1/3
interface ethernet 1/5-6,ethernet 1/3
   description ** L3-Port-Channel-0-Po0-Interfaces **
   no shutdown
   no switchport
   speed 1000
   duplex full
   channel-group 1 mode active
exit
interface port-channel 1
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/3
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

#! L3 WAN INTERFACES @ INTERNET + OSPF FULL MESH

#! OSPF BEST PREFERENCE (1) @ RT1 >> @ WAN 1 
interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 10.10.0.2/30
   speed 1000
   duplex full
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 1
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable
exit

#! OSPF WORST PREFERENCE (100) @ RT2 (WAN2)
interface ethernet 1/2
   no shutdown
   no switchport
   description ** OSPF-BACKUP-TO-RT-2 **
   ip address 10.40.0.1/30
   speed 1000
   duplex full
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 100
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable
exit

#! OSPF MEDIUM PREFERENCE (10) @ NX9-2 [Port Channel 1]
interface port-channel 1
   no shutdown
   no switchport
   description ** OSPF-BACKUP-NX1-TO-NX2-PORT-CHANNEL **
   ip address 10.50.0.1/30
   speed 1000
   duplex full
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 100
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable

#! Configure HSRP (FOR EACH VLAN) (BOTH SWITCHES = SAME VIP ;)) {ACTIVE = PRIORITY 200}

interface vlan 10
   !# Select version 2
   hsrp version 2
   !# Create Group
   hsrp 10
   !# Configure VIP
   ip 192.168.10.1
   !# Highest priprity will be primary
   preempt
   priority 200
exit
interface vlan 20
   !# Select version 2
   hsrp version 2
   !# Create Group
   hsrp 20
   !# Configure VIP
   ip 192.168.20.1
   !# Highest priprity will be primary
   preempt
   priority 200
exit
interface vlan 30
   !# Select version 2
   hsrp version 2
   !# Create Group
   hsrp 30
   !# Configure VIP
   ip 192.168.30.1
   !# Highest priprity will be primary
   preempt
   priority 200
exit

!# L2 INTERFACES - TRUNK (STP = NETWORK)

interface ethernet1/4,ethernet1/7
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable
exit

!# OOB MANAGEMENT INTERFACE - MGMT -> CRADLEPOINT

interface mgmt 0
  no shutdown
  description ** OOB Management Interface **
  ip address 192.168.0.1/24
exit

vrf context management
   ip route 0.0.0.0/0 192.168.0.2
exit

!# TELNET & SSH #

line vty
   session-limit 5
   exec-timeout 3
exit
ip access-list remote-access-users
   permit ip 192.168.0.0/24 any
   permit ip 192.168.30.0/24 any
   permit ip host 192.168.10.101 any
exit  
line vty
   access-class remote-access-users in
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-1 
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.254
+ VIP(HSRP) =  192.168.30.1 

? login     :  admin
? Password  :  Adm1n.C1sc0

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

#
$

!############################################################################################
!                                                                                           #
!          @@@@@@@@@@@@@@@@@@             ((_.-'- Cisco Switch Data Center Config -'-._))   #
!        @@@@@@@@@@@@@@@@@@@@@@@                                                            #
!      @@@@@@@@@@@@@@@@@@@@@@@@@@@            - Configuration for Cisco Nexus -             #
!     @@@@@@@@@@@@@@@@@@@@@@@@@@@@@                                                         #
!    @@@@@@@@@@@@@@@/      \@@@/   @      [+] Cyber-Weapon:............. Nexus Configs      #
!   @@@@@@@@@@@@@@@@\  O   @@  @ O @      [+] Version:.................. 2.2                #
!   @@@@@@@@@@@@@ @@@@@@@@@@  | \@@@@@    [+] Author:................... Fz3r0              #
!   @@@@@@@@@@@@@ @@@@@@@@@\__@_/@@@@@    [+] Github:................... github.com/Fz3r0   #
!    @@@@@@@@@@@@@@@/./././ /_|.\ \.\     [+] Twitter:.................. @Fz3r0_OPs         #
!      @@@@@@@@@@@@@|  | | | | | | | |    [+] Youtube:.................. @Fz3r0_OPs         #
!                  \_|_|_|_|_|_|_|_|                                                        #
!                                                                                           #
!############################################################################################
!                                 !!! DISCLAIMER !!!                                        #
!############################################################################################
!                                                                                           #
!        THE AUTHOR ASSUMES NO RESPONSIBILITY OR LIABILITY FOR ANY DAMAGE OR ISSUES         #
!        THAT MAY ARISE FROM THE USE OR MISUSE OF THIS SCRIPT. USE AT YOUR OWN RISK.        #
!                                                                                           #
!############################################################################################

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-1
copy running-config startup-config

!
!


````

## 🥈 `NX9-2-CR-STB` - (Switch NX9-2 - CORE: STAND-BY HSRP (Priority 100))

````py
!##################################################
!#    NEXUS - NX9-2-CORE-STANDBY                  #
!#    IP    - 192.168.30.253/24                   #
!#          - 192.168.30.1 (VIP-HSRP)             #
!#    LAYER 3 + LAYER 2                           #
!#    OSPF -> LAN+P2P @ Area 0 / OSPF 1           #
!#    HSRP = STANDBY @ x.x.x.253 / Priority 100   #
!#              VIP @ x.x.x.1                     #
!#    STP  = SECONDARY / Sec Bridge               #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname NX9-2-CR-STB
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

!# FEATURES

feature interface-vlan
feature ospf
feature hsrp
feature telnet
feature ssh
feature lldp
feature lacp

!# VLANs

vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
vlan 99
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native

!# RPVSTP+ (ROOT-SECONDARY)

spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30 root secondary

!# Enable OSPF 1
router ospf 1
   router-id 2.2.2.2
   log-adjacency-changes
exit

#! SVIs (GATEWAY L3) {OSPF AREA 0}

interface vlan 10
   no shutdown
   description ** SVI+GW-L3-VLAN10-BLUE **
   ip address 192.168.10.253/24
   ip router ospf 1 area 0
exit
interface vlan 20
   no shutdown
   description ** SVI+GW-L3-VLAN20-RED **
   ip address 192.168.20.253/24
   ip router ospf 1 area 0
exit
interface vlan 30
   no shutdown
   description ** SVI+GW-L3-VLAN30-GREEN **
   ip address 192.168.30.253/24
   ip router ospf 1 area 0
exit

!# L3 PORT CHANNELS (no switchport)

default interface ethernet 1/5-6,ethernet 1/3
interface ethernet 1/5-6,ethernet 1/3
   description ** L3-Port-Channel-0-Po0-Interfaces **
   no shutdown
   no switchport
   speed 1000
   duplex full
   channel-group 1 mode active
exit
interface port-channel 1
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/3
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

#! L3 WAN INTERFACES @ INTERNET + OSPF FULL MESH

#! OSPF BEST PREFERENCE (1) @ RT2 >> @ WAN 2 
interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 10.20.0.2/30
   speed 1000
   duplex full
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 1
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable
exit

#! OSPF WORST PREFERENCE (100) @ RT1 (WAN1)
interface ethernet 1/2
   no shutdown
   no switchport
   description ** OSPF-BACKUP-TO-RT-2 **
   ip address 10.30.0.2/30
   speed 1000
   duplex full
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 100
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable
exit

#! OSPF MEDIUM PREFERENCE (10) @ NX9-1 [Port Channel 1]
interface port-channel 1
   no shutdown
   no switchport
   description ** OSPF-BACKUP-NX1-TO-NX2-PORT-CHANNEL **
   ip address 10.50.0.2/30
   speed 1000
   duplex full
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 100
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable

#! Configure HSRP (FOR EACH VLAN) (BOTH SWITCHES = SAME VIP ;)) {STANDBY = PRIORITY 100}

interface vlan 10
   !# Select version 2
   hsrp version 2
   !# Create Group
   hsrp 10
   !# Configure VIP
   ip 192.168.10.1
   !# Highest priprity will be primary
   preempt
   priority 100
exit
interface vlan 20
   !# Select version 2
   hsrp version 2
   !# Create Group
   hsrp 20
   !# Configure VIP
   ip 192.168.20.1
   !# Highest priprity will be primary
   preempt
   priority 100
exit
interface vlan 30
   !# Select version 2
   hsrp version 2
   !# Create Group
   hsrp 30
   !# Configure VIP
   ip 192.168.30.1
   !# Highest priprity will be primary
   preempt
   priority 100
exit

!# L2 INTERFACES - TRUNK (STP = NETWORK)

interface ethernet1/4,ethernet1/7
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable
exit

!# OOB MANAGEMENT INTERFACE - MGMT -> CRADLEPOINT

interface mgmt 0
  no shutdown
  description ** OOB Management Interface **
  ip address 192.168.0.1/24
exit

vrf context management
   ip route 0.0.0.0/0 192.168.0.2
exit

!# TELNET & SSH #

line vty
   session-limit 5
   exec-timeout 3
exit
ip access-list remote-access-users
   permit ip 192.168.0.0/24 any
   permit ip 192.168.30.0/24 any
   permit ip host 192.168.10.101 any
exit  
line vty
   access-class remote-access-users in
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-2 
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.253
+ VIP(HSRP) =  192.168.30.1 

? login     :  admin
? Password  :  Adm1n.C1sc0

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

#
$

!############################################################################################
!                                                                                           #
!          @@@@@@@@@@@@@@@@@@             ((_.-'- Cisco Switch Data Center Config -'-._))   #
!        @@@@@@@@@@@@@@@@@@@@@@@                                                            #
!      @@@@@@@@@@@@@@@@@@@@@@@@@@@            - Configuration for Cisco Nexus -             #
!     @@@@@@@@@@@@@@@@@@@@@@@@@@@@@                                                         #
!    @@@@@@@@@@@@@@@/      \@@@/   @      [+] Cyber-Weapon:............. Nexus Configs      #
!   @@@@@@@@@@@@@@@@\  O   @@  @ O @      [+] Version:.................. 2.2                #
!   @@@@@@@@@@@@@ @@@@@@@@@@  | \@@@@@    [+] Author:................... Fz3r0              #
!   @@@@@@@@@@@@@ @@@@@@@@@\__@_/@@@@@    [+] Github:................... github.com/Fz3r0   #
!    @@@@@@@@@@@@@@@/./././ /_|.\ \.\     [+] Twitter:.................. @Fz3r0_OPs         #
!      @@@@@@@@@@@@@|  | | | | | | | |    [+] Youtube:.................. @Fz3r0_OPs         #
!                  \_|_|_|_|_|_|_|_|                                                        #
!                                                                                           #
!############################################################################################
!                                 !!! DISCLAIMER !!!                                        #
!############################################################################################
!                                                                                           #
!        THE AUTHOR ASSUMES NO RESPONSIBILITY OR LIABILITY FOR ANY DAMAGE OR ISSUES         #
!        THAT MAY ARISE FROM THE USE OR MISUSE OF THIS SCRIPT. USE AT YOUR OWN RISK.        #
!                                                                                           #
!############################################################################################

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-2
copy running-config startup-config

!
!



````

## ↔️ `NX9-11-ACCESS` - (Switch NX9-11 - ACCESS)

````py
!##################################################
!#    NEXUS - NX9-11-ACCESS                       #
!#    IP    - 192.168.30.11 /24                   #
!#    LAYER 2                                     #
!#    STP   = ZOMBIE                              #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname NX9-11-ACCESS
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

!# FEATURES

feature interface-vlan
feature telnet
feature ssh
feature lldp
feature lacp

!# VLANs

vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
vlan 99
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native

!# RPVSTP+ (ZOMBIE)

spanning-tree mode rapid-pvst

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)

interface vlan 30
   no shutdown
   description ** SVI-MGMT-L3-VLAN30-GREEN **
   ip address 192.168.30.11/24
exit

!# ip default-gateway 192.168.30.1 <- This command is deprecated.
ip route 0.0.0.0/0 192.168.30.1

!# L2 PORT CHANNELS (switchport)

default interface ethernet 1/1-3
interface ethernet 1/1-3
   description ** L2-Port-Channel-1-Po1-Interfaces **
   no shutdown
   switchport
   speed 1000
   duplex full
   channel-group 1 mode active
exit
interface port-channel 1
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/3
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

!# L2 INTERFACES - TRUNK

interface port-channel 1
   description ** Port-Channel-1-L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable

interface ethernet1/4
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable
exit

!# L2 INTERFACES - ACCESS

interface ethernet1/5
   no shutdown
   description ** L2-ACCESS-VLAN10-BLUE **
   switchport
   switchport mode access
   switchport access vlan 10
   speed 1000
   duplex full
   spanning-tree port type edge
   spanning-tree bpduguard enable
   cdp enable
exit

interface ethernet1/6
   no shutdown
   description ** L2-ACCESS-VLAN20-RED **
   switchport
   switchport mode access
   switchport access vlan 20
   speed 1000
   duplex full
   spanning-tree port type edge
   spanning-tree bpduguard enable
   cdp enable
exit

interface ethernet1/7
   no shutdown
   description ** L2-ACCESS-VLAN30-GREEN **
   switchport
   switchport mode access
   switchport access vlan 30
   speed 1000
   duplex full
   spanning-tree port type edge
   spanning-tree bpduguard enable
   cdp enable
exit

!# TELNET & SSH #

line vty
   session-limit 5
   exec-timeout 3
exit
ip access-list remote-access-users
   permit ip 192.168.30.0/24 any
   permit ip host 192.168.10.101 any
exit  
line vty
   access-class remote-access-users in
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-11
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.11

? login     :  admin
? Password  :  Adm1n.C1sc0

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

#
$

!############################################################################################
!                                                                                           #
!          @@@@@@@@@@@@@@@@@@             ((_.-'- Cisco Switch Data Center Config -'-._))   #
!        @@@@@@@@@@@@@@@@@@@@@@@                                                            #
!      @@@@@@@@@@@@@@@@@@@@@@@@@@@            - Configuration for Cisco Nexus -             #
!     @@@@@@@@@@@@@@@@@@@@@@@@@@@@@                                                         #
!    @@@@@@@@@@@@@@@/      \@@@/   @      [+] Cyber-Weapon:............. Nexus Configs      #
!   @@@@@@@@@@@@@@@@\  O   @@  @ O @      [+] Version:.................. 2.2                #
!   @@@@@@@@@@@@@ @@@@@@@@@@  | \@@@@@    [+] Author:................... Fz3r0              #
!   @@@@@@@@@@@@@ @@@@@@@@@\__@_/@@@@@    [+] Github:................... github.com/Fz3r0   #
!    @@@@@@@@@@@@@@@/./././ /_|.\ \.\     [+] Twitter:.................. @Fz3r0_OPs         #
!      @@@@@@@@@@@@@|  | | | | | | | |    [+] Youtube:.................. @Fz3r0_OPs         #
!                  \_|_|_|_|_|_|_|_|                                                        #
!                                                                                           #
!############################################################################################
!                                 !!! DISCLAIMER !!!                                        #
!############################################################################################
!                                                                                           #
!        THE AUTHOR ASSUMES NO RESPONSIBILITY OR LIABILITY FOR ANY DAMAGE OR ISSUES         #
!        THAT MAY ARISE FROM THE USE OR MISUSE OF THIS SCRIPT. USE AT YOUR OWN RISK.        #
!                                                                                           #
!############################################################################################

!# SAVE CHECKPOINT & CONFIGURATION
end
checkpoint fz3r0-check-2025-NX9-11
copy running-config startup-config


!
!


````

## ↔️ `NX9-12-ACCESS` - (Switch NX9-12 - ACCESS)

````py
!##################################################
!#    NEXUS - NX9-12-ACCESS                       #
!#    IP    - 192.168.30.12 /24                   #
!#    LAYER 2                                     #
!#    STP   = ZOMBIE                              #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname NX9-12-ACCESS
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

!# FEATURES

feature interface-vlan
feature telnet
feature ssh
feature lldp
feature lacp

!# VLANs

vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
vlan 99
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native

!# RPVSTP+ (ZOMBIE)

spanning-tree mode rapid-pvst

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)

interface vlan 30
   no shutdown
   description ** SVI-MGMT-L3-VLAN30-GREEN **
   ip address 192.168.30.12/24
exit

!# ip default-gateway 192.168.30.1 <- This command is deprecated.
ip route 0.0.0.0/0 192.168.30.1

!# L2 PORT CHANNELS (switchport)

!# Po1

default interface ethernet 1/1-3
interface ethernet 1/1-3
   description ** L2-Port-Channel-1-Po1-Interfaces **
   no shutdown
   switchport
   speed 1000
   duplex full
   channel-group 1 mode active
exit
interface port-channel 1
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/3
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

!# Po2

default interface ethernet 1/4-6
interface ethernet 1/4-6
   description ** L2-Port-Channel-2-Po2-Interfaces **
   no shutdown
   switchport
   speed 1000
   duplex full
   channel-group 2 mode active
exit
interface port-channel 2
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/6
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

!# L2 INTERFACES - TRUNK

interface port-channel1,port-channel2
   no shutdown
   description ** L2-Port-Channel-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable

interface Ethernet1/7
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable
exit

!# TELNET & SSH #

line vty
   session-limit 5
   exec-timeout 3
exit
ip access-list remote-access-users
   permit ip 192.168.30.0/24 any
   permit ip host 192.168.10.101 any
exit  
line vty
   access-class remote-access-users in
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-12 
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.12

? login     :  admin
? Password  :  Adm1n.C1sc0

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

#
$

!############################################################################################
!                                                                                           #
!          @@@@@@@@@@@@@@@@@@             ((_.-'- Cisco Switch Data Center Config -'-._))   #
!        @@@@@@@@@@@@@@@@@@@@@@@                                                            #
!      @@@@@@@@@@@@@@@@@@@@@@@@@@@            - Configuration for Cisco Nexus -             #
!     @@@@@@@@@@@@@@@@@@@@@@@@@@@@@                                                         #
!    @@@@@@@@@@@@@@@/      \@@@/   @      [+] Cyber-Weapon:............. Nexus Configs      #
!   @@@@@@@@@@@@@@@@\  O   @@  @ O @      [+] Version:.................. 2.2                #
!   @@@@@@@@@@@@@ @@@@@@@@@@  | \@@@@@    [+] Author:................... Fz3r0              #
!   @@@@@@@@@@@@@ @@@@@@@@@\__@_/@@@@@    [+] Github:................... github.com/Fz3r0   #
!    @@@@@@@@@@@@@@@/./././ /_|.\ \.\     [+] Twitter:.................. @Fz3r0_OPs         #
!      @@@@@@@@@@@@@|  | | | | | | | |    [+] Youtube:.................. @Fz3r0_OPs         #
!                  \_|_|_|_|_|_|_|_|                                                        #
!                                                                                           #
!############################################################################################
!                                 !!! DISCLAIMER !!!                                        #
!############################################################################################
!                                                                                           #
!        THE AUTHOR ASSUMES NO RESPONSIBILITY OR LIABILITY FOR ANY DAMAGE OR ISSUES         #
!        THAT MAY ARISE FROM THE USE OR MISUSE OF THIS SCRIPT. USE AT YOUR OWN RISK.        #
!                                                                                           #
!############################################################################################

!# SAVE CHECKPOINT & CONFIGURATION
end
checkpoint fz3r0-check-2025-NX9-12
copy running-config startup-config


!
!


````

## ↔️ `NX9-13-ACCESS` - (Switch NX9-13 - ACCESS)

````py
!##################################################
!#    NEXUS - NX9-13-ACCESS                       #
!#    IP    - 192.168.30.13 /24                   #
!#    LAYER 2                                     #
!#    STP   = ZOMBIE                              #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname NX9-13-ACCESS
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

!# FEATURES

feature interface-vlan
feature telnet
feature ssh
feature lldp
feature lacp

!# VLANs

vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
vlan 99
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native

!# RPVSTP+ (ZOMBIE)

spanning-tree mode rapid-pvst

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)

interface vlan 30
   no shutdown
   description ** SVI-MGMT-L3-VLAN30-GREEN **
   ip address 192.168.30.13/24
exit

!# ip default-gateway 192.168.30.1 <- This command is deprecated.
ip route 0.0.0.0/0 192.168.30.1

!# L2 PORT CHANNELS (switchport)

!# Po1

default interface ethernet 1/1-3
interface ethernet 1/1-3
   description ** L2-Port-Channel-1-Po1-Interfaces **
   no shutdown
   switchport
   speed 1000
   duplex full
   channel-group 1 mode active
exit
interface port-channel 1
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/3
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

!# Po2

default interface ethernet 1/4-6
interface ethernet 1/4-6
   description ** L2-Port-Channel-2-Po2-Interfaces **
   no shutdown
   switchport
   speed 1000
   duplex full
   channel-group 2 mode active
exit
interface port-channel 2
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/6
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

!# L2 INTERFACES - TRUNK

interface port-channel1,port-channel2
   no shutdown
   description ** L2-Port-Channel-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable

interface Ethernet1/7
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable
exit

!# TELNET & SSH #

line vty
   session-limit 5
   exec-timeout 3
exit
ip access-list remote-access-users
   permit ip 192.168.30.0/24 any
   permit ip host 192.168.10.101 any
exit  
line vty
   access-class remote-access-users in
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-13
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.13

? login     :  admin
? Password  :  Adm1n.C1sc0

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

#
$

!############################################################################################
!                                                                                           #
!          @@@@@@@@@@@@@@@@@@             ((_.-'- Cisco Switch Data Center Config -'-._))   #
!        @@@@@@@@@@@@@@@@@@@@@@@                                                            #
!      @@@@@@@@@@@@@@@@@@@@@@@@@@@            - Configuration for Cisco Nexus -             #
!     @@@@@@@@@@@@@@@@@@@@@@@@@@@@@                                                         #
!    @@@@@@@@@@@@@@@/      \@@@/   @      [+] Cyber-Weapon:............. Nexus Configs      #
!   @@@@@@@@@@@@@@@@\  O   @@  @ O @      [+] Version:.................. 2.2                #
!   @@@@@@@@@@@@@ @@@@@@@@@@  | \@@@@@    [+] Author:................... Fz3r0              #
!   @@@@@@@@@@@@@ @@@@@@@@@\__@_/@@@@@    [+] Github:................... github.com/Fz3r0   #
!    @@@@@@@@@@@@@@@/./././ /_|.\ \.\     [+] Twitter:.................. @Fz3r0_OPs         #
!      @@@@@@@@@@@@@|  | | | | | | | |    [+] Youtube:.................. @Fz3r0_OPs         #
!                  \_|_|_|_|_|_|_|_|                                                        #
!                                                                                           #
!############################################################################################
!                                 !!! DISCLAIMER !!!                                        #
!############################################################################################
!                                                                                           #
!        THE AUTHOR ASSUMES NO RESPONSIBILITY OR LIABILITY FOR ANY DAMAGE OR ISSUES         #
!        THAT MAY ARISE FROM THE USE OR MISUSE OF THIS SCRIPT. USE AT YOUR OWN RISK.        #
!                                                                                           #
!############################################################################################

!# SAVE CHECKPOINT & CONFIGURATION
end
checkpoint fz3r0-check-2025-NX9-13
copy running-config startup-config


!
!


````

## ↔️ `NX9-14-ACCESS` - (Switch NX9-14 - ACCESS)

````py
!##################################################
!#    NEXUS - NX9-14-ACCESS                       #
!#    IP    - 192.168.30.14 /24                   #
!#    LAYER 2                                     #
!#    STP   = ZOMBIE                              #
!##################################################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname NX9-14-ACCESS
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

!# FEATURES

feature interface-vlan
feature telnet
feature ssh
feature lldp
feature lacp

!# VLANs

vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
vlan 99
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native

!# RPVSTP+ (ZOMBIE)

spanning-tree mode rapid-pvst

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)

interface vlan 30
   no shutdown
   description ** SVI-MGMT-L3-VLAN30-GREEN **
   ip address 192.168.30.14/24
exit

!# ip default-gateway 192.168.30.1 <- This command is deprecated.
ip route 0.0.0.0/0 192.168.30.1

!# L2 PORT CHANNELS (switchport)

default interface ethernet 1/1-3
interface ethernet 1/1-3
   description ** L2-Port-Channel-1-Po1-Interfaces **
   no shutdown
   switchport
   speed 1000
   duplex full
   channel-group 1 mode active
exit
interface port-channel 1
   lacp max-bundle 2
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup/standby
#! 1/3 = STANDBY (Higher than Default)
interface ethernet 1/3
   lacp port-priority 39000
exit
!# load balance: src-dst mac is the default
port-channel load-balance src-dst mac

!# L2 INTERFACES - TRUNK

interface port-channel 1
   description ** Port-Channel-1-L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable

interface ethernet1/4
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30
   speed 1000
   duplex full
   spanning-tree port type network
   cdp enable
exit

!# L2 INTERFACES - ACCESS

interface ethernet1/5
   no shutdown
   description ** L2-ACCESS-VLAN10-BLUE **
   switchport
   switchport mode access
   switchport access vlan 10
   speed 1000
   duplex full
   spanning-tree port type edge
   spanning-tree bpduguard enable
   cdp enable
exit

interface ethernet1/6
   no shutdown
   description ** L2-ACCESS-VLAN20-RED **
   switchport
   switchport mode access
   switchport access vlan 20
   speed 1000
   duplex full
   spanning-tree port type edge
   spanning-tree bpduguard enable
   cdp enable
exit

interface ethernet1/7
   no shutdown
   description ** L2-ACCESS-VLAN30-GREEN **
   switchport
   switchport mode access
   switchport access vlan 30
   speed 1000
   duplex full
   spanning-tree port type edge
   spanning-tree bpduguard enable
   cdp enable
exit

!# TELNET & SSH #

line vty
   session-limit 5
   exec-timeout 3
exit
ip access-list remote-access-users
   permit ip 192.168.30.0/24 any
   permit ip host 192.168.10.101 any
exit  
line vty
   access-class remote-access-users in
exit

!# MOTD & CREDITS

banner motd $

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

                         -- Fz3r0 : Nexus Datacenter --

+ DEVICE    =  NX9-14
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.14

? login     :  admin
? Password  :  Adm1n.C1sc0

* Github : Fz3r0           
* Twitter: @fz3r0_OPs 
* Youtube: @fz3r0_OPs

#
$

!############################################################################################
!                                                                                           #
!          @@@@@@@@@@@@@@@@@@             ((_.-'- Cisco Switch Data Center Config -'-._))   #
!        @@@@@@@@@@@@@@@@@@@@@@@                                                            #
!      @@@@@@@@@@@@@@@@@@@@@@@@@@@            - Configuration for Cisco Nexus -             #
!     @@@@@@@@@@@@@@@@@@@@@@@@@@@@@                                                         #
!    @@@@@@@@@@@@@@@/      \@@@/   @      [+] Cyber-Weapon:............. Nexus Configs      #
!   @@@@@@@@@@@@@@@@\  O   @@  @ O @      [+] Version:.................. 2.2                #
!   @@@@@@@@@@@@@ @@@@@@@@@@  | \@@@@@    [+] Author:................... Fz3r0              #
!   @@@@@@@@@@@@@ @@@@@@@@@\__@_/@@@@@    [+] Github:................... github.com/Fz3r0   #
!    @@@@@@@@@@@@@@@/./././ /_|.\ \.\     [+] Twitter:.................. @Fz3r0_OPs         #
!      @@@@@@@@@@@@@|  | | | | | | | |    [+] Youtube:.................. @Fz3r0_OPs         #
!                  \_|_|_|_|_|_|_|_|                                                        #
!                                                                                           #
!############################################################################################
!                                 !!! DISCLAIMER !!!                                        #
!############################################################################################
!                                                                                           #
!        THE AUTHOR ASSUMES NO RESPONSIBILITY OR LIABILITY FOR ANY DAMAGE OR ISSUES         #
!        THAT MAY ARISE FROM THE USE OR MISUSE OF THIS SCRIPT. USE AT YOUR OWN RISK.        #
!                                                                                           #
!############################################################################################

!# SAVE CHECKPOINT & CONFIGURATION
end
checkpoint fz3r0-check-2025-NX9-14
copy running-config startup-config


!
!


````

## 🔀 `RT-1-EDGE` - (Router 1 Edge) 

````py
!# Namings

enable
configure terminal
hostname RT-1-EDGE

!# Interfaces

#! L3 WAN INTERFACES @ CORE/LAN + OSPF FULL MESH

#! OSPF BEST PREFERENCE (1) @ NX9-1-CORE-PRIMARY
interface Ethernet0/1
   no shutdown
   duplex full
   description ** Link-to-NX9-1-CORE **
   ip address 10.10.0.1 255.255.255.252
   ip ospf network point-to-point
   ip ospf cost 1
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   ip nat inside
exit

#! OSPF MEDIUM PREFERENCE (100) @ NX9-2-CORE-SECONDARY
interface Ethernet0/2
   no shutdown
   duplex full
   description ** Link-to-NX9-2-CORE **
   ip address 10.30.0.1 255.255.255.252
   ip ospf network point-to-point
   ip ospf cost 100
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   ip nat inside
exit

#! OSPF WORST PREFERENCE (200) @ RT2 (WAN2)
interface Ethernet0/3
   no shutdown
   duplex full
   description ** RT1-RT2-Edge-to-Edge-Link **
   ip address 10.60.0.1 255.255.255.252
   ip ospf network point-to-point
   ip ospf cost 200
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   ip nat inside
exit

!# WAN INTERFACE (NAT OUTSIDE) [Default Route @ Internet]

interface Ethernet0/0
   no shutdown
   duplex full
   description ** Link-to-WAN-1_INTERNET **
   ip address 123.1.1.2 255.255.255.252
   ip nat outside
exit

!# Ruta por defecto hacia la nube Google
ip route 0.0.0.0 0.0.0.0 123.1.1.1

!# NAT Inside/Outside ACL 10

access-list 10 permit 10.20.0.0 0.0.0.3
access-list 10 permit 10.40.0.0 0.0.0.3
access-list 10 permit 10.50.0.0 0.0.0.3

access-list 10 permit 10.10.0.0 0.0.0.3
access-list 10 permit 10.30.0.0 0.0.0.3
access-list 10 permit 10.60.0.0 0.0.0.3

access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 permit 192.168.20.0 0.0.0.255
access-list 10 permit 192.168.30.0 0.0.0.255

!# Overload all matching inside traffic to the WAN interface address
ip nat inside source list 10 interface Ethernet0/0 overload

!# OSPF Area 0 : Anuncia las redes LAN y P2P
router ospf 1

    !# Router ID (OSPF)
    router-id 3.3.3.3

    !# enlace hacia NX9-1
    network 10.10.0.0 0.0.0.3 area 0
    !# enlace hacia NX9-2
    network 10.30.0.0 0.0.0.3 area 0
    !# enlace Edge-to-Edge
    network 10.60.0.0 0.0.0.3 area 0

    !# VLAN10       
    network 192.168.10.0 0.0.0.255 area 0
    !# VLAN20  
    network 192.168.20.0 0.0.0.255 area 0
    !# VLAN30
    network 192.168.30.0 0.0.0.255 area 0

    !# RT1 - Outside WAN
    network 123.1.1.0 0.0.0.3 area 0

    !# RT2 - to MPLS (No NAT) (Network: 10.100.0.0)
    network 10.100.0.0 0.0.0.3 area 0

    !# propaga la ruta por defecto      
    default-information originate always

    !# redistribuye las rutas estáticas (MPLS)
    redistribute static subnets
exit

!# MPLS INTERFACE (Static Private Address) [Static Route @ MPLS]

!# MPLS INTERFACE 
interface Ethernet1/0
   no shutdown
   duplex full
   description ** Link-to-MPLS-1 **
   ip address 10.100.0.2 255.255.255.252
exit

!# MPLS STATIC ROUTES @ MPLS CIRCUITS
ip route 10.200.0.100 255.255.255.255 10.100.0.1
ip route 10.210.0.100 255.255.255.255 10.100.0.1

end
write memory

!
!


````

## 🔀 `RT-2-EDGE` - (Router 2 Edge) 

````py
!# Namings

enable
configure terminal
hostname RT-2-EDGE

!# Interfaces

#! L3 WAN INTERFACES @ CORE/LAN + OSPF FULL MESH

#! OSPF BEST PREFERENCE (1) @ NX9-2-CORE-SECONDARY
interface Ethernet0/1
   no shutdown
   duplex full
   description ** Link-to-NX9-2-CORE **
   ip address 10.20.0.1 255.255.255.252
   ip ospf network point-to-point
   ip ospf cost 1
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   ip nat inside
exit

#! OSPF MEDIUM PREFERENCE (100) @ NX9-1-CORE-PRIMARY
interface Ethernet0/2
   no shutdown
   duplex full
   description ** Link-to-NX9-1-CORE **
   ip address 10.40.0.2 255.255.255.252
   ip ospf network point-to-point
   ip ospf cost 100
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   ip nat inside
exit

#! OSPF WORST PREFERENCE (200) @ RT1 (WAN1)
interface Ethernet0/3
   no shutdown
   duplex full
   description ** RT1-RT2-Edge-to-Edge-Link **
   ip address 10.60.0.2 255.255.255.252
   ip ospf network point-to-point
   ip ospf cost 200
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   ip nat inside
exit

!# WAN INTERFACE (NAT OUTSIDE) [Default Route @ Internet]

interface Ethernet0/0
   no shutdown
   duplex full
   description ** Link-to-WAN-2_INTERNET **
   ip address 123.2.2.2 255.255.255.252
   ip nat outside
exit

!# Ruta por defecto hacia la nube Google
ip route 0.0.0.0 0.0.0.0 123.2.2.1

!# NAT Inside/Outside ACL 10
access-list 10 permit 10.10.0.0 0.0.0.3
access-list 10 permit 10.30.0.0 0.0.0.3
access-list 10 permit 10.50.0.0 0.0.0.3

access-list 10 permit 10.20.0.0 0.0.0.3
access-list 10 permit 10.40.0.0 0.0.0.3
access-list 10 permit 10.60.0.0 0.0.0.3
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 permit 192.168.20.0 0.0.0.255
access-list 10 permit 192.168.30.0 0.0.0.255

!# Overload all matching inside traffic to the WAN interface address
ip nat inside source list 10 interface Ethernet0/0 overload

!# OSPF Area 0 : Anuncia las redes LAN y P2P
router ospf 1

    !# Router ID (OSPF)
    router-id 4.4.4.4

    !# enlace hacia NX9-2
    network 10.20.0.0 0.0.0.3 area 0
    !# enlace hacia NX9-1
    network 10.40.0.0 0.0.0.3 area 0
    !# enlace Edge-to-Edge
    network 10.60.0.0 0.0.0.3 area 0

    !# VLAN10       
    network 192.168.10.0 0.0.0.255 area 0
    !# VLAN20  
    network 192.168.20.0 0.0.0.255 area 0
    !# VLAN30
    network 192.168.30.0 0.0.0.255 area 0

    !# RT2 - Outside WAN
    network 123.2.2.0 0.0.0.3 area 0

    !# RT2 - to MPLS (No NAT) (Network: 10.100.0.4)
    network 10.100.0.4 0.0.0.3 area 0

    !# propaga la ruta por defecto      
    default-information originate always

    !# redistribuye las rutas estáticas (MPLS)
    redistribute static subnets
       
exit

!# MPLS INTERFACE (Static Private Address) [Static Route @ MPLS]

!# MPLS INTERFACE 
interface Ethernet1/0
   no shutdown
   description ** Link-to-MPLS-2 **
   ip address 10.100.0.6 255.255.255.252
   duplex full
exit

!# MPLS STATIC ROUTES @ MPLS CIRCUITS
ip route 10.200.0.100 255.255.255.255 10.100.0.5
ip route 10.210.0.100 255.255.255.255 10.100.0.5

end
write memory

!
!


````

## 🌎 `WAN-1` - (ACTIVE Internet Circuit)

````py
!###############
!# WAN-1 (IOS) #
!###############

!# Namings
enable
configure terminal
hostname WAN-1

!# WAN Interface
interface Ethernet0/0
  no shutdown
  duplex full  
  description LINK-TO-NETWORK
  ip address 123.1.1.1 255.255.255.252

exit

!# Loopbacks (Google)
interface Loopback0
  description ** GOOGLE-DNS-1 **
  ip address 8.8.8.8 255.255.255.255
exit
interface Loopback1
  description ** GOOGLE-DNS-2 **
  ip address 8.8.4.4 255.255.255.255
exit

!# Default Route (opcional si usas default-route en el otro router)
ip route 0.0.0.0 0.0.0.0 123.1.1.2

end
write memory

!
!


````


## 🌎 `WAN-2` - (STAND-BY Internet Circuit)



````py
!###############
!# WAN-2 (IOS) #
!###############

!# Namings
enable
configure terminal
hostname WAN-2

!# WAN Interface
interface Ethernet0/0
  no shutdown
  duplex full
  description LINK-TO-NETWORK
  ip address 123.2.2.1 255.255.255.252
exit

!# Loopbacks (Google)
interface Loopback0
  description ** GOOGLE-DNS-1 **
  ip address 8.8.8.8 255.255.255.255
exit
interface Loopback1
  description ** GOOGLE-DNS-2 **
  ip address 8.8.4.4 255.255.255.255
exit

!# Default Route (opcional si usas default-route en el otro router)
ip route 0.0.0.0 0.0.0.0 123.2.2.2

end
write memory

!
!


````

## 🕸️ `MPLS-1` - (ACTIVE MPLS Circuit)


````py
!################
!# MPLS-1 (IOS) #
!################

!# Namings
enable
configure terminal
hostname MPLS-1

interface Ethernet0/0
   no shutdown
   description ** Link-MPLS-1 (RT-1) **
   ip address 10.100.0.1 255.255.255.252
   duplex full

!
interface Loopback0
 description ** Simulated Remote Net A **
 ip address 10.200.0.100 255.255.255.255
!
interface Loopback1
 description ** Simulated Remote Net B **
 ip address 10.210.0.100 255.255.255.255
!

# LANS & Backbone
ip route 192.168.10.0 255.255.255.0 10.100.0.2
ip route 192.168.20.0 255.255.255.0 10.100.0.2
ip route 192.168.30.0 255.255.255.0 10.100.0.2
ip route 10.0.0.0 255.0.0.0 10.100.0.2


end
write memory

!
!


````

## 🕸️ `MPLS-2` - (STAND-BY MPLS Circuit)

````py
!################
!# MPLS-2 (IOS) #
!################

!# Namings
enable
configure terminal
hostname MPLS-2

interface Ethernet0/0
   no shutdown
   description ** Link-MPLS-1 (RT-1) **
   ip address 10.100.0.5 255.255.255.252
   duplex full

!
interface Loopback0
 description ** Simulated Remote Net A **
 ip address 10.200.0.100 255.255.255.255
!
interface Loopback1
 description ** Simulated Remote Net B **
 ip address 10.210.0.100 255.255.255.255
!

# LANS & Backbone
ip route 192.168.10.0 255.255.255.0 10.100.0.6
ip route 192.168.20.0 255.255.255.0 10.100.0.6
ip route 192.168.30.0 255.255.255.0 10.100.0.6
ip route 10.0.0.0 255.0.0.0 10.100.0.6


end
write memory

!
!


````

## 🖥️ `SERVER-1-V10BLUE` - (Server 1)

````py
! ############
! # SERVER-1 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-1-V10BLUE

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 192.168.10.101 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 192.168.10.1

! # Exit & Save
end
write memory


!
!


````

## 🖥️ `SERVER-2-V20RED` - (Server 2)


````py
! ############
! # SERVER-2 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-2-V20RED

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 192.168.20.101 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 192.168.20.1

! # Exit & Save
end
write memory


!
!


````

## 🖥️ `SERVER-3-V30GREEN` - (Server 3)


````py
! ############
! # SERVER-3 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-3-V30GREEN

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 192.168.30.101 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 192.168.30.1

! # Exit & Save
end
write memory


!
!


````

## 🖥️ `SERVER-4-V10BLUE` - (Server 4)


````py
! ############
! # SERVER-4 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-4-V10BLUE

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 192.168.10.102 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 192.168.10.1

! # Exit & Save
end
write memory


!
!


````

## 🖥️ `SERVER-5-V20RED` - (Server 5)


````py
! ############
! # SERVER-5 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-5-V20RED

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 192.168.20.102 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 192.168.20.1

! # Exit & Save
end
write memory


!
!


````

## 🖥️ `SERVER-6-V30GREEN` - (Server 6)

````py
! ############
! # SERVER-6 #
! ############

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname SERVER-6-V30GREEN

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated PC Interface **
   ip address 192.168.30.102 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 192.168.30.1

! # Exit & Save
end
write memory


!
!


````

## 📡 `CP-OOB-1` - (Cradlepoint Out-Of-Band Management 1)

````py
! #####################
! # CRADLEPOINT OOB 1 #
! #####################

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname CP-OOB-1

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated Cradlepoint OOB Port **
   ip address 192.168.0.2 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (Management Nexus Interface (MGMT))
ip route 0.0.0.0 0.0.0.0 192.168.0.1

! # Exit & Save
end
write memory


!
!


````

## 📡 `CP-OOB-2` - (Cradlepoint Out-Of-Band Management 2)

````py
! #####################
! # CRADLEPOINT OOB 2 #
! #####################

! # Enable & Config Exec Line
enable
configure terminal

! # Change the device hostname
hostname CP-OOB-2

! # Disable DNS lookup to avoid delays when typing invalid commands
no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** Simulated Cradlepoint OOB Port **
   ip address 192.168.0.2 255.255.255.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (Management Nexus Interface (MGMT))
ip route 0.0.0.0 0.0.0.0 192.168.0.1

! # Exit & Save
end
write memory


!
!


````

# 🎥 Resources

- https://www.youtube.com/watch?v=lADK3STwwAM&list=PLwAU7bA502wFB5j6RnpDPNG5xwb5JEbq8
- https://www.youtube.com/watch?v=fdc912ReAE4
- https://youtu.be/oBJNkFhPpfU?si=BpyN82rV99dBf_cI
- https://youtu.be/aPzNzvyv20A?si=1QMckKT0AjZHR1bm
- https://youtu.be/ieZkA7Ayc-4?si=XgsEx2Pz87hLBZM5
- https://youtu.be/IFb-Ncj5w-E?si=BsXcc9WlyG-W0vpu
- https://journey2theccie.wordpress.com/2020/07/20/hsrp-aware-dhcp-relay/

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
