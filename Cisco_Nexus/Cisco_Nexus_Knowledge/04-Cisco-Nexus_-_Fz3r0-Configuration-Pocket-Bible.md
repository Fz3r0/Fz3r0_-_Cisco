# 🧠🏗️🌐 Cisco Nexus: `Fz3r0 :: Nexus Configuration Bible`

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

![image](https://github.com/user-attachments/assets/de337694-197d-4271-832f-06537bcd29e8)


## Cisco Nexus & NX-OS Notes:

v7 VS v9 images:

- 7k Image doesn't support port-channel or LACP, unfortunately.
- The only problem I have with the 9k Image is that it does not support multiple VDCs.

Basic Notes:

- No existen los comandos enable en nexus 9k (NX-OS) para cambiar ese nivel en CLI.
- El comando "show" se puede ejecutar donde sea (a diferenciade IOS que solo se puede en el primer nivel de CLI o teniendo que usar "do")
- Todas las interfaces por default vienen como "routed" layer 3 en nuxus 9k, es decir, no son el clásico capa 2 como en IOS/Catalyst.
- Todas las interfaces por default vienen como "disabled/shut down" (apagadas)

Management Interface Notes: 

- El puerto management va conectado a un router out-of-band, por ejemplo un cradlepoint con un chip 5G LTE conectado completamente fuera de la red que comunmente se usa como MPLS, Internet, etc.
- Todos los Nexus tienen un puerto Management, incluso los modulares que tienen los orquestadores en el centro. 
- El puerto management pertenece a la VRF management por defecto
- Permite la administracion out of band OOB
- Debe tener una IP asignada
- Debe existir una Default ROute dentro del conectexto de la VRF management

Rollback & Checkpoint Notes

- A diferencia de IOS, en NX se puede crear un punto de retorno o checkpoint para poder hacer roll-backs más fácil.
- Ejemplo, si yo hago un checkpoint, después hago 1203990123 páginas de configuración y algo salió mal, solo hago un roll-back y automáticamente regresaré el checkpoint. 

SSH & Telnet Notes

- Para activar SSH es muy sencillo solo usar "feature ssh"
- Pero para tener mayor seguridad, una vez activado el SSH lo mas recomendable es configurar cosas adicionales como configs de VTY o ACLs.
- A diferencia de IOS la VTY solo se selecciona a nivel general, no una por una (eg. "line vty" en lugar de "line vty 4")

VLAN & Trunk Notes

- Es practicamente igual la configuración de VLAN y Trunks como se hace en un Catalyst IOS
- Los Nexus además de las VLANs reservadas clásicas, reserva para uso interno de la 3968 a la 4095 (Multicast, Online Diagnostic, ERSPAN, Satellite, Multicast VPC, etc)
- En Catalyst una VLAN puede aprender MAC hasta topar con el limite de espacio de una MAC Address Table, en Nexus se puede poner un tope desde antes con el comando "mac address-table limit vlan 50 199" (la vlan 50 solo puede aprender 199 MACs)

Port Profiles Notes

- En Nexus se pueden crear "plantillas" de comandos que se repiten en varias interfaces.
- Por ejemplo, si en 20 interfaces diferentes con exactamente la misma configuración, se puede crear el port profile para crearlo una sola vez y replicarlo en otras interfaces.
- Lo mismo pasa si edito el port profile, todas las interfaces tomarían el cambio. 

Port Channel LACP Notes

- Al igual que en IOS se trata se utilizar varios enlaces entre interfaces como si fuera uno solo, también se configura practicamente igual
- Se pueden configurar de 2 a 16 enlaces y todos deben ser del mismo tipo, velocidad, etc.
- Ninguno debe tener configuración (deben ser seteados a default al inicio de configuración de preferencia), o en caso contrario, todos deberían tener exactamente la misma configuración. 
- PAGP ya no esta disnonible en Cisco, el estándar es LACP (active/passive), lo recomendado es siempre ponerlo en modo active.
- Siempre los port channels deben ir de 2 en 2, por ejemplo 2,4,6,8, etc... esto porque si hay en números nones pensará uno está caído.
- Para backup, si se puede usar un tercer link o un número non, ya que este estarña configurado como backup y solo entrará cuando un par realmente muera. 

HSRP Notes

- Se necesitan 2 Cores (Router/Gateway) ambos con SVI con las VLANs como si uera cualquier otro gatewaya pero duplicado
- Un Core/Gateway Tendrá la IP 192.168.30.254/24 y el otro 92.168.30.253/24
- Se crea una VIP en ambos dentro de HSRP con la IP 192.168.30.1/254
- Así los Hosts verán ambos gateways como uno solo, pero en realidad uno es el activo y el otro el pasivo

Spanning Tree Notes

- Nexus solo soporta rapid-pvst (por default) y multiple (mst), es mejor utilizar RPVST por default. 

## Basic Configurations & Commands: 

````py

!############
!# FEATURES #
!############

!# Show all apliable features (Port Channel, DHCP, BGP, BFD, LACP, Inter-VLAN, etc)
feature ?

!# Add interface VLAN feature (v7 needs to turn on this first, 9k doesn't)
feature interface-vlan

!# Add demo licence to test all features
license grace-period

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########
!# NAMINGS #
!###########

!# Change Switch Hostname
hostname NXv9K-1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!#################
!# USERS & ROLES #
!#################

!# Change Switch Hostname
username fz3r0 password Cisco.12345

!# Enable/Disable password strenght check (Capital+AlfaNumeric+10Chars)
password strength-check
no password strength-check

!# Check all user default available roles
username fz3r0 role ?

!# Set Full Privileges (Priv-15)
username fz3r0 role network-admin

!# Create customized roles (eg. permit commands)
role name Fz3r0-Custom-Role
   rule 1 permit command show interface status
   rule 2 permit command show interface description
   rule 3 deny command show interface
   rule 4 deny command reload
   rule 5 deny command write memory

!# Show roles descriptions
show role name network-admin
show role name Fz3r0-Custom-Role

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!################
!# TELNET & SSH #
!################

!# 1. Enable Telnet & SSH features (Mandatory)
license grace-period
feature telnet
feature ssh

!# 2. Set an Admin Interface (Mandatory/Optional)
interface ethernet 2/1
no shutdown
ip address 192.168.10.1/24
exit

!# 3. Configure VTY [additional security] (Optional)
line vty
   session-limit 5
   exec-timeout 3
exit

!# 4. ACL for permit only certain remote IP Address [additional security] (Optional)
ip access-list remote-access-users
   permit ip host 192.168.10.100 any
exit  
line vty
   access-class remote-access-users in
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##############################
!# INTERFACE RANGE SELECTION
!##############################

#! Configure ONE interface
interface ethernet 1/1

#! Configure RANGE interfaces
interface ethernet 1/1-4

#! Configure DIFFERENT interfaces
interface ethernet 1/1,1/6,1/8

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##################################
!# INTERFACE L3 / IP & ADDRESSING #
!##################################

#! Set interface to L3 (default is L3 already)
interface ethernet 1/1
    no switchport
exit

interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 123.123.123.2/30
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##################################
!# SVI + SET SWITCH/INTERFACE AS GATEWAY #
!##################################

#! 1. Create the VLANs
vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
exit

#! 2. Create SVIs
feature interface-vlan
interface vlan 10
   no shutdown
   description ** SVI+GW-L3-VLAN10-BLUE **
   ip address 192.168.10.254/24
exit
interface vlan 20
   no shutdown
   description ** SVI+GW-L3-VLAN20-RED **
   ip address 192.168.20.254/24
exit
interface vlan 30
   no shutdown
   description ** SVI+GW-L3-VLAN30-GREEN **
   ip address 192.168.30.254/24
exit

#! 3. Set L3 interface to ISP/WAN

interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 123.1.1.2/30
exit

#! 4. Create Default Route to ISP/WAN

ip route 0.0.0.0/0 123.1.1.1/30


!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!##################################
!# HSRP
!##################################

#! CORE 1      = 192.168.x.254 : 
#! CORE 2      = 192.168.x.253 : 
#! HSRP (BOTH) = 192.168.x.1

#! Group 10 = VLAN 10 : 192.168.10.x >> VIP = 192.168.10.1 (HSRP BOTH)
#! Group 20 = VLAN 20 : 192.168.20.x >> VIP = 192.168.20.1 (HSRP BOTH)
#! Group 30 = VLAN 30 : 192.168.30.x >> VIP = 192.168.30.1 (HSRP BOTH)

#! ** CORE 1 ** 192.x.x.254  (ACTIVE)

#! 1. Create the VLANs
vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
exit

#! 2. Create SVIs
feature interface-vlan
interface vlan 10
   no shutdown
   description ** SVI+GW-L3-VLAN10-BLUE **
   ip address 192.168.10.254/24
exit
interface vlan 20
   no shutdown
   description ** SVI+GW-L3-VLAN20-RED **
   ip address 192.168.20.254/24
exit
interface vlan 30
   no shutdown
   description ** SVI+GW-L3-VLAN30-GREEN **
   ip address 192.168.30.254/24
exit

#! 3. Set L3 interface to ISP/WAN

interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 123.1.1.2/30
exit

#! 4. Create Default Route to ISP/WAN

ip route 0.0.0.0/0 123.1.1.1/30

#! 5. Configure HSRP (FOR EACH VLAN) (BOTH SWITCHES = SAME VIP ;))

feature hsrp
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

#! --------------------------------------------

#! ** CORE 2 ** 192.x.x.253 (PASSIVE)

#! 1. Create the VLANs
vlan 10
   name VLAN10-BLUE
vlan 20
   name VLAN10-RED
vlan 30
   name VLAN10-GREEN-MANAGEMENT
exit

#! 2. Create SVIs
feature interface-vlan
interface vlan 10
   no shutdown
   description ** SVI+GW-L3-VLAN10-BLUE **
   ip address 192.168.10.253/24
exit
interface vlan 20
   no shutdown
   description ** SVI+GW-L3-VLAN20-RED **
   ip address 192.168.20.253/24
exit
interface vlan 30
   no shutdown
   description ** SVI+GW-L3-VLAN30-GREEN **
   ip address 192.168.30.253/24
exit

#! 3. Set L3 interface to ISP/WAN

interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 123.2.2.2/30
exit

#! 4. Create Default Route to ISP/WAN

ip route 0.0.0.0/0 123.2.2.1/30

#! 5. Configure HSRP (FOR EACH VLAN) (BOTH SWITCHES = SAME VIP ;))

feature hsrp
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

!# --- HSRP HELP COmmands

show hsrp brief

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-


!##############################
!# INTERFACE L2 BASICS #
!##############################

#! Set interface to L2
interface ethernet 1/1
    switchport
exit

#! Basic Access Config
interface ethernet 1/1
   no shutdown
   switchport
   description ** LAN-L2-VLAN10-INTERFACE **
   switchport mode access
   switchport access vlan 10
exit

!# Basic Trunk Interface (vlan 99 & dot1q is optional)
vlan 99
vlan dot1q tag native
interface ethernet 1/1
   no shutdown
   switchport mode trunk
   switchport trunk native vlan 99
   switchport allowed vlan 50,60,99
exit

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

!# ---

! # Show all VRFs configured on the device
show vrf
! # Show interfaces assigned to the 'management' VRF
show vrf management interface
! # Show running configuration specific to the 'management' VRF
show running-config vrf management

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########################
!# VLANs & TRUNKS
!###########################

!# VLAN Creation & Activation
configure terminal
vlan 50
   state active
   name VLAN50-BLUE
exit

!# VLAN De-activation
configure terminal
vlan 50
   state suspended
exit

!# Access VLAN on interface "X"
configure terminal
interface ethernet 2/10
   no shutdown
   description ** LAN-L2-ACCESS **
   switchport
   switchport mode access
   switchport access vlan 50
exit

!# Trunk interface (vlan 99 & dot1q is optional / no "exit" command is needed)
vlan 99
   name 99-TRUNK-NATIVE
   vlan dot1q tag native

interface ethernet1/4,ethernet1/7
   no shutdown
   description ** LAN-L2-TRUNK **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   cdp enable
exit

---

!# Limit MAC Address table learning
mac address-table limit vlan 50 199

---

! # vlan & trunk show important commands
show vlan
show interface status
show run interface ethernet 2/1
show interface trunk

! # Show VLANs reserved for internal uses (Cisco reserved VLANs)
show vlan interal usage
show system vlan reserved

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########################
!# PORT PROFILE
!###########################

!# Check all available port profile options for interface type (ethernet, interface-vlan(SVI), port-channel)
port-profile type ?

!# Create Port Profile for a basic ethernet interface configuration
port-profile type ethernet fz3r0-interfaces-access-10
   description ACCESS-VLAN10-BLUE
   no shutdown
   switchport mode access
   switchport access vlan 10
   duplex full
state enabled   
exit

!# Create Port Profile for a basic ethernet interface configuration + Using an inherit from other Port Profile (Adds both configs together)
port-profile type ethernet fz3r0-interfaces-access-10-AND-speed100  
   inherit port-profile fz3r0-interfaces-access-10
   description ACCESS-VLAN10-BLUE-SPEED100
   speed100
state enabled 
exit

!# Add Port-Profile to interface range
interface ethernet 1/1-2
   inherit port-profile fz3r0-interfaces-access-10    
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!###########################
!# PORT CHANNEL (LACP)
!###########################

!# Create Port Channel Switch "A" interfaces ethernet 1/1-3 (TURN ON INTERFACES BEFORE PORT CHANNEL!)
feature lacp
default interface ethernet 1/1-2
interface ethernet 1/1-2
   no shutdown
   switchport 
   channel-group 1 mode active
exit

!# Configure the Port Channel created, just like a normal interface
interface port-channel 1
   description PORT-CHANNEL-TRUNK-LINK-1
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport allowed vlan 50,60,99   
exit

!# Create additional 3rd Port Channel connection & force it to add to existing Port Channel + Make it for backup only
default interface ethernet 1/3
interface ethernet 1/3
   no shutdown
   switchport 
   #! Force the new interface to participate in already created port channel
   channel-group 1 force mode active
exit
interface port-channel 1
   #! Only 2 interfaces will be active on port channel (1 pair)
   lacp max-bundle 2
   #! Force 2 links to be always working, if a link fails all the Po will be taken down
   lacp min-links 2
exit
#! Choose the standby interface by prority, default is 32768 (1-65535), highest will be the backup
interface ethernet 1/3
   lacp port-priority 39000
exit

!# Select load balance method for the Port Channel (src-dst is always good!)
port-channel load-balance src-dst

!# ---

!# Port Channel Help Commands
show port-channel
show port-channel summary
show interface status
show port-channel load-balance

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!#######################
!# SPANNINGTREE (R-PVSTP) #
!#######################

!# Activate R-PVSTP (per default is already active)
spanning-tree mode rapid-pvst

!# R-PVSTP set ROOT device (ONLY ONE ROOT per VLAN/NETWORK!!!)
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30 root primary

!# R-PVSTP set SECONDARY device (ONLY ONE SECONDARY per VLAN/NETWORK!!!)
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30 root secondary

!# R-PVSTP port type = <<EDGE>> (for edge devices like PC/Access) - Activate Port Fast
interface ethernet 1/3
   switchport
   switchport mode access
   spanning-tree port type edge
exit

!# R-PVSTP port type = <<NETWORK>> (for network devices Switch/Trunk) - Activate Bridge Assurance & BPDUs
interface ethernet 1/5
   switchport
   switchport mode trunk
   spanning-tree port type network
exit

!# R-PVSTP BPDUGUARD Security - Block Access Port if detects BPDUs (rogue switch)
interface ethernet 1/1
   switchport
   switchport mode access
   spanning-tree port type edge
   spanning-tree bpduguard enable
exit

!# ---

!# PVRSTP Help Commands
show spanning-tree

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!#######################
!# SPEED & DUPLEX #
!#######################

! # Set full duplex speed on interface
interface ethernet 1/1
   duplex full
exit

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

!##########################################
!# SET DEFAULT SETTINGS
!##########################################

!# Set all interfaces to Layer 2 (On Nexus all interfaces are L3 by default)
system default switchport

!# Set default settings for "X" interface
default interface ethernet 1/1

!# Set default settings for a range from "X" to "Z" interfaces
default interface ethernet 1/1-3

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


````

## Help Commands

````py
!##########
!# BASIC SHOW #
!##########

!#
show running

!#
show interface status

!# Show connected users at the moment and VTYs used by each one
show users


!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-


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







# LAB EXAMPLE FZ3R0 BIBLE


## Switch NX9-1 - ACTIVE HSRP (Priority 200)

````py
!######################
!# NEXUS NX9-1 - ROOT (ACTIVE / .254 PRIORITY 200)
!######################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname Nx9-1-CR-ACT
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

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

#! SVIs (GATEWAY L3)

feature interface-vlan
interface vlan 10
   no shutdown
   description ** SVI+GW-L3-VLAN10-BLUE **
   ip address 192.168.10.254/24
exit
interface vlan 20
   no shutdown
   description ** SVI+GW-L3-VLAN20-RED **
   ip address 192.168.20.254/24
exit
interface vlan 30
   no shutdown
   description ** SVI+GW-L3-VLAN30-GREEN **
   ip address 192.168.30.254/24
exit

#! L3 WAN INTERFACE @ INTERNET

interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 123.1.1.2/30
   speed 1000
   duplex full
   cdp enable
exit

#! Create Default Route to ISP/WAN

ip route 0.0.0.0/0 123.1.1.1

#! Configure HSRP (FOR EACH VLAN) (BOTH SWITCHES = SAME VIP ;))

feature hsrp
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

!# L2 INTERFACES - TRUNK

interface ethernet1/4,ethernet1/7
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
   cdp enable
exit

!# TELNET & SSH #

feature telnet
feature ssh
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

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-1
copy running-config startup-config
!
!


````



## Switch NX9-2 - PASSIVE HSRP (Priority 100)

````py
!######################
!# NEXUS NX9-2 - ROOT (PASSIVE / .253 PRIORITY 100)
!######################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname Nx9-2-CR-PSV
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

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

#! SVIs (GATEWAY L3)

feature interface-vlan
interface vlan 10
   no shutdown
   description ** SVI+GW-L3-VLAN10-BLUE **
   ip address 192.168.10.253/24
exit
interface vlan 20
   no shutdown
   description ** SVI+GW-L3-VLAN20-RED **
   ip address 192.168.20.253/24
exit
interface vlan 30
   no shutdown
   description ** SVI+GW-L3-VLAN30-GREEN **
   ip address 192.168.30.253/24
exit

#! L3 WAN INTERFACE @ INTERNET

interface ethernet 1/1
   no shutdown
   no switchport
   description ** WAN-L3-INTERFACE **
   ip address 123.2.2.2/30
   speed 1000
   duplex full
   cdp enable
exit

#! Create Default Route to ISP/WAN

ip route 0.0.0.0/0 123.2.2.1

#! Configure HSRP (FOR EACH VLAN) (BOTH SWITCHES = SAME VIP ;))

feature hsrp
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

!# L2 INTERFACES - TRUNK

interface ethernet1/4,ethernet1/7
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
   cdp enable
exit

!# TELNET & SSH #

feature telnet
feature ssh
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

!# SAVE CHECKPOINT & CONFIGURATION

end
checkpoint fz3r0-check-2025-NX9-2
copy running-config startup-config

!
!



````














































## Switch NX9-11 - ACCESS

````py
!######################
!# NEXUS NX9-11-ACCESS
!######################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname Nx9-11-ACCESS
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

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

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)

