# 🧠🏗️🌐 Cisco Nexus: `Basic Config, Help Commands, + Backup & Checkpoints`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Datacenter` `NX-OS` `Cisco Nexus` `Leaf & Spine` `DC Switching` `Modular Chassis` `Fixed Form Factor` `vPC` `ECMP` `FEX` `Fabric Extender` `Nexus 2000` `Nexus 5000` `Nexus 7000` `Nexus 9000`  `Port-Channel` `LAG` `VLAN` `VRF` `Overlay` `Underlay` `EVPN` `VXLAN`  `Spine Switch` `Leaf Switch` `Control Plane` `Forwarding Plane` `Supervisor Card` `Line Card`  `Redundant Power Supply` `Hot-swappable` `In-Service Software Upgrade (ISSU)` `High Availability`  `Twinax Cable` `SFP+` `QSFP28` `QSFP-DD` `10G` `25G` `40G` `100G` `400G`

---

<br>

# 📝❓📄 `Index`



# ⚡ Cisco Nexus - Basic Config, Help Commands, + Backup & Checkpoints




## Topology

We will be using this simple topology just for example:

![image](https://github.com/user-attachments/assets/01e77d1b-e113-405c-abfd-1612f39379c5)

## Notes:

Basic Notes:

- No existen los comandos enable/disable en nexus 9k
- El comando "show" se puede ejecutar donde sea (a diferenciade IOS que solo se puede en "enable")
- Todas las interfaces por default vienen como "routed" en nuxus 9k, es decir, no son capa 2.
- Todas las interfaces por default vienen como "disabled/shut down" (apagadas)

Management Interface Notes: 

- El puerto management va conectado a un router out-of-band, por ejemplo un cradlepoint con un chip 5G LTE conectado completamente fuera de la red que comunmente se usa como MPLS, Internet, etc. 
- El puerto management pertenece a la VRF management por defecto
- Permite la administracion out of band
- Debe tener una IP asignada
- Debe existir una Default ROute dentro del conectexto de la VRF management

## Basic Commands: 

````py

!###########
!# NAMINGS #
!###########

!# Change Switch Hostname
hostname NXv9K-1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##########
!# BASICS #
!##########

!#
show running

!#
show interface status

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!#################
!# IP ADDRESSING #
!#################

interface ethernet 1/1
no shutdown
ip address 10.1.1.1/30
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##########################################
!# Out-Of-Band (OOB) Management Interface #
!##########################################
!
! ## IP Addressing:
interface management 0
ip address 192.168.100.100/24
no shutdown
!
! ## Enter the management VRF context (used for OOB traffic separation):
vrf context management
! ## Add a default route (0.0.0.0/0) in the management VRF pointing to the OOB router/gateway (eg. Cradlepoint)
ip route 0.0.0.0/0 192.168.100.1
!
! # Show all VRFs configured on the device
show vrf
! # Show interfaces assigned to the 'management' VRF
show vrf management interface
! # Show running configuration specific to the 'management' VRF
show running-config vrf management

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!#######################
!# DISCOVERY PROTOCOLS #
!#######################

cdp enable
interface ethernet 1/1
cdp enable
end

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!############
!# FEATURES #
!############

!# Show all apliable features (Port Channel, DHCP, BGP, BFD, LACP, Inter-VLAN, etc)
feature ?

!# Add interface VLAN feature (v7 needs to turn on this first, 9k doesn't)
feature interface-vlan

!# Add demo licence to test all features
licence grace-period

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########
!# LICENCES
!###########

!# Enable Free test licence for 120 days
license grace-period

````

## Help Commands

````py

!###########
!# WHERE
!###########

!# Muestra donde estoy ubicado en la CLI, osea dónde estoy configurando
where

!# Detalle del comando "where" (más información)
where detail

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########
!# VIEW CONFIGS
!###########

!# Ver la diferencia del running-config contra el startup-config
show diff rollback-patch running-config startup-config

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########
!# PUSH / POP
!###########

!# Crea un Check-point o CUE en la CLI para poder regresar posteriormente con un "pop"
push

!# Regresa al Check-point de CLI creado anteriormente con un "push"
pop

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########
!# DIRs
!###########

!# Muestra lo que hay en el fash de booteo
dir bootflash:

!# Muestra lo que hay en un archivo especifico
dir backup-1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########
!# SHOW, PWD
!###########

!# MOVEMENT (like linux)
pwd
mkdir tech-support
cd tech-support
cd ..

!# Muestra lo que hay en el archivo "X"
show backup-1

!# Genera el archivo tech-support para el TAC de cisco (!!!puede tardar hasta 10 minutos)
show tech-support

!# Genera el archivo tech-support para el TAC de cisco (mejor práctica)
show tech-support > tech-support-10.10.2020


!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########
!# MOVES & FLASH
!###########

!# ">" Move a file (similar to linux)

   - !# Create .txt running config on flash
show running-config > backup-1




````




## Backups & Checkpoints







## Basic Example tu use:

````py
!# BASIC EXAMPLE
!#################################

!######################
!# NEXUS 7K
!######################

configure terminal
hostname NXv7K-1
username admin password Admin12345
license grace-period

feature interface-vlan

interface ethernet 2/1
no shutdown
ip address 10.1.1.1/30
cdp enable
exit

cdp enable
end

copy running-config startup-config

!=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!######################
!# NEXUS 9K
!######################

configure terminal
hostname NXv9K-1
username admin password Admin12345
license smart 

interface ethernet 1/1
no shutdown
ip address 10.1.1.2/30
cdp enable
exit

cdp enable
end

copy running-config startup-config





````


## Management interface configuration




````py

!## Configure the out-of-band management interface
!###################################################
!
! ## IP Addressing:
interface management 0
ip address 192.168.100.100/24
no shutdown
!
! ## Enter the management VRF context (used for OOB traffic separation):
vrf context management
! ## Add a default route (0.0.0.0/0) in the management VRF pointing to the OOB router/gateway (eg. Cradlepoint)
ip route 0.0.0.0/0 192.168.100.1
!
! # Show all VRFs configured on the device
show vrf
! # Show interfaces assigned to the 'management' VRF
show vrf management interface
! # Show running configuration specific to the 'management' VRF
show running-config vrf management





````


## Nexus Help Commands: 




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

