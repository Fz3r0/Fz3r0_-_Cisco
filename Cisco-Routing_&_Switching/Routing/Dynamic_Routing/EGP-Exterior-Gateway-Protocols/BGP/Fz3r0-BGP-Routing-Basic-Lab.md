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

| **Device**       | **Interface** | **IP Address**       | **CIDR**  | **Subnet Mask**       | **Description**                   |
| ------------ | --------- | ---------------- | ----- | ----------------- | ----------------------------- |
| 🌐 **R1**    | Eth0      | `1.0.0.1`        | `/30` | `255.255.255.252` | WAN link to R2                |
|              | Eth1.10   | `192.168.10.1`   | `/24` | `255.255.255.0`   | Gateway for VLAN 10 in Site A |
|              | Eth1.20   | `192.168.20.1`   | `/24` | `255.255.255.0`   | Gateway for VLAN 20 in Site A |
|              | Eth1.30   | `192.168.30.1`   | `/24` | `255.255.255.0`   | Gateway for VLAN 30 in Site A |
| 📡 **R2**    | Eth0      | `1.0.0.2`        | `/30` | `255.255.255.252` | Link to R1                    |
|              | Eth1      | `2.0.0.1`        | `/30` | `255.255.255.252` | Link to R3                    |
| 📡 **R3**    | Eth1      | `2.0.0.2`        | `/30` | `255.255.255.252` | Link to R2                    |
|              | Eth0      | `3.0.0.1`        | `/30` | `255.255.255.252` | Link to R4                    |
| 🌐 **R4**    | Eth0      | `3.0.0.2`        | `/30` | `255.255.255.252` | WAN link to R3                |
|              | Eth1.10   | `192.168.10.254` | `/24` | `255.255.255.0`   | Gateway for VLAN 10 in Site B |
|              | Eth1.20   | `192.168.20.254` | `/24` | `255.255.255.0`   | Gateway for VLAN 20 in Site B |
|              | Eth1.30   | `192.168.30.254` | `/24` | `255.255.255.0`   | Gateway for VLAN 30 in Site B |
| 💻 **PC-A1** | eth0      | `192.168.10.100` | `/24` | `255.255.255.0`   | Site A - VLAN 10 PC #1        |
| 💻 **PC-A2** | eth0      | `192.168.10.101` | `/24` | `255.255.255.0`   | Site A - VLAN 10 PC #2        |
| 💻 **PC-A3** | eth0      | `192.168.20.100` | `/24` | `255.255.255.0`   | Site A - VLAN 20 PC #1        |
| 💻 **PC-A4** | eth0      | `192.168.20.101` | `/24` | `255.255.255.0`   | Site A - VLAN 20 PC #2        |
| 💻 **PC-A5** | eth0      | `192.168.30.100` | `/24` | `255.255.255.0`   | Site A - VLAN 30 PC #1        |
| 💻 **PC-A6** | eth0      | `192.168.30.101` | `/24` | `255.255.255.0`   | Site A - VLAN 30 PC #2        |
| 💻 **PC-B1** | eth0      | `192.168.10.100` | `/24` | `255.255.255.0`   | Site B - VLAN 10 PC #1        |
| 💻 **PC-B2** | eth0      | `192.168.10.101` | `/24` | `255.255.255.0`   | Site B - VLAN 10 PC #2        |
| 💻 **PC-B3** | eth0      | `192.168.20.100` | `/24` | `255.255.255.0`   | Site B - VLAN 20 PC #1        |
| 💻 **PC-B4** | eth0      | `192.168.20.101` | `/24` | `255.255.255.0`   | Site B - VLAN 20 PC #2        |
| 💻 **PC-B5** | eth0      | `192.168.30.100` | `/24` | `255.255.255.0`   | Site B - VLAN 30 PC #1        |
| 💻 **PC-B6** | eth0      | `192.168.30.101` | `/24` | `255.255.255.0`   | Site B - VLAN 30 PC #2        |


## Configuration








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