feature interface-vlan
interface vlan 30
   no shutdown
   description ** SVI-MGMT-L3-VLAN30-GREEN **
   ip address 192.168.30.11/24
exit

!# ip default-gateway 192.168.30.1 <- This command is deprecated.
ip route 0.0.0.0/0 192.168.30.1

!# L2 PORT CHANNELS 

feature lacp
default interface ethernet 1/1-3
interface ethernet 1/1-3
   description ** Port-Channel-1-Po1-Interfaces **
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
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
   cdp enable

interface ethernet1/4
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
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
   cdp enable
exit

!# TELNET & SSH #

feature telnet
feature ssh
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

!# SAVE CHECKPOINT & CONFIGURATION
end
checkpoint fz3r0-check-2025-NX9-1
copy running-config startup-config


!
!


````




## Switch NX9-12 - ACCESS

````py
!######################
!# NEXUS NX9-12-ACCESS
!######################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname Nx9-12-ACCESS
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

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

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)

feature interface-vlan
interface vlan 30
   no shutdown
   description ** SVI-MGMT-L3-VLAN30-GREEN **
   ip address 192.168.30.12/24
exit

!# ip default-gateway 192.168.30.1 <- This command is deprecated.
ip route 0.0.0.0/0 192.168.30.1

!# L2 PORT CHANNELS 

