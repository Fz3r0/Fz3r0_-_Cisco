# 🧠🏗️🌐 Cisco Nexus: `Fz3r0 :: Nexus Configuration Bible`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Datacenter` `NX-OS` `Cisco Nexus` `Leaf & Spine` `DC Switching` `Modular Chassis` `Fixed Form Factor` `vPC` `ECMP` `FEX` `Fabric Extender` `Nexus 2000` `Nexus 5000` `Nexus 7000` `Nexus 9000`  `Port-Channel` `LAG` `VLAN` `VRF` `Overlay` `Underlay` `EVPN` `VXLAN`  `Spine Switch` `Leaf Switch` `Control Plane` `Forwarding Plane` `Supervisor Card` `Line Card`  `Redundant Power Supply` `Hot-swappable` `In-Service Software Upgrade (ISSU)` `High Availability`  `Twinax Cable` `SFP+` `QSFP28` `QSFP-DD` `10G` `25G` `40G` `100G` `400G`

#### Target Audience: CCNA/CCNP+ level engineers transitioning from IOS switches (Catalyst) to NX-OS (Nexus).

#### Prerequisites: Basic understanding of Cisco IOS, OSPF, HSRP, LACP, and Rapid PVST+; _familiarity with NX-OS CLI conventions is helpful but not required._

---

<br>

# 📝❓📄 `Index`



# ⚡ Cisco Nexus - Fz3r0 :: Nexus Init Confioguration Bible

This lab is designed for engineers transitioning from IOS switches (Cisco Catalyst) to NX-OS (Cisco Nexus). It covers essential network/data-center concepts and foundational protocols like VLANs, Trunk & Access Interfaces, SVI, HSRP, Static & Default Routing, OSPF, Rapid PVST+, Port Channels LACP, and common NX-OS configuration patterns. Upon completion, you’ll be ready to tackle advanced features like VRF, VPC, VXLAN or EVPN.

The lab recreates a typical two-tier collapsed Core/Distribution topology connected to simulated WAN and MPLS circuits, with four downstream access switches and six end-hosts. 

### Key Features & Protocols Covered

1. **NX-OS feature activation** (`feature interface-vlan`, `feature hsrp`, etc.)  
2. **HSRP (v2)** for gateway redundancy on SVIs  
3. **OSPF Area 0** full-mesh adjacency among core and edge devices  
4. **Rapid PVST+** for STP resiliency on access layer trunks  
5. **Port-channel (LACP)** bundling for inter-switch links  
6. **SVI gateways per VLAN**, advertised into OSPF  
7. **NAT overload** (`ip nat inside/outside`) on edge routers for outbound Internet  
8. **Static routing** to simulate MPLS circuits  
9. **Collapsed Core/Distribution** architecture with L3 all-in-one on NX9  

## Topology

