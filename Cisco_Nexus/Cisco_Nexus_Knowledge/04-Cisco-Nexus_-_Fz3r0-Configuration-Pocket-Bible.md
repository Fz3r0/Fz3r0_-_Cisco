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

I will be using this topology for all the examples & configurations:

![image](https://github.com/user-attachments/assets/15266b6c-694d-4786-96d8-cc81b0616ccb)


## Cisco Nexus & NX-OS Notes:

v7 VS v9 images:

- 7k Image doesn't support port-channel or LACP, unfortunately.
- The only problem I have with the 9k Image is that it does not support multiple VDCs.

Basic Notes:

- No existen los comandos enable en nexus 9k (NX-OS) para cambiar ese nivel en CLI.
- El comando "show" se puede ejecutar donde sea (a diferenciade IOS que solo se puede en el primer nivel de CLI o teniendo que usar "do")
- Todas las interfaces por default vienen como "routed" layer 3 en nuxus 9k, es decir, no son el clásico capa 2 como en IOS/Catalyst.
- Todas las interfaces por default vienen como "disabled/shut down" (apagadas)
- A menos que se manipulen los timers de OSPF, BGP, etc, cada que se haga un failover puede tardar hasta 10 segundos, no desesperes!!! (o modifica los timers)

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
- Para hacer un Port Channel L3 la única diferencia es crearlo cuando antes las interfaces estan configuradas con "no switchport" (L3), en caso que tengan "switchport" se crearán como L2. 

HSRP Notes

- Se necesitan 2 Cores (Router/Gateway) ambos con SVI con las VLANs como si uera cualquier otro gatewaya pero duplicado
- Un Core/Gateway Tendrá la IP 192.168.30.254/24 y el otro 92.168.30.253/24
- Se crea una VIP en ambos dentro de HSRP con la IP 192.168.30.1/254
- Así los Hosts verán ambos gateways como uno solo, pero en realidad uno es el activo y el otro el pasivo
- Para usar HSRP y DHCP es mejor usar un DHCP server aparte y usar DHCP relay, ya que si se configura DHCP en ambos cores podría haber problemas. Otra técnica no muy limpia es crear el DHCP server en ambos, con DHCP pools que no repitan IPs eg. SW.1 1-50 / SW.2 51-100
- Para tener redundancia de DHCP se necesitaría un servidor DHCP mas dedicado que unos simples routers o switches Cisco, ya sea Windows Server DHCP con failover, o DHCP cluster en Infoblox o algo por el estilo. 

Spanning Tree Notes

- Nexus solo soporta rapid-pvst (por default) y multiple (mst), es mejor utilizar RPVST por default.
- El core es casi siempre el mejor candidato para ser root bridge, porque está en el centro de tu red y conecta la mayoría de los enlaces troncales, tiene más capacidad de cómputo y estabilidad que los switches de acceso, al hacer que el core sea root, minimizas la latencia y evitas que STP tenga que reelecciones innecesarias en el borde.
- Network Port: por defecto en NX-OS bloquea cualquier puerto tipo “network” donde no reciba BPDUs del otro extremo. Cuando no se ha levantado STP en un extremo trunk,  bloquea los puertos trunk para “asegurar” que no haya bucles unidireccionales. (Esto se arregla configurando el trunk del otro lado... o se apaga par aque ningun loquillo en PC vea esos BPDUs)
- Edge Port: Para puertos de acceso, deja de enviar BPDUs y si recibe alguno de un loquillo bloquea el puerto. Habilita port fast para encender mas rapido el puerto sin negociar ni calcular STP. 

NAT

- Se configura basicamente igual que en IOS
- En NX9 de simulador no existe feature NAT, por eso en este lab se hace en router aparte

OSPF (Full Mesh en Core Layer)

- En esta config uso una config básica de OSPF punto a punto, tanto en router como en NX se debe configurar el p2p en cad ainterfaz.
- Aunque se use Full mesh para alta disponibilidad y redundancia con los otros sw y routers es recomendable mantener los p2p entre las interfaces principales ya que No generan tráfico de OSPF tipo broadcast, son más rápidos de converger y más seguros y fáciles de controlar.
- Si en algún momento se agrega un switch entre dos dispositivos OSPF (aunque no enrute), ya no se debería usar point-to-point, porque se vuelve multiacceso y OSPF cambiará el comportamiento. En este caso
- Point-to-Point	- l	No hay DR/BDR, LSA tipo 1 únicamente

### 🔍 OSPF Role Classification for Lab Routers

| Router    | OSPF Area(s) | Redistributes External Routes? | OSPF Role              | Notes                                           |
|-----------|--------------|-------------------------------|-------------------------|-------------------------------------------------|
| NX9-1     | Area 0        | No                            | Internal + Backbone     | Part of Area 0 only, does not redistribute      |
| NX9-2     | Area 0        | No                            | Internal + Backbone     | Same as NX9-1                                   |
| RT-1      | Area 0        | Yes (`default-information originate`) | ASBR + Backbone     | Injects default route from static to OSPF       |
| RT-2      | Area 0        | Yes (`default-information originate`) | ASBR + Backbone     | Same behavior as RT-1                           |


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

!# Create L2 Port Channel Switch "A" interfaces ethernet 1/1-3 (TURN ON INTERFACES BEFORE PORT CHANNEL!) {L2 = switchport}
feature lacp
default interface ethernet 1/1-2
interface ethernet 1/1-2
   no shutdown
   switchport 
   channel-group 1 mode active
exit

!# Create L3 Port Channel Switch "X" interfaces ethernet 1/1-3 (TURN ON INTERFACES BEFORE PORT CHANNEL!) {L3 = no switchport}
feature lacp
default interface ethernet 1/1-2
interface ethernet 1/1-2
   no shutdown
   no switchport 
   channel-group 1 mode active
exit

!# Configure the Port Channel created, just like a normal interface
interface port-channel 1
   description PORT-CHANNEL-L2-TRUNK-LINK-1
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
!# OSPF
!##########################################

feature ospf
!# Crear OSPF 1 + anunciar todas las redes LAN y WAN + crear default route @ WAN
router ospf 1
    network 192.168.10.0/24 area 0
    network 192.168.20.0/24 area 0
    network 192.168.30.0/24 area 0
    network 123.1.1.0/30  area 0
    exit
    !
    !# Crear default route hacia la WAN/ISP (LA DEFAULT ROUTE DEBE ESTAR DENTRO DE LA CONFIGURACIÓN DE OSPF!!!!) <<<
    ip route 0.0.0.0/0 123.1.1.1
exit

! # hELP COMMANDS

!# Vecinos OSPF

!# Lista de vecinos OSPF (ID, interfaz, estado, tiempo)
show ip ospf neighbor
!# Resumen de interfaces con OSPF habilitado (tipo, estado, cost)
show ip ospf interface brief
!# Detalles de OSPF por interfaz específica
show ip ospf interface Ethernet0/1

!# Estado del proceso OSPF

!# Estado general del proceso OSPF (router-id, áreas, roles, ASBR)
show ip ospf
!# Eventos importantes del proceso (solo NX-OS o con ciertos IOS)
show ip ospf events
!# Estadísticas generales de OSPF (LSAs, SPF runs, errores)
show ip ospf statistics

!# Base de datos SPF (LSDB)

!# Base de datos OSPF completa
show ip ospf database
!# LSA tipo 1 - Routers conectados directamente
show ip ospf database router
!# LSA tipo 2 - Redes broadcast (DR/BDR) [No aplica en enlaces p2p]
show ip ospf database network
!# LSA tipo 3 - Rutas entre áreas (summary LSAs)
show ip ospf database summary
!# LSA tipo 5 - Rutas externas (ej. default route desde ASBR)
show ip ospf database external
!# LSAs generadas por este router
show ip ospf database self-originate
!# Ver los enlaces y vecinos de un router específico
show ip ospf database router 1.1.1.1

!# Tabla de rutas OSPF

!# Ver únicamente las rutas OSPF aprendidas
show ip route ospf
!# Ver toda la tabla de rutas con prefijos OSPF marcados con "O"
show ip route
!# Ver por qué camino llega la ruta default
show ip route 0.0.0.0
!# Ver la mejor ruta hacia una IP específica
show ip route 192.168.20.1

!# Debugs para laboratorio

!# Ver cambios de vecinos OSPF en tiempo real
debug ip ospf adj
!# Ver cada evento de OSPF: timers, LSA, SPF, etc.
debug ip ospf events
!# Ver SPF recalculation (muy útil cuando cae una interfaz)
debug ip ospf spf

!# Extra para NX-OS (si aplica en tu lab)

!# Ver la RIB interna de OSPF en Nexus
show ip ospf rib
!# Ver eventos OSPF históricos
show ip ospf events
!# Ver todos los interfaces NX-OS con OSPF, sus estados y costos
show ip ospf interface brief

--

!##########################################
!# NAT
!##########################################

!# NO DISPONIBLE EN IMAGEN VIRTUAL DE GNS3 :(

!# Enable NAT
configure terminal
feature nat

!# 1. Mark WAN link as NAT outside
interface Ethernet1/1
   no shutdown
   ip nat outside
exit

!# 2. Mark each SVI as NAT inside
interface Vlan10
   ip nat inside
exit
interface Vlan20
   ip nat inside
exit
interface Vlan30
   ip nat inside
exit

!# 3. Define which source IPs to NAT
ip access-list extended NAT_INSIDE
   permit ip 192.168.10.0/24 any
   permit ip 192.168.20.0/24 any
   permit ip 192.168.30.0/24 any
exit

!# 4. Overload all matching inside traffic to the WAN interface address
   ip nat inside source list NAT_INSIDE interface Ethernet1/1 overload

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































## 








# Fz3r0 Cisco Labs: `Data Center v1`

This Cisco Nexus lab is designed for engineers transitioning from IOS switches to NX-OS. It covers essential data-center concepts and foundational protocols—HSRP, OSPF, Rapid PVST+, LACP—and common NX-OS configuration patterns. Upon completion, you’ll be ready to tackle advanced features like VXLAN, VPC and EVPN.

## Fz3r0 Nexus Lab: Important Notes

- Se deben encender y configurar primero los NX9-1 y 2, antes de los edge routers o el OSPF 1 da error. 

## 🎯 Objectives   

- **Hands-on with NX-OS CLI**: Master feature activation and NX-OS naming.  
- **High Availability**: Implement HSRP for gateway redundancy.  
- **Dynamic Routing**: Deploy OSPF Area 0 across core and edge devices.  
- **Layer 2 Resiliency**: Configure Rapid PVST+ and port channels using LACP.  
- **SVI Gateways**: Build SVIs per VLAN and integrate them into OSPF.  
- **Inter-site Connectivity & NAT**: Simulate edge routers with multiple WAN links and NAT overload.

## 📋 Device Roles & IP Addressing   

| Device         | Role                        | Interface / Network      | IP Address        | Notes                          |
|----------------|-----------------------------|--------------------------|-------------------|--------------------------------|
| **NX9-1**      | Core Active (HSRP A)        | VLAN 10 SVI              | 192.168.10.254/24 | HSRP VIP .1, OSPF area 0       |
|                |                             | VLAN 20 SVI              | 192.168.20.254/24 |                                |
|                |                             | VLAN 30 SVI              | 192.168.30.254/24 |                                |
|                |                             | Port-Channel 1           | 10.50.0.1/30      | OSPF cost 10                   |
|                |                             | Eth1/1 → RT-1            | 10.10.0.2/30      | OSPF cost 1                    |
|                |                             | Eth1/2 → RT-2            | 10.40.0.1/30      | OSPF cost 100                  |
| **NX9-2**      | Core Standby (HSRP B)       | VLAN 10 SVI              | 192.168.10.253/24 | HSRP VIP .1, OSPF area 0       |
|                |                             | VLAN 20 SVI              | 192.168.20.253/24 |                                |
|                |                             | VLAN 30 SVI              | 192.168.30.253/24 |                                |
|                |                             | Port-Channel 1           | 10.50.0.2/30      | OSPF cost 10                   |
|                |                             | Eth1/1 → RT-2            | 10.20.0.2/30      | OSPF cost 1                    |
|                |                             | Eth1/2 → RT-1            | 10.30.0.2/30      | OSPF cost 100                  |
| **NX9-11**     | Access Switch (Mgmt)        | VLAN 30 SVI              | 192.168.30.11/24  | Default GW 192.168.30.1        |
| **NX9-12**     | Access Switch (Mgmt)        | VLAN 30 SVI              | 192.168.30.12/24  | Default GW 192.168.30.1        |
| **NX9-13**     | Access Switch (Mgmt)        | VLAN 30 SVI              | 192.168.30.13/24  | Default GW 192.168.30.1        |
| **NX9-14**     | Access Switch (Mgmt)        | VLAN 30 SVI              | 192.168.30.14/24  | Default GW 192.168.30.1        |
| **RT-1-EDGE**  | Edge Router 1               | Eth0/1 → NX9-1           | 10.10.0.1/30      | OSPF cost 1, NAT inside        |
|                |                             | Eth0/2 → NX9-2           | 10.30.0.1/30      | OSPF cost 100, NAT inside      |
|                |                             | Eth0/3 → RT-2            | 10.60.0.1/30      | OSPF cost 200                  |
|                |                             | Eth0/0 (WAN-1 outside)   | 123.1.1.2/30      | NAT outside                    |
| **RT-2-EDGE**  | Edge Router 2               | Eth0/1 → NX9-2           | 10.20.0.1/30      | OSPF cost 1, NAT inside        |
|                |                             | Eth0/2 → NX9-1           | 10.40.0.2/30      | OSPF cost 100, NAT inside      |
|                |                             | Eth0/3 → RT-1            | 10.60.0.2/30      | OSPF cost 200                  |
|                |                             | Eth0/0 (WAN-2 outside)   | 123.2.2.2/30      | NAT outside                    |
| **WAN-1**      | Simulated Internet A        | Eth0/0                   | 123.1.1.1/30      | Default route → 123.1.1.2      |
|                |                             | Loopback0                | 8.8.8.8/32        | Google DNS                     |
|                |                             | Loopback1                | 8.8.4.4/32        | Google DNS                     |
| **WAN-2**      | Simulated Internet B        | Eth0/0                   | 123.2.2.1/30      | Default route → 123.2.2.2      |
|                |                             | Loopback0                | 8.8.8.8/32        | Google DNS                     |
|                |                             | Loopback1                | 8.8.4.4/32        | Google DNS                     |
| **Server-1**   | Host in VLAN 10 (Blue)      | Eth0                     | 192.168.10.101/24 | GW 192.168.10.1                |
| **Server-2**   | Host in VLAN 20 (Red)       | Eth0                     | 192.168.20.101/24 | GW 192.168.20.1                |
| **Server-3**   | Host in VLAN 30 (Green)     | Eth0                     | 192.168.30.101/24 | GW 192.168.30.1                |
| **Server-4**   | Host in VLAN 10 (Blue)      | Eth0                     | 192.168.10.102/24 | GW 192.168.10.1                |
| **Server-5**   | Host in VLAN 20 (Red)       | Eth0                     | 192.168.20.102/24 | GW 192.168.20.1                |
| **Server-6**   | Host in VLAN 30 (Green)     | Eth0                     | 192.168.30.102/24 | GW 192.168.30.1                |




## Switch NX9-1 - ACTIVE HSRP (Priority 200)

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

+ DEVICE    =  NX9-1 
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.254
+ VIP(HSRP) =  192.168.30.1 

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



## Switch NX9-2 - PASSIVE HSRP (Priority 100)

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

+ DEVICE    =  NX9-2 
+ SITE      =  SITE_A
+ LOCATION  =  RAPTOR PADDOK
+ ADMIN 1   =  Fz3r0
+ ADMIN 2   =  Dennis Nedry

+ IP        =  192.168.30.253
+ VIP(HSRP) =  192.168.30.1 

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














































## Switch NX9-11 - ACCESS

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




## Switch NX9-12 - ACCESS

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








## Switch NX9-13 - ACCESS

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







## Switch NX9-14 - ACCESS

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

































## Router 1 Edge

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


## Router 2 Edge

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





## MPLS

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




| Switch            | Interface                   | STP Role        | Port State  |
|-------------------|-----------------------------|-----------------|-------------|
| **NX9-1 (CORE)**  | Ethernet1/4                 | Designated      | Forwarding  |
|                   | Ethernet1/7                 | Designated      | Forwarding  |
| **NX9-11 (ACCESS)** | Ethernet1/4               | Root Port       | Forwarding  |
|                   | Port-Channel1               | Designated      | Forwarding  |
|                   | Ethernet1/5, 1/6, 1/7        | Edge (PortFast) | Forwarding  |
| **NX9-12 (ACCESS)** | Ethernet1/7               | Root Port       | Forwarding  |
|                   | Port-Channel1               | Alternate       | Blocking    |
|                   | Access Ports                | Edge (PortFast) | Forwarding  |




# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=lADK3STwwAM&list=PLwAU7bA502wFB5j6RnpDPNG5xwb5JEbq8
- https://www.youtube.com/watch?v=fdc912ReAE4
- https://youtu.be/oBJNkFhPpfU?si=BpyN82rV99dBf_cI
- https://youtu.be/aPzNzvyv20A?si=1QMckKT0AjZHR1bm
- https://youtu.be/ieZkA7Ayc-4?si=XgsEx2Pz87hLBZM5
- https://youtu.be/IFb-Ncj5w-E?si=BsXcc9WlyG-W0vpu
- https://journey2theccie.wordpress.com/2020/07/20/hsrp-aware-dhcp-relay/
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