!# Po1

feature lacp
default interface ethernet 1/1-3
interface ethernet 1/1-3
   description ** Port-Channel-1-Po1-Interfaces **
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

feature lacp
default interface ethernet 1/4-6
interface ethernet 1/4-6
   description ** Port-Channel-2-Po2-Interfaces **
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
   description ** Port-Channel-1-L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
   cdp enable

interface Ethernet1/7
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
   cdp enable
exit

!# TELNET & SSH #

feature telnet
feature ssh
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

!# SAVE CHECKPOINT & CONFIGURATION
end
checkpoint fz3r0-check-2025-NX9-1
copy running-config startup-config


!
!


````



























## Switch NX9-14 - ACCESS

````py
!######################
!# NEXUS NX9-14-ACCESS
!######################

!# NAMINGS, USERS, LICENCES, DISCOVERY

configure terminal
hostname Nx9-14-ACCESS
password strength-check
username admin password Adm1n.C1sc0
username fz3r0 password Adm1n.C1sc0
username fz3r0 role network-admin
!   license grace-period
cdp enable

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

#! SVIs (MANAGEMENT) + DEFAULT GATEWAY (HSRP CORES)

feature interface-vlan
interface vlan 30
   no shutdown
   description ** SVI-MGMT-L3-VLAN30-GREEN **
   ip address 192.168.30.14/24