![image](https://github.com/user-attachments/assets/cd6e8dc1-ee2e-46e8-8d82-5df8bf4f4deb)

## Network Device Inventory & IP Address Summary

| Device                      | Function                    | Interface / VLAN                | IP (Mask)           | VIP / Gateway  | *Network*      | *Broadcast*      | Main Description                           |
| --------------------------- | --------------------------- | ------------------------------- | ------------------- | -------------- | -------------- | ---------------- | ------------------------------------------ |
| **NX9-1-CR-ACT**<br>(NXOS)  | Core L3/L2 (Active HSRP)    | VLAN 10 (SVI)                   | `192.168.10.254/24` | `192.168.10.1` | *192.168.10.0* | *192.168.10.255* | SVI GW VLAN 10–BLUE                        |
|                             |                             | VLAN 20 (SVI)                   | `192.168.20.254/24` | `192.168.20.1` | *192.168.20.0* | *192.168.20.255* | SVI GW VLAN 20–RED                         |
|                             |                             | VLAN 30 (SVI)                   | `192.168.30.254/24` | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | SVI GW VLAN 30–GREEN-MGT                   |
|                             |                             | Port-Channel 1 (P2P OSPF)       | `10.50.0.1/30`      | –              | *10.50.0.0*    | *10.50.0.3*      | OSPF backup link to NX9-2 (cost 100)       |
|                             |                             | Eth1/1 (P2P OSPF)               | `10.10.0.2/30`      | –              | *10.10.0.0*    | *10.10.0.3*      | OSPF primary link to RT-1 (cost 1)         |
|                             |                             | Eth1/2 (P2P OSPF)               | `10.40.0.1/30`      | –              | *10.40.0.0*    | *10.40.0.3*      | OSPF backup link to RT-2 (cost 100)        |
|                             |                             | Eth1/3,1/5-6 (Port-Channel 0)   | –                   | –              | –              | –                | L3 Port-Channel between NX9-1 and NX9-2    |
|                             |                             | Eth1/4,1/7 (trunks VLANs)       | trunk VLAN 10,20,30 | –              | –              | –                | L2 uplinks to Access                       |
|                             |                             | Management (VLAN 30)            | `192.168.30.254/24` | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | HSRP Active on VLAN 30                     |
| **NX9-2-CR-STB**<br>(NXOS)  | Core L3/L2 (Standby HSRP)   | VLAN 10 (SVI)                   | `192.168.10.253/24` | `192.168.10.1` | *192.168.10.0* | *192.168.10.255* | SVI GW VLAN 10–BLUE                        |
|                             |                             | VLAN 20 (SVI)                   | `192.168.20.253/24` | `192.168.20.1` | *192.168.20.0* | *192.168.20.255* | SVI GW VLAN 20–RED                         |
|                             |                             | VLAN 30 (SVI)                   | `192.168.30.253/24` | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | SVI GW VLAN 30–GREEN-MGT                   |
|                             |                             | Port-Channel 1 (P2P OSPF)       | `10.50.0.2/30`      | –              | *10.50.0.0*    | *10.50.0.3*      | OSPF link to NX9-1 (cost 100)              |
|                             |                             | Eth1/1 (P2P OSPF)               | `10.20.0.2/30`      | –              | *10.20.0.0*    | *10.20.0.3*      | OSPF primary link to RT-2 (cost 1)         |
|                             |                             | Eth1/2 (P2P OSPF)               | `10.30.0.2/30`      | –              | *10.30.0.0*    | *10.30.0.3*      | OSPF backup link to RT-1 (cost 100)        |
|                             |                             | Eth1/3,1/5-6 (Port-Channel 0)   | –                   | –              | –              | –                | L3 Port-Channel between NX9-2 and NX9-1    |
|                             |                             | Eth1/4,1/7 (trunks VLANs)       | trunk VLAN 10,20,30 | –              | –              | –                | L2 uplinks to Access                       |
|                             |                             | Management (VLAN 30)            | `192.168.30.253/24` | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | HSRP Standby on VLAN 30                    |
| **NX9-11-ACCESS**<br>(NXOS) | L2 Access Switch            | VLAN 30 (SVI mgmt)              | `192.168.30.11/24`  | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Management – default-route to HSRP Core    |
|                             |                             | Port-Channel 1 (trunk)          | trunk VLAN 10,20,30 | –              | –              | –                | Redundant uplink to Core                   |
|                             |                             | Eth1/4 (trunk)                  | trunk VLAN 10,20,30 | –              | –              | –                | Additional uplink to Core                  |
|                             |                             | Eth1/5 (access VLAN 10)         | `192.168.10.x/24`   | `192.168.10.1` | *192.168.10.0* | *192.168.10.255* | Access port VLAN 10                        |
|                             |                             | Eth1/6 (access VLAN 20)         | `192.168.20.x/24`   | `192.168.20.1` | *192.168.20.0* | *192.168.20.255* | Access port VLAN 20                        |
|                             |                             | Eth1/7 (access VLAN 30)         | `192.168.30.x/24`   | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Access port VLAN 30                        |
| **NX9-12-ACCESS**<br>(NXOS) | L2 Access Switch            | VLAN 30 (SVI mgmt)              | `192.168.30.12/24`  | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Management – default-route to HSRP Core    |
|                             |                             | Port-Channel 1 (trunk)          | trunk VLAN 10,20,30 | –              | –              | –                | Redundant uplink to Core                   |
|                             |                             | Port-Channel 2 (trunk)          | trunk VLAN 10,20,30 | –              | –              | –                | 2nd redundant uplink to Core               |
|                             |                             | Eth1/7 (trunk)                  | trunk VLAN 10,20,30 | –              | –              | –                | Additional uplink to Core                  |
| **NX9-13-ACCESS**<br>(NXOS) | L2 Access Switch            | VLAN 30 (SVI mgmt)              | `192.168.30.13/24`  | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Management – default-route to HSRP Core    |
|                             |                             | Port-Channel 1 (trunk)          | trunk VLAN 10,20,30 | –              | –              | –                | Redundant uplink (1) to Core               |
|                             |                             | Port-Channel 2 (trunk)          | trunk VLAN 10,20,30 | –              | –              | –                | Redundant uplink (2) to Core               |
|                             |                             | Eth1/7 (trunk)                  | trunk VLAN 10,20,30 | –              | –              | –                | Additional uplink to Core                  |
| **NX9-14-ACCESS**<br>(NXOS) | L2 Access Switch            | VLAN 30 (SVI mgmt)              | `192.168.30.14/24`  | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Management – default-route to HSRP Core    |
|                             |                             | Port-Channel 1 (trunk)          | trunk VLAN 10,20,30 | –              | –              | –                | Redundant uplink (1) to Core               |
|                             |                             | Eth1/4 (trunk)                  | trunk VLAN 10,20,30 | –              | –              | –                | Redundant uplink (2) to Core               |
|                             |                             | Eth1/5 (access VLAN 10)         | `192.168.10.x/24`   | `192.168.10.1` | *192.168.10.0* | *192.168.10.255* | Access port VLAN 10                        |
|                             |                             | Eth1/6 (access VLAN 20)         | `192.168.20.x/24`   | `192.168.20.1` | *192.168.20.0* | *192.168.20.255* | Access port VLAN 20                        |
|                             |                             | Eth1/7 (access VLAN 30)         | `192.168.30.x/24`   | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Access port VLAN 30                        |
| **RT-1-EDGE**<br>(IOS)      | Edge Router (WAN/MPLS/Core) | Ethernet0/0 (WAN-1)             | `123.1.1.2/30`      | –              | *123.1.1.0*    | *123.1.1.3*      | NAT outside → 0.0.0.0/0 via `123.1.1.1`    |
|                             |                             | Ethernet0/1 (P2P OSPF to NX9-1) | `10.10.0.1/30`      | –              | *10.10.0.0*    | *10.10.0.3*      | OSPF cost 1                                |
|                             |                             | Ethernet0/2 (P2P OSPF to NX9-2) | `10.30.0.1/30`      | –              | *10.30.0.0*    | *10.30.0.3*      | OSPF cost 100                              |
|                             |                             | Ethernet0/3 (P2P OSPF to RT-2)  | `10.60.0.1/30`      | –              | *10.60.0.0*    | *10.60.0.3*      | OSPF cost 200 (Edge-to-Edge)               |
|                             |                             | Ethernet1/0 (MPLS-1)            | `10.100.0.2/30`     | –              | *10.100.0.0*   | *10.100.0.3*     | Connected to MPLS-1                        |
|                             |                             | OSPF process                    | router-id `3.3.3.3` | –              | –              | –                | Redistributes static routes (MPLS)         |
| **RT-2-EDGE**<br>(IOS)      | Edge Router (WAN/MPLS/Core) | Ethernet0/0 (WAN-2)             | `123.2.2.2/30`      | –              | *123.2.2.0*    | *123.2.2.3*      | NAT outside → 0.0.0.0/0 via `123.2.2.1`    |
|                             |                             | Ethernet0/1 (P2P OSPF to NX9-2) | `10.20.0.1/30`      | –              | *10.20.0.0*    | *10.20.0.3*      | OSPF cost 1                                |
|                             |                             | Ethernet0/2 (P2P OSPF to NX9-1) | `10.40.0.2/30`      | –              | *10.40.0.0*    | *10.40.0.3*      | OSPF cost 100                              |
|                             |                             | Ethernet0/3 (P2P OSPF to RT-1)  | `10.60.0.2/30`      | –              | *10.60.0.0*    | *10.60.0.3*      | OSPF cost 200 (Edge-to-Edge)               |
|                             |                             | Ethernet1/0 (MPLS-2)            | `10.100.0.6/30`     | –              | *10.100.0.4*   | *10.100.0.7*     | Connected to MPLS-2                        |
|                             |                             | OSPF process                    | router-id `4.4.4.4` | –              | –              | –                | Redistributes static routes (MPLS)         |
| **WAN-1 (IOS)**<br>(IOS)    | Simulated WAN Router        | Ethernet0/0                     | `123.1.1.1/30`      | –              | *123.1.1.0*    | *123.1.1.3*      | Loopbacks 8.8.8.8/32, 8.8.4.4/32           |
| **WAN-2 (IOS)**<br>(IOS)    | Simulated WAN Router        | Ethernet0/0                     | `123.2.2.1/30`      | –              | *123.2.2.0*    | *123.2.2.3*      | Loopbacks 8.8.8.8/32, 8.8.4.4/32           |
| **MPLS-1 (IOS)**<br>(IOS)   | Simulated MPLS Router       | Ethernet0/0                     | `10.100.0.1/30`     | –              | *10.100.0.0*   | *10.100.0.3*     | Loopbacks 10.200.0.100/32, 10.210.0.100/32 |
| **MPLS-2 (IOS)**<br>(IOS)   | Simulated MPLS Router       | Ethernet0/0                     | `10.100.0.5/30`     | –              | *10.100.0.4*   | *10.100.0.7*     | Loopbacks 10.200.0.100/32, 10.210.0.100/32 |
| **Server-1**<br>(Linux)     | Host VLAN 10                | Ethernet0/0                     | `192.168.10.101/24` | `192.168.10.1` | *192.168.10.0* | *192.168.10.255* | Default-route to HSRP VIP Core             |
| **Server-2**<br>(Linux)     | Host VLAN 20                | Ethernet0/0                     | `192.168.20.101/24` | `192.168.20.1` | *192.168.20.0* | *192.168.20.255* | Default-route to HSRP VIP Core             |
| **Server-3**<br>(Linux)     | Host VLAN 30                | Ethernet0/0                     | `192.168.30.101/24` | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Default-route to HSRP VIP Core             |
| **Server-4**<br>(Linux)     | Host VLAN 10                | Ethernet0/0                     | `192.168.10.102/24` | `192.168.10.1` | *192.168.10.0* | *192.168.10.255* | Default-route to HSRP VIP Core             |
| **Server-5**<br>(Linux)     | Host VLAN 20                | Ethernet0/0                     | `192.168.20.102/24` | `192.168.20.1` | *192.168.20.0* | *192.168.20.255* | Default-route to HSRP VIP Core             |
| **Server-6**<br>(Linux)     | Host VLAN 30                | Ethernet0/0                     | `192.168.30.102/24` | `192.168.30.1` | *192.168.30.0* | *192.168.30.255* | Default-route to HSRP VIP Core             |


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













































# Nexus NX-OS: Help Miscellaneous Commands

This section provides useful commands for:

- Viewing general configurations
- File management
- Directory navigation
- Basic utility operations in NX-OS.

````py
!#############################################
!# HELP & MISCELLANEOUS COMMANDS (NX-OS CLI) #
!#############################################

! ### BASIC SHOW

!# - Basic “show” commands for quick system insight and troubleshooting.

!# Display the entire running configuration
show running

!# Display a summary of all interfaces and their current status (up/down, VLAN, etc.)
show interface status

!# List all currently connected users and the VTY sessions they are using
show users

!# Show system resource usage (CPU, memory, etc.)
show system resources

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### WHERE & LOCATION

!# - Commands to identify your current CLI context or mode.

!# Display the current CLI context (which mode you are in)
where

!# Show detailed information about the CLI context (more verbose)
where detail

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### VIEW CONFIGS

!# - Commands to compare configurations or view specific sections.

!# Show current running configuration
show running-config

!# Compare running-config to startup-config and highlight differences
show diff rollback-patch running-config startup-config

!# Show only the VLAN section of the running config
show running-config vlan

!# Show only the interface configuration for Ethernet1/1
show running-config interface Ethernet1/1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### PUSH / POP

!# - Push creates a temporary CLI checkpoint; Pop reverts to it if needed.

!# Create a push (CLI volatile checkpoint) so you can revert to it if needed
push

!# Return to the most recent push checkpoint
pop

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### DIRECTORY (DIR)

!# - Commands to inspect filesystems and their contents.

!# Show all file systems mounted on the device
show file system

!# List contents of the primary flash filesystem
dir bootflash:

!# List contents of the “system” filesystem (where NX-OS images reside)
dir system:

!# List contents of a USB flash (if present)
dir usb0:

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### NAVIGATION (PWD, CD, MKDIR, RMDIR)

!# - Commands to navigate and manipulate directories.

!# Print working directory (shows current path in NX-OS file hierarchy)
pwd

!# Create a directory named “tech-support” in the current filesystem
mkdir tech-support

!# Change into the “tech-support” directory
cd tech-support

!# Move up one directory level
cd ..

!# Remove an empty directory named “old-backups”
rmdir old-backups

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### FILE INSPECTION (MORE, TYPE)

!# - Commands to view file contents or validate integrity.

!# Display the contents of “backup-1” one page at a time
more backup-1

!# Display the first 10 lines of “backup-1”
more bootflash:/backup-1 | head

!# Verify file integrity or view a file’s content in plain text
show file bootflash:/nxos.9.2.3.bin

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### FILE MANAGEMENT

!# - Commands to delete, rename, or manage files on NX-OS filesystems.

!# Delete a file named “old-backup” from bootflash
delete bootflash:/old-backup

!# Rename (“move”) a file from bootflash to a new name
rename bootflash:/backup-1 bootflash:/backup-1-old

!# Remove all files matching a pattern (wildcard)
delete bootflash:/*.cfg

!# Verify remaining free and used space on bootflash
dir bootflash: | include bytes

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

!# TECH-SUPPORT 

!# - Use tech-support commands to collect diagnostics for TAC or internal use.

!# Generate a full tech-support bundle (collects logs, configs, diagnostics)
show tech-support

!# Generate tech-support and save it to “tech-support-<date>”
show tech-support > tech-support-$(date +%Y%m%d)

!# View the directory to ensure the tech-support file is created
dir bootflash: | include tech-support

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### SAVE CONFIGURATION, FILE COPY & TRANSFER

!# - Common copy commands for configurations and IOS/NX-OS images.

!# Copy running-config to startup-config
copy running-config startup-config

!# Copy running-config to a remote TFTP server (replace <tftp-ip> and <filename>)
copy running-config tftp://<tftp-ip>/nxos-running.backup

!# Copy startup-config from TFTP to running-config
copy tftp://<tftp-ip>/nxos-startup.backup running-config

!# Copy NX-OS image from USB to bootflash (replace <image.bin>)
copy usb0:/nxos.9.3.3.bin bootflash:/nxos.9.3.3.bin

!# Copy file from bootflash to USB
copy bootflash:/backup-1 usb0:/backup-1

!# Secure copy (SCP) running-config to a remote server (replace <user@host:path>)
copy running-config scp://<user>@<host>/configs/nxos-running.cfg

!# Download a file via FTP (requires an FTP server)
copy ftp://<user>@<host>/nxos.10.0.1.bin bootflash:/nxos.10.0.1.bin

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

! ### CHECKPOINT & ROLLBACK 

!# - Checkpoints allow you to capture the running configuration at a point in time.
!# - You can roll back to a saved checkpoint if needed—useful for testing changes safely.

!# Create a named checkpoint of the running configuration
checkpoint fz3r0-check-04212025

!# Roll back to a named checkpoint
rollback running-config checkpoint fz3r0-check-04212025

!# List all saved checkpoints
show checkpoint

!# List checkpoint names in the output
show checkpoint | include name

!# Show a summary of all checkpoints and their metadata
show checkpoint summary

!# Show the contents of a specific checkpoint (view its config)
show checkpoint fz3r0-check-04212025


````








# Nexus: Common Configs & Commands: 

## Features

In NX-OS, many protocol modules and services are packaged as optional “features” that can be enabled or disabled. This modular approach allows you to:

- **Save resources:** Only load the protocols/services you need.
- **Meet licensing requirements:** Some advanced features (e.g., BGP, QoS modules, security services) require a valid license.
- **Improve security & troubleshooting:** Disabling unused features reduces attack surface and debug noise.

````py
!############
!# FEATURES #
!############

!# List all available features (e.g., port-channel, ospf, hsrp, interface-vlan, lacp, ssh, telnet, etc.)
feature ?

!# Enable the Interface‐VLAN feature (required for SVIs on some NX-OS versions)
feature interface-vlan

!# Enable demo/trial licenses (usually 120 days) so you can test features
license grace-period

!# Disable an unwanted feature (e.g., SSH)
no feature ssh

!# Show current feature status (enabled vs. disabled)
show feature

!# Show license token usage
show license usage

!# Show overall license status
show license status
````

## Device Naming, Users & Roles

In Cisco NX-OS, configuring the hostname and user accounts/roles is very similar to IOS.
The commands differ slightly for roles and password strength, but the overall workflow is the same.

````py
!###########
!# NAMINGS #
!###########

!# Set the device hostname
hostname NXv9K-1

!#################
!# USERS & ROLES #
!#################

!# Create or update a local user with a password
username fz3r0 password Cisco.12345

!# Enable password strength enforcement (uppercase, lowercase, numbers, symbols, min length)
password strength-check
!# Disable password strength enforcement
no password strength-check

!# Display available built-in roles
username fz3r0 role ?

!# Assign full-privilege role (network-admin = priv-15)
username fz3r0 role network-admin

!# Create a custom role with specific command permissions
role name Fz3r0-Custom-Role
   rule 1 permit command show interface status
   rule 2 permit command show interface description
   rule 3 deny command show interface
   rule 4 deny command reload
   rule 5 deny command write memory

!# Show the rules for a specified role
show role name network-admin
show role name Fz3r0-Custom-Role
````

## Telnet & SSH

! Enabling Telnet and SSH on NX-OS is almost identical to IOS.
! You need to load the feature modules, assign an IP to a management port, and optionally lock down VTY access.

````py
!################
!# TELNET & SSH #
!################

!# 1. Enable demo licenses (120-day trial) and activate Telnet & SSH features
license grace-period
feature telnet
feature ssh

!# 2. Configure an admin interface for remote access (e.g., Ethernet2/1)
interface ethernet 2/1
  no shutdown
  ip address 192.168.10.1/24
exit

!# 3. (Optional) Restrict VTY sessions: max 5 sessions, auto-logout after 3 minutes
line vty
  session-limit 5
  exec-timeout 3
exit

!# 4. (Optional) Create an ACL to allow only specific remote IPs to reach VTY
ip access-list remote-access-users
  permit ip host 192.168.10.100 any
exit
!# Bind the ACL to VTY lines
line vty
  access-class remote-access-users in
exit
````


## Interface Configuration

In NX-OS, interface configuration is similar to IOS but with a few Nexus-specific nuances (e.g., default L3 mode, feature enablement).

### Interface Selection

- In NX-OS you can also use wildcards and port‐channel selectors to match multiple interfaces.
- Any syntax that matches the interface name pattern is accepted, similar to IOS wildcard usage.

````py
!#############################################
!# INTERFACE RANGE SELECTION                #
!#############################################

!# Single interface:
interface ethernet 1/1

!# Range of sequential interfaces:
interface ethernet 1/1-4

!# Non-sequential list:
interface ethernet 1/1,1/6,1/8

!# Wildcard for an entire slot (all ports on module 1):
interface ethernet 1/*

!# Mix of ranges and single ports:
interface ethernet 1/[1-2],1/[5-6],1/8

!# Port‐channel interfaces (all Po interfaces):
interface port-channel *

!# Specific subset of port‐channels:
interface port-channel 1-3

!# You can combine VLAN SVIs too:
interface vlan 10-12
````

### INTERFACES L2, L3 / IP & ADDRESSING 

- On Nexus, interfaces default to Layer-3 mode unless “system default switchport” is active.
- To use interface as traditional Layer-2 interface (as IOS/Catalyst) you will need to run "switchport" command

````py
!########################
!# INTERFACES: L3 & L2  #
!########################

!# To explicitly set an interface to L2 (enable switchport / traditional Catalyst layer 2):
interface ethernet 1/1
  switchport
exit

!# To explicitly set an interface to L3 (disable switchport):
interface ethernet 1/1
  no switchport
exit
````

### L3 Interfaces: IP & Addressing

In NX-OS, IP & Addressing configuration setup closely mirror IOS

````py
!################################
!# INTERFACES: IP & ADDRESSING  #
!################################

!# Example: Assign a WAN IP on a pure L3 port
interface ethernet 1/1
  no shutdown
  no switchport
  description ** WAN-L3-INTERFACE **
  ip address 123.123.123.2/30
exit

!# After assigning IP, you can add a default route:
ip route 0.0.0.0/0 123.123.123.1
````

### L2 Interfaces: VLANs, Trunks & Access Ports

In NX-OS, VLAN creation, trunk configuration, and access port setup closely mirror IOS, with a few Nexus‐specific commands (e.g., “vlan dot1q tag native”).

````py
!#############################################
!# VLANs, TRUNKS & ACCESS PORTS             #
!#############################################

! # vlan & trunk show important commands
show vlan
show interface status
show run interface ethernet 2/1
show interface trunk
! # Show VLANs reserved for internal uses (Cisco reserved VLANs)
show vlan interal usage
show system vlan reserved

!# 1. Ensure VLANs exist or create new VLANs
vlan 10
  name VLAN10-BLUE
vlan 20
  name VLAN10-RED
vlan 30
  name VLAN10-GREEN-MANAGEMENT
vlan 99
  name VLAN99-TRUNK-NATIVE
  vlan dot1q tag native
exit

!# 2. Configure a trunk interface (for VLANs 10,20,30; native VLAN 99)
interface ethernet 1/4
  no shutdown
  description ** L2-TRUNK-NATIVE99 **
  switchport
  switchport mode trunk
  switchport trunk native vlan 99
  switchport trunk allowed vlan 10,20,30
  speed 1000
  duplex full
  spanning-tree port type network   !# R-PVSTP: mark as network port
  cdp enable
exit

!# 3. Configure an access port in VLAN 10 (edge port settings)
interface ethernet 1/5
  no shutdown
  description ** L2-ACCESS-VLAN10-BLUE **
  switchport
  switchport mode access
  switchport access vlan 10
  speed 1000
  duplex full
  spanning-tree port type edge      !# R-PVSTP: mark as edge port (PortFast equivalent)
  spanning-tree bpduguard enable    !# Block if BPDU received
  cdp enable
exit

!# (Optional) Limit MAC Address table learning (e.g. 199 MACs max)
mac address-table limit vlan 20 199


````

### Speed & Duplex

In NX-OS, interfaces default to `auto-negotiate` both speed and duplex.  

- In some scenarios (e.g., connecting to older devices or troubleshooting link issues), you may need to manually set speed and duplex.

````py
!#######################
!# SPEED & DUPLEX #
!#######################

! # Set full duplex and 1 Giga speed on interface
interface ethernet 1/1
   duplex full
   speed 1000
exit

! To revert to default  an interface back to auto-negotiation for both speed and duplex:
interface ethernet 1/1
   duplex auto     
   speed auto      
exit
````

### Set Default Settings on Interfaces

````py
!##########################################
!# SET DEFAULT SETTINGS ON INTERFACES
!##########################################

!# Set all interfaces to Layer 2 (On Nexus all interfaces are L3 by default)
system default switchport

!# Set default settings for "X" interface
default interface ethernet 1/1

!# Set default settings for a range from "X" to "Z" interfaces
default interface ethernet 1/1-3
````





## Port Profile

- Port Profiles in NX-OS allow you to group common interface settings into a reusable template. This simplifies configuration, ensures consistency, and speeds deployment. Instead of typing the same commands on every interface, you attach a port profile to one or many interfaces. If you need to update settings network-wide, you only modify the port profile.

````py
!################
!# PORT PROFILE #
!################

!# 1. List available port profile types (ethernet, interface-vlan, port-channel, etc.)
port-profile type ?

!# 2. Create a basic Ethernet port profile for an access port in VLAN 10
port-profile type ethernet fz3r0-interfaces-access-10
   !# Human-friendly description
   description ACCESS-VLAN10-BLUE
   !# Ensure interface is active
   no shutdown
   !# Put interface into L2 access mode
   switchport mode access
   !# Assign VLAN 10
   switchport access vlan 10
   !# Force full-duplex
   duplex full
   !# Enable this port profile
   state enabled
exit

!# 3. Create a second Ethernet port profile that inherits the first, adding 100 Mbps speed
port-profile type ethernet fz3r0-interfaces-access-10-AND-speed100
   !# Reuse all settings from fz3r0-interfaces-access-10
   inherit port-profile fz3r0-interfaces-access-10
   !# Override or add settings: set speed to 100 Mbps
   speed 100
   !# Describe the purpose
   description ACCESS-VLAN10-BLUE-SPEED100
   !# Enable this port profile
   state enabled
exit

!# 4. Apply the fz3r0-interfaces-access-10 profile to a range of interfaces
interface ethernet 1/1-2
   !# Attach the port profile, which pushes all its commands to these interfaces
   inherit port-profile fz3r0-interfaces-access-10
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

!# Helpful show commands:

!# Show all configured port profiles
show port-profile summary

!# Show details of a specific port profile
show port-profile type ethernet fz3r0-interfaces-access-10

!# Verify which profile is applied to a given interface
show interface ethernet 1/1 port-profile

!# Show running configuration for port profiles
show running-config port-profile
````




## Out-Of-Band (OOB) Management Interface

In NX-OS, a dedicated Out-Of-Band (OOB) management interface separates device management traffic from production data. This ensures administrative access remains available even if data-plane interfaces are down. By placing this interface in its own “management” VRF, you isolate management routing, improve security, and simplify policy control.

````py
!##########################################
!# Out-Of-Band (OOB) Management Interface #
!##########################################

!# 1. Configure the Management Interface (usually mgmt0)
interface management 0
  no shutdown
  description ** OOB Management Interface **
  ip address 192.168.100.100/24
exit

!# 2. Create or enter the 'management' VRF context
vrf context management

!# 3. Add a default route in the management VRF to reach the OOB gateway
ip route 0.0.0.0/0 192.168.100.1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

!# Useful show commands for Management VRF and VRFs in general:

!# Show all VRFs configured on the device
show vrf

!# Show interfaces assigned to the 'management' VRF
show vrf management interface

!# Show running configuration for the 'management' VRF only
show running-config vrf management

!# Show the IP routing table for the 'management' VRF
show ip route vrf management

!# Verify the management interface status and IP
show interface management 0

!# Check process reachability to OOB gateway from management VRF
ping vrf management 192.168.100.1
````

## SVI (Switch VLAN Interface)

In NX-OS, SVIs (Switch Virtual Interfaces) act as Layer-3 gateways for VLANs—similar to “interface vlan X” on IOS, but you must ensure VLAN exists and the interface-vlan feature is enabled.

````py
!#############################################
!# SVI + CONFIGURE SWITCH AS GATEWAY         #
!#############################################

!# 1. Create VLANs (needed before creating SVIs)
vlan 10
  name VLAN10-BLUE
vlan 20
  name VLAN10-RED
vlan 30
  name VLAN10-GREEN-MANAGEMENT
exit

!# 2. Enable the interface-vlan feature (required for SVIs on some NX-OS versions)
feature interface-vlan

!# 3. Create SVIs (logical L3 interfaces for each VLAN)
interface vlan 10
  no shutdown
  description ** SVI + GW L3 VLAN10-BLUE **
  ip address 192.168.10.254/24
exit

interface vlan 20
  no shutdown
  description ** SVI + GW L3 VLAN20-RED **
  ip address 192.168.20.254/24
exit

interface vlan 30
  no shutdown
  description ** SVI + GW L3 VLAN30-GREEN **
  ip address 192.168.30.254/24
exit

!# 4. (Optional) If you also need a dedicated WAN L3 port on the same switch, configure like above:
interface ethernet 1/1
  no shutdown
  no switchport
  description ** WAN-L3-INTERFACE **
  ip address 123.1.1.2/30
exit

!# 5. Create a default route pointing to your ISP/WAN gateway:
ip route 0.0.0.0/0 123.1.1.1
````

## Port Channel (LACP)

Port-channels (etherchannel) in NX-OS bundle multiple physical interfaces into a single logical link (very similar to IOS). This provides:

 - **Increased bandwidth**: Combined throughput of member links.  
 - **Redundancy**: If one member fails, traffic continues on the others.  
 - **Simplified configuration**: Treat the bundle as one interface for spanning-tree, OSPF, etc.

Port-channels can be used as both: L2 & L3 interfaces.

````py
!###########################
!# PORT CHANNEL (LACP)     #
!###########################

!# 1. Enable LACP feature (required for channel-group commands)
feature lacp

!# 2. (Optional) Reset interfaces to default before bundling
default interface ethernet 1/1-2
default interface ethernet 1/3

!# 3. Create an L2 port-channel on interfaces Ethernet1/1 and Ethernet1/2
!#    - Turn on each interface  
!#    - Configure as switchport (Layer-2)  
!#    - Assign to channel-group 1 in active LACP mode  
interface ethernet 1/1
   no shutdown
   switchport
   channel-group 1 mode active
exit

interface ethernet 1/2
   no shutdown
   switchport
   channel-group 1 mode active
exit

!# 4. Configure the logical Port-Channel1 as a trunk (L2)
interface port-channel 1
   description L2-PORT-CHANNEL-TRUNK-TO-DIST  
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk native vlan 99
   switchport trunk allowed vlan 10,20,30,99
exit

!# 5. Create an L3 port-channel on interfaces Ethernet1/1 and Ethernet1/2
!#    - Turn on each interface  
!#    - Remove switchport (Layer-3)  
!#    - Assign to channel-group 2 in active LACP mode  
interface ethernet 1/1
   no shutdown
   no switchport
   channel-group 2 mode active
exit

interface ethernet 1/2
   no shutdown
   no switchport
   channel-group 2 mode active
exit

!# 6. Configure the logical Port-Channel2 as a routed interface (L3)
interface port-channel 2
   description L3-PORT-CHANNEL-UPLINK  
   no shutdown
   no switchport
   ip address 10.10.0.1/30
exit

!# 7. (Optional) Add a third interface as a standby link in the existing port-channel
!#    - Use “force” to include it without negotiation  
!#    - Adjust LACP bundle parameters  
interface ethernet 1/3
   no shutdown
   switchport     !# or “no switchport” if L3  
   channel-group 1 force mode active
exit

interface port-channel 1
   lacp max-bundle 2   !# Only 2 active members  
   lacp min-links 2    !# Bring down bundle if fewer than 2 remain  
exit

!# 8. Assign a higher LACP priority to make Ethernet1/3 the standby (lower priority = more preferred)
interface ethernet 1/3
   lacp port-priority 39000
exit

!# 9. Select load-balance algorithm for traffic distribution  
!#    - “src-dst” (source/dest MAC) is a common choice  
port-channel load-balance src-dst

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!# Useful “show” commands for troubleshooting:

!# Show port-channel status and member interfaces  
show port-channel summary

!# Show detailed LACP information for a specific bundle  
show port-channel database

!# Verify Port-Channel configuration and counters  
show interface port-channel 1

!# Check individual member interface status  
show interface ethernet 1/1 status
show interface ethernet 1/2 status

!# View load-balance algorithm in use  
show port-channel load-balance
````

## HSRP (Hot Standby Router Protocol)

In NX-OS, HSRP (Hot Standby Router Protocol) provides a virtual gateway IP for hosts, ensuring continuous Layer-3 access if one device fails. Both active and standby routers share a virtual IP; the higher-priority device becomes active, and the other takes over if it stops responding.  

````py
!##################################
!# HSRP CONFIGURATION EXAMPLE     #
!##################################

!# Assume two core switches with SVI IPs:
!#   Core1 SVI  = 192.168.X.254 (higher priority → ACTIVE)
!#   Core2 SVI  = 192.168.X.253 (lower priority → STANDBY)
!# Virtual IP for each VLAN = 192.168.X.1

!# 1. Create VLANs (if not already present)
vlan 10
  name VLAN10-BLUE
vlan 20
  name VLAN10-RED
vlan 30
  name VLAN10-GREEN-MANAGEMENT
exit

!# 2. Enable SVI feature (required on some NX-OS versions)
feature interface-vlan

!# 3. Create SVIs on Core1 (active)
interface vlan 10
  no shutdown
  description ** SVI + GW VLAN10-BLUE (Core1) **
  ip address 192.168.10.254/24
exit

interface vlan 20
  no shutdown
  description ** SVI + GW VLAN20-RED (Core1) **
  ip address 192.168.20.254/24
exit

interface vlan 30
  no shutdown
  description ** SVI + GW VLAN30-GREEN (Core1) **
  ip address 192.168.30.254/24
exit

!# 4. (Optional) Configure a dedicated WAN interface on Core1
interface ethernet 1/1
  no shutdown
  no switchport
  description ** WAN-L3-INTERFACE (Core1) **
  ip address 123.1.1.2/30
exit

!# 5. Create default route on Core1
ip route 0.0.0.0/0 123.1.1.1

!# 6. Enable HSRP feature
feature hsrp

!# 7. Configure HSRP groups on Core1 (higher priority = active)
interface vlan 10
  hsrp version 2
  hsrp 10
  ip 192.168.10.1
  preempt
  priority 200
exit

interface vlan 20
  hsrp version 2
  hsrp 20
  ip 192.168.20.1
  preempt
  priority 200
exit

interface vlan 30
  hsrp version 2
  hsrp 30
  ip 192.168.30.1
  preempt
  priority 200
exit

!# --------------------------------------------

!# Repeat on Core2 (standby) with lower priority

!# 1. Create VLANs (if not already present)
vlan 10
  name VLAN10-BLUE
vlan 20
  name VLAN10-RED
vlan 30
  name VLAN10-GREEN-MANAGEMENT
exit

!# 2. Enable SVI feature (if needed)
feature interface-vlan

!# 3. Create SVIs on Core2 (standby)
interface vlan 10
  no shutdown
  description ** SVI + GW VLAN10-BLUE (Core2) **
  ip address 192.168.10.253/24
exit

interface vlan 20
  no shutdown
  description ** SVI + GW VLAN20-RED (Core2) **
  ip address 192.168.20.253/24
exit

interface vlan 30
  no shutdown
  description ** SVI + GW VLAN30-GREEN (Core2) **
  ip address 192.168.30.253/24
exit

!# 4. (Optional) Configure a dedicated WAN interface on Core2
interface ethernet 1/1
  no shutdown
  no switchport
  description ** WAN-L3-INTERFACE (Core2) **
  ip address 123.2.2.2/30
exit

!# 5. Create default route on Core2
ip route 0.0.0.0/0 123.2.2.1

!# 6. Enable HSRP feature
feature hsrp

!# 7. Configure HSRP groups on Core2 (lower priority = standby)
interface vlan 10
  hsrp version 2
  hsrp 10
  ip 192.168.10.1
  preempt
  priority 100
exit

interface vlan 20
  hsrp version 2
  hsrp 20
  ip 192.168.20.1
  preempt
  priority 100
exit

interface vlan 30
  hsrp version 2
  hsrp 30
  ip 192.168.30.1
  preempt
  priority 100
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

!# Useful show commands for troubleshooting HSRP

!#  - Verify HSRP state and VIP assignments
show hsrp brief

!#  - Show detailed HSRP statistics per interface
show hsrp interface vlan 10

!#  - Verify HSRP timers and priority
show hsrp detail

!#  - Confirm active/standby roles
show hsrp status

!#  - Check underlying VLAN and SVI status
show interface vlan 10
show ip interface brief

````


## Spanning Tree (R-PVSTP) 

- Rapid PVST+ (R-PVSTP) is Cisco’s rapid version of Per-VLAN Spanning Tree.
- It provides fast convergence and loop prevention on VLAN-based networks.

````py
!###########################
!# SPANNING TREE (R-PVSTP) #
!###########################

! 1. Enable R-PVSTP (usually on by default in NX-OS)  
spanning-tree mode rapid-pvst

! 2. Designate this switch as the primary root for VLANs 10, 20, 30  
!    - “root primary” raises priority to be the root bridge  
spanning-tree vlan 10,20,30 root primary

! 3. (On the secondary switch) Designate as secondary root  
!    - “root secondary” sets a priority just below the primary’s  
spanning-tree vlan 10,20,30 root secondary

! 4. Mark an access (edge) port to bypass listening/learning  
!    - Use on host-facing ports for instant transition (PortFast equivalent)  
interface ethernet 1/3
   switchport
   switchport mode access
   spanning-tree port type edge
exit

! 5. Mark a trunk (network) port to enable bridge assurance & BPDU forwarding  
!    - Use on links between switches to detect unidirectional failures  
interface ethernet 1/5
   switchport
   switchport mode trunk
   spanning-tree port type network
exit

! 6. Enable BPDU Guard on an edge port to shut it down if a BPDU is received  
!    - Protects against rogue switches/compliance issues on access ports  
interface ethernet 1/1
   switchport
   switchport mode access
   spanning-tree port type edge
   spanning-tree bpduguard enable
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

!# Helpful “show” commands for troubleshooting R-PVSTP:

!# Show root bridge status and priority  
show spanning-tree vlan 10

!# Display per-VLAN spanning-tree summary  
show spanning-tree summary

!# View port roles and states for VLAN 10  
show spanning-tree vlan 10 detail

!# Check BPDU Guard status on all interfaces  
show spanning-tree summary | include BPDU Guard

!# View global R-PVSTP configuration  
show running-config | include spanning-tree
````




## OSPF 

- By enabling OSPF, you advertise network prefixes (SVIs, WAN links, etc.) so that all participating routers build a consistent topology. 
- In this lab, OSPF is used to dynamically advertise networks and build a loop-free topology at core level

````py
!# ##########################################
!# OSPF
!# ##########################################

!# Enable OSPF Feature
feature ospf

! ### INTERFACE CONFIGURATION EXAMPLES WITH OSPF

!# OSPF BEST PREFERENCE (cost 1) on RT1 → WAN-1 link
interface ethernet 1/1
   no shutdown
   no switchport
   ip address 10.10.0.2/30
   ip router ospf 1 area 0               !# Enable OSPF on this interface, assign to Area 0
   ip ospf network point-to-point        !# Optimize OSPF for a point-to-point link
   ip ospf cost 1                        !# Set cost to 1 (best preference)
   ip ospf hello-interval 1              !# Reduce hello interval to 1s for fast adjacency
   ip ospf dead-interval 4               !# Dead interval = 4s (faster failure detection)
   cdp enable                            !# Enable CDP for neighbor discovery (optional)
exit

!# OSPF WORST PREFERENCE (cost 100) on RT1 → RT2 link (backup path)
interface ethernet 1/2
   no shutdown
   no switchport
   ip address 10.40.0.1/30
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 100                       !# Higher cost → lower priority path
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable
exit

!# OSPF MEDIUM PREFERENCE (cost 100) on NX9-2 → NX9-1 port-channel (backup core link)
interface port-channel 1
   no shutdown
   no switchport
   ip address 10.50.0.1/30
   ip router ospf 1 area 0
   ip ospf network point-to-point
   ip ospf cost 500                       !# Highest cost → lowest priority path (worst)
   ip ospf hello-interval 1
   ip ospf dead-interval 4
   cdp enable
exit

! ### CONFIGURE OSPF PROCESS IN ROUTER/NEXUS

router ospf 1
   !# Advertise VLAN10 (192.168.10.0/24) into Area 0
   network 192.168.10.0/24 area 0
   !# Advertise VLAN20 (192.168.20.0/24) into Area 0
   network 192.168.20.0/24 area 0
   !# Advertise VLAN30 (192.168.30.0/24) into Area 0
   network 192.168.30.0/24 area 0
   !# Advertise the WAN-1 link (123.1.1.0/30) into Area 0
   network 123.1.1.0/30 area 0
exit

!# Configure Default Route to ISP/WAN
!# In NX-OS, a static default route must exist before redistributing it into OSPF.
ip route 0.0.0.0/0 123.1.1.1

!# Optional: Redistribute Default into OSPF
!# To advertise the default route as an OSPF external LSA, under “router ospf 1” add:
default-information originate

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! ### USEFUL OSPF SHOW COMMANDS FOR TROUBLESHOOTING

!# OSPF Neighbor Information
show ip ospf neighbor

!# OSPF Interface Summary
show ip ospf interface brief

!# OSPF Interface Details
show ip ospf interface Ethernet0/1

!# OSPF Process Status
show ip ospf

!# OSPF Event Logs
show ip ospf events

!# OSPF Statistics
show ip ospf statistics

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! ### OSPF LINK-STATE DATABASE (LSDB) INSPECTION

!# Full OSPF Database hows the entire LSDB (all LSA types) known to this router.
show ip ospf database

!# Type-1 (Router) LSAs - Lists LSAs describing directly connected OSPF routers.
show ip ospf database router

!# Type-2 (Network) LSAs - Lists LSAs representing multi-access networks (DR/BDR). Not used on point-to-point links.
show ip ospf database network

!# Type-3 (Summary) LSAs - Lists inter-area summary LSAs (routes injected from other OSPF areas).
show ip ospf database summary

!# Type-5 (External) LSAs - Lists LSAs for routes redistributed from other protocols (e.g., the default route).
show ip ospf database external

!# LSAs Originated by This Router - Displays LSAs that this router has generated (Router, Network, Summary, External).
show ip ospf database self-originate

!# LSAs for a Specific Router ID - Shows all LSAs advertised by the router with ID 1.1.1.1.
show ip ospf database router 1.1.1.1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! ### OSPF ROUTING TABLE

!# OSPF-Learned Routes Only - Filters the routing table to show only prefixes learned via OSPF.
show ip route ospf

!# Full Routing Table with OSPF Prefixes Marked “O” - Displays all routes; OSPF routes are prefixed with “O”.
show ip route

!# Path to Default Route - Shows how the default route is resolved in the routing table.
show ip route 0.0.0.0

!# Path to a Specific IP - Displays the best path towards 192.168.20.1 and its next‐hop information.
show ip route 192.168.20.1

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! ### DEBUG COMMANDS (LAB USE ONLY)

!# Trace OSPF Adjacency Changes - Logs OSPF neighbor adjacency state changes in real-time (use in lab only).
debug ip ospf adj

!# Trace All OSPF Events - Logs OSPF timers, LSA generation, and SPF recalculations to console.
debug ip ospf events

!# Trace SPF Recalculation - Shows detailed SPF recalculation activity; useful when a link or neighbor goes down.
debug ip ospf spf

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! ### NX-OS–SPECIFIC OSPF COMMANDS

!# --- Show OSPF RIB (Internal RIB) on Nexus - Displays routes installed in the OSPF RIB on NX-OS.
show ip ospf rib

!# --- Show Historical OSPF Events - (Same as above, but especially helpful on NX-OS to review past events.)
show ip ospf events

!# --- Brief OSPF Interface Status on NX-OS - Lists all interfaces with their OSPF cost and adjacency state (Nexus-specific formatting).
show ip ospf interface brief

````

## Discovery Protocols (CDP & LLDP)

In NX-OS, CDP and LLDP are used to discover directly connected Cisco and non-Cisco devices.

- CDP is on by default in NX-OS, but you can enable/disable it globally or per-interface.
- LLDP must be explicitly enabled as a feature before use.

````py
!#########################
!# DISCOVERY PROTOCOLS   #
!#########################

! ### CDP 

! Enable CDP globally (exec-level command):
cdp enable

! Disable CDP globally if not needed:
no cdp enable

! Enable CDP on a specific interface:
interface ethernet 1/1
   cdp enable        ! Allows this interface to send/receive CDP PDUs
exit

! Disable CDP on a specific interface:
interface ethernet 1/1
   no cdp enable     ! Prevents CDP on this interface
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! Useful CDP show commands:

! show cdp neighbors         ! Displays immediate neighbor devices
! show cdp neighbors detail  ! Detailed information about neighbors
! show cdp interface         ! Which interfaces have CDP enabled

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! ### LLDP

! LLDP must be turned on as a feature:
feature lldp

! Disable LLDP feature if not needed:
no feature lldp

! Enable LLDP globally (once enabled as a feature, LLDP is active):
! By default, LLDP runs on all supported interfaces after "feature lldp".

! To disable LLDP per interface:
interface ethernet 1/1
   no lldp transmit   ! Stop sending LLDP frames on this interface
   no lldp receive    ! Stop processing LLDP frames on this interface
exit

! To enable LLDP per interface (if it was disabled earlier):
interface ethernet 1/1
   lldp transmit      ! Allow sending LLDP frames
   lldp receive       ! Allow receiving LLDP frames
exit

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! Useful LLDP show commands:

! show lldp neighbors         ! Displays LLDP neighbor summary
! show lldp neighbors detail  ! Detailed LLDP neighbor information
! show lldp interface         ! LLDP status per interface
````



## NAT (PAT)

- NON AVAILABLE IN VIRTUAL CURRENT VERSION, THIS EXAMPLE SHOW IOS/CATALYST NAT
- NAT overload (PAT) translates multiple internal IPs to a single public IP for Internet access.

````py
!# ##########################################
!# NAT
!# ##########################################

!# Enable the NAT feature
feature nat

!# Mark the WAN-facing interface as NAT outside
interface ethernet 1/1
   description ** WAN-Internet **
   no shutdown
   ip nat outside
exit

!# Mark each VLAN SVI as NAT inside
interface vlan 10
   ip nat inside
exit
interface vlan 20
   ip nat inside
exit
interface vlan 30
   ip nat inside
exit

!# Define which internal source IPs should be translated
ip access-list extended NAT_INSIDE
   permit ip 192.168.10.0/24 any
   permit ip 192.168.20.0/24 any
   permit ip 192.168.30.0/24 any
exit

!# Overload all matching inside traffic onto the WAN interface IP
ip nat inside source list NAT_INSIDE interface ethernet 1/1 overload
````






## Licences

- In NX-OS, many advanced features require a valid license. You can use a free trial (grace-period) or install paid licenses via Smart Licensing. Below are commands to manage both.

````py
!# ###########
!# LICENSES
!# ###########

!# Enable a free test license for 120 days (activates most features)
license grace-period

!# Display usage for all installed licenses
show license usage

!# Display usage for a specific license package (e.g., LAN_ENTERPRISE_SERVICES_PKG)
show license usage LAN_ENTERPRISE_SERVICES_PKG

!# Display overall license status and expiration details
show license status

!#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

! ### Install paid licenses (requires Internet connectivity)

!# 1. Enable the Smart Licensing feature
feature license smart

!# 2. Activate Smart Licensing
license smart enable

!# 3. Enable DNS resolution (required for Smart Licensing communication)
ip domain-lookup

!# 4. Configure a DNS server (e.g., Google DNS)
ip name-server 8.8.8.8

!# 5. Ensure there is a default route to reach Cisco’s Smart Licensing servers
ip route 0.0.0.0/0 123.123.123.1

!# 6. Verify connectivity to Cisco’s Smart tools
ping tools.cisco.com

!# 7. Start the Call Home process (used to register and report licensing info)
callhome
  transport http use-vrf default
  exit

!# 8. Register with a Cisco ID token (replace with your valid token)
license smart register idtoken e128fa0bcc32ffaccd91ff0001

!# 9. After registration, confirm licenses in the Smart Software Licensing Dashboard provided by Cisco
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










## Fz3r0 Nexus Lab: Important Notes

- Se deben encender y configurar primero los NX9-1 y 2, antes de los edge routers o el OSPF 1 da error. 

## 🎯 Objectives   

- **Hands-on with NX-OS CLI**: Master feature activation and NX-OS naming.  
- **High Availability**: Implement HSRP for gateway redundancy.  
- **Dynamic Routing**: Deploy OSPF Area 0 across core and edge devices.  
- **Layer 2 Resiliency**: Configure Rapid PVST+ and port channels using LACP.  
- **SVI Gateways**: Build SVIs per VLAN and integrate them into OSPF.  
- **Inter-site Connectivity & NAT**: Simulate edge routers with multiple WAN links and NAT overload.




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

