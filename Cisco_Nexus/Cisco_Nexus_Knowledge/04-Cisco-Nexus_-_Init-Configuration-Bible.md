# 🧠🏗️🌐 Cisco Nexus: `Fz3r0 :: Nexus Init Configuration Bible`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Datacenter` `NX-OS` `Cisco Nexus` `Leaf & Spine` `DC Switching` `Modular Chassis` `Fixed Form Factor` `vPC` `ECMP` `FEX` `Fabric Extender` `Nexus 2000` `Nexus 5000` `Nexus 7000` `Nexus 9000`  `Port-Channel` `LAG` `VLAN` `VRF` `Overlay` `Underlay` `EVPN` `VXLAN`  `Spine Switch` `Leaf Switch` `Control Plane` `Forwarding Plane` `Supervisor Card` `Line Card`  `Redundant Power Supply` `Hot-swappable` `In-Service Software Upgrade (ISSU)` `High Availability`  `Twinax Cable` `SFP+` `QSFP28` `QSFP-DD` `10G` `25G` `40G` `100G` `400G`

---

<br>

# 📝❓📄 `Index`



# ⚡ Cisco Nexus - Fz3r0 :: Nexus Init Confioguration Bible




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

Rollback & Checkpoint Notes

- A diferencia de IOS, en NX se puede crear un punto de retorno o checkpoint para poder hacer roll-backs más fácil.
- Ejemplo, si yo hago un checkpoint, después hago 1203990123 páginas de configuración y algo salió mal, solo hago un roll-back y automáticamente regresaré el checkpoint. 

## Basic Configurations & Commands: 

````py

!###########
!# NAMINGS #
!###########

!# Change Switch Hostname
hostname NXv9K-1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##########
!# BASIC SHOW #
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

! # Enable CDP in exec line 
cdp enable

! # Enable CDP in selected interface
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
!# LICENSES
!###########

!# Enable Free test license for 120 days (enable features)
license grace-period

!# Check all licenses usage
show license usage

!# Check "X" license usage
show license usage LAN_ENTERPRISE_SERVICES_PKG

!# Check license status
show license status

!#---

!# Install paid licenses (need Internet connection):

    !# 1. Install smart license
feature license smart
    !# 2. Enable smart license
license smart enable
    !# 4. Enable DNS resolving
ip domain-lookup 
    !# 5. Add DNS to use (eg. Google DNS)
ip name-server 8.8.8.8
    !# 6. Check or Add a Default Route
ip route 0.0.0.0/0 123.123.123.1
    !# 7. Test ping to Cisco Tools
ping tools.cisco.com
    !# 8. Start callhome process (it calls Cisco's Mothership)
callhome
  transport http use-vrf default
  exit
exit
    !# 9. Register Cisco ID Token
license smart register idtoken e128fa0bcc32ffaccd91ff0001
    !# 10. Done! you can also check the license in the Smart Software Licensing Dashboard URL provided by Cisco 


!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##########################################
!# SET DEFAULT SETTINGS
!##########################################

!# Set default settings for "X" interface
default interface ethernet 1/1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##########################################
!# CHECKPOINTS & ROLL-BACKS
!##########################################

!# Creates a checkpoint with a custom name
checkpoint fz3r0-check-04212025

!# Roll-back to a checkpoint
rollback running config checkpoint fz3r0-check-04212025

!# See the saved checkpoints
show checkpoint 

!# See the saved checkpoints list by name
show checkpoint | include name

!# See the details of saved checkpoints
show checkpoint summary

!# See the details of selected checkpoint configurations
show checkpoint fz3r0-check-04212025






!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

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








# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=lADK3STwwAM&list=PLwAU7bA502wFB5j6RnpDPNG5xwb5JEbq8
- https://www.youtube.com/watch?v=fdc912ReAE4
- https://youtu.be/oBJNkFhPpfU?si=BpyN82rV99dBf_cI



  
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