exit

!# ip default-gateway 192.168.30.1 <- This command is deprecated.
ip route 0.0.0.0/0 192.168.30.1

!# L2 PORT CHANNELS 

feature lacp
default interface ethernet 1/1-3
interface ethernet 1/1-3
   description ** Port-Channel-1-Po1-Interfaces **
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

interface ethernet1/4
   no shutdown
   description ** L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
   cdp enable
exit

interface port-channel 1
   description ** Port-Channel-1-L2-TRUNK-NATIVE99 **
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
   speed 1000
   duplex full
   cdp enable

!# L2 INTERFACES - ACCESS

interface ethernet1/5
   no shutdown
   description ** L2-ACCESS-VLAN10-BLUE **
   switchport
   switchport mode access
   switchport access vlan 10
   speed 1000
   duplex full
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
   cdp enable
exit

!# TELNET & SSH #

feature telnet
feature ssh
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

!# SAVE CHECKPOINT & CONFIGURATION
end
checkpoint fz3r0-check-2025-NX9-1
copy running-config startup-config


!
!


````

































## WANS / INTERNET

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
  description LINK-TO-NETWORK
  ip address 123.1.1.1 255.255.255.252
  duplex full
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
  description LINK-TO-NETWORK
  ip address 123.2.2.1 255.255.255.252
  duplex full
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








