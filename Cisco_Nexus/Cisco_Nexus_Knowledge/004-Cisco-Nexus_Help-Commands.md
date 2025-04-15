# 🧠🏗️🌐 Cisco Nexus: `Help Commands`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Datacenter` `NX-OS` `Cisco Nexus` `Leaf & Spine` `DC Switching` `Modular Chassis` `Fixed Form Factor` `vPC` `ECMP` `FEX` `Fabric Extender` `Nexus 2000` `Nexus 5000` `Nexus 7000` `Nexus 9000`  `Port-Channel` `LAG` `VLAN` `VRF` `Overlay` `Underlay` `EVPN` `VXLAN`  `Spine Switch` `Leaf Switch` `Control Plane` `Forwarding Plane` `Supervisor Card` `Line Card`  `Redundant Power Supply` `Hot-swappable` `In-Service Software Upgrade (ISSU)` `High Availability`  `Twinax Cable` `SFP+` `QSFP28` `QSFP-DD` `10G` `25G` `40G` `100G` `400G`

---

<br>

# 📝❓📄 `Index`



# ⚡ Cisco Nexus - Help Commands




## Topology

We will be using this simple topology just for example:

![image](https://github.com/user-attachments/assets/01e77d1b-e113-405c-abfd-1612f39379c5)

## Help Commands: 

Notas:

- No existen los comandos enable/disable en nexus 9k
- Todas las interfaces por default vienen como "routed" en nuxus 9k, es decir, no son capa 2
- Todas las interfaces vienen como "shut down" (apagadas)

````py

!# NAMINGS
!###########

!# Change Switch Hostname
hostname NXv9K-1

!# BASICS
!###########

!#
show running

!#
show interface status

!# FEATURES
!###########

!# Show all apliable features
feature ?

!# Add interface VLAN feature (v7 needs to turn on this first, 9k doesn't)
feature interface-vlan

!# Add demo licence to test all features
licence grace-period


!# IP ADDRESSING
!###########

interface ethernet 1/1
no shutdown
ip address 10.1.1.1/30
exit

!# DISCOVERY PROTOCOLS
!###########
cdp run
lldp run


````

## Basic Example tu use:

````py
!# BASIC EXAMPLE
!###########

!# NEXUS 7K

hostname NXv7K-1
licence grace-period

feature interface-vlan

interface ethernet 2/1
no shutdown
ip address 10.1.1.1/30
exit

cdp run
lldp run

!# NEXUS 9K

hostname NXv9K-1
licence grace-period

interface ethernet 1/1
no shutdown
ip address 10.1.1.2/30
exit

cdp enable
lldp enable

interface ethernet 1/1
cdp enable
lldp enable
exit

````


## Management interface configuration

- El puerto management pertenece a la VRF management por defecto
- Permite la administracion out of band
- Debe tener una IP asignada
- Debe existir una Default ROute dentro del conectexto de la VRF management








# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=fdc912ReAE4



  
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