## PC

- PCs are IOS routers simulating a PC (for having shh, telnet, ping, traceroute, etc)

| PC Name | VLAN         | IP Address        | Subnet Mask     | CIDR  | Default Gateway   |
|---------|--------------|-------------------|------------------|-------|-------------------|
| PC-1    | VLAN10-BLUE  | 192.168.10.101     | 255.255.255.0    | /24   | 192.168.10.1      |
| PC-4    | VLAN10-BLUE  | 192.168.10.102     | 255.255.255.0    | /24   | 192.168.10.1      |
| PC-2    | VLAN20-RED   | 192.168.20.101     | 255.255.255.0    | /24   | 192.168.20.1      |
| PC-5    | VLAN20-RED   | 192.168.20.102     | 255.255.255.0    | /24   | 192.168.20.1      |
| PC-3    | VLAN30-GREEN | 192.168.30.101     | 255.255.255.0    | /24   | 192.168.30.1      |
| PC-6    | VLAN30-GREEN | 192.168.30.102     | 255.255.255.0    | /24   | 192.168.30.1      |


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




# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=lADK3STwwAM&list=PLwAU7bA502wFB5j6RnpDPNG5xwb5JEbq8
- https://www.youtube.com/watch?v=fdc912ReAE4
- https://youtu.be/oBJNkFhPpfU?si=BpyN82rV99dBf_cI
- https://youtu.be/aPzNzvyv20A?si=1QMckKT0AjZHR1bm
- https://youtu.be/ieZkA7Ayc-4?si=XgsEx2Pz87hLBZM5
- https://youtu.be/IFb-Ncj5w-E?si=BsXcc9WlyG-W0vpu



  
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

