# 🧠🏗️🌐 Cisco Nexus: `Fz3r0 Home Lab Basic Setup`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` 

---

<br>

# 📄 `Index`

## Wipe

````py
! #####################################################################################
! # WIPE SWITCH TO INIT SETTINGS                                                      #
! #####################################################################################

! # Completely erases the startup configuration
write erase

! # Completely erases the VLAN database
delete vlan.dat

! # Completely erases any stack provisioning
!no switch <X> provision

! # Reloads the device for a full factory‑default reset.
reload
````

## F0-SW-01 - Core Switch 1

````py
enable
conf t

hostname F0-SW-01
ip domain-name fz3r0.dojo

! DNS y gateway para administración L2 (sin ip routing)
ip name-server 8.8.8.8
ip name-server 8.8.4.4
ip default-gateway 192.168.1.254
ip route 0.0.0.0 0.0.0.0 192.168.1.254

! usuarios y secretos
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption

! proteger intentos
login block-for 60 attempts 3 within 60
login on-failure log
login on-success log

! claves y SSH
crypto key generate rsa modulus 2048
ip ssh version 2

! consola (si quieres que pida password en consola, agrega 'login')
line con 0
 logging synchronous
 no transport preferred
! login
! password Cisco.12345
exit

! VTY: SOLO SSH + usuario local en TODAS las líneas
line vty 0 15
 no transport preferred
 transport input ssh
 login local
 exec-timeout 10 0
exit

authentication mac-move permit

! VLANs
vlan 10
 name V10-BLUE
exit
vlan 20
 name V20-RED
exit
vlan 30
 name V30-GREEN
exit
vlan 40
 name V40-VOIP
exit
vlan 50
 name V50-WIRELESS-TUNNEL
exit
vlan 66
 name V66-MANAGEMENT
exit
vlan 888
   name V888-RSPAN
   remote-span
exit
vlan 999
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native
! # <Auto-Exit>

! (global) taggear la native (si lo necesitas)
vlan dot1q tag native

interface Vlan1
   no ip address
   shutdown
exit

! SVI de management
interface Vlan 66
 description *** V66-MANAGEMENT - GATEWAY ***
 ip address 192.168.1.11 255.255.255.0
 no shutdown
exit

! Puerto hacia tu mgmt
interface range GigabitEthernet1/0/1-12
 description *** V66 MANAGEMENT ***
 switchport
 switchport mode access
 switchport access vlan 66
 spanning-tree portfast
   no shutdown
exit

! Trunks solitos
interface range GigabitEthernet1/0/13-16
 description *** Trunk ***
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan all
   no shutdown
exit

! Trunks port channel

interface range GigabitEthernet1/0/17-20
   description *** Uplink (Port-Channel-1 Members) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   channel-group 1 mode active
   no shutdown
exit
!
! # Paso 2: Configurar la interfaz Port-Channel lógica
interface Port-channel 1
   description *** Uplink (Trunk Port-Channel 1) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   no shutdown
exit

interface range GigabitEthernet1/0/21-24
   description *** Uplink (Port-Channel-2 Members) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   channel-group 2 mode active
   no shutdown
exit

! # Paso 2: Configurar la interfaz Port-Channel lógica
interface Port-channel 2
   description *** Uplink (Trunk Port-Channel 2) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   no shutdown
exit

lldp run

end
write memory

!
!

````


## F0-SW-02 - Core Switch 2

````py
enable
conf t

hostname F0-SW-02
ip domain-name fz3r0.dojo

! DNS y gateway para administración L2 (sin ip routing)
ip name-server 8.8.8.8
ip name-server 8.8.4.4
ip default-gateway 192.168.1.254
ip route 0.0.0.0 0.0.0.0 192.168.1.254

! usuarios y secretos
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption

! proteger intentos
login block-for 60 attempts 3 within 60
login on-failure log
login on-success log

! claves y SSH
crypto key generate rsa modulus 2048
ip ssh version 2

! consola (si quieres que pida password en consola, agrega 'login')
line con 0
 logging synchronous
 no transport preferred
! login
! password Cisco.12345
exit

! VTY: SOLO SSH + usuario local en TODAS las líneas
line vty 0 15
 no transport preferred
 transport input ssh
 login local
 exec-timeout 10 0
exit

authentication mac-move permit

! VLANs
vlan 10
 name V10-BLUE
exit
vlan 20
 name V20-RED
exit
vlan 30
 name V30-GREEN
exit
vlan 40
 name V40-VOIP
exit
vlan 50
 name V50-WIRELESS-TUNNEL
exit
vlan 66
 name V66-MANAGEMENT
exit
vlan 888
   name V888-RSPAN
   remote-span
exit
vlan 999
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native
! # <Auto-Exit>

! (global) taggear la native (si lo necesitas)
vlan dot1q tag native

interface Vlan1
   no ip address
   shutdown
exit

! SVI de management
interface Vlan 66
 description *** V66-MANAGEMENT - GATEWAY ***
 ip address 192.168.1.12 255.255.255.0
 no shutdown
exit

! Puerto hacia tu mgmt
interface range GigabitEthernet1/0/1-12
 description *** V66 MANAGEMENT ***
 switchport
 switchport mode access
 switchport access vlan 66
 spanning-tree portfast
   no shutdown
exit

! Trunks solitos
interface range GigabitEthernet1/0/13-20
 description *** Trunk ***
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan all
   no shutdown
exit

! Trunks port channel

interface range GigabitEthernet1/0/17-20
   description *** Uplink (Port-Channel-1 Members) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   channel-group 1 mode active
   no shutdown
exit
!
! # Paso 2: Configurar la interfaz Port-Channel lógica
interface Port-channel 1
   description *** Uplink (Trunk Port-Channel 1) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   no shutdown
exit

interface range GigabitEthernet1/0/21-24
   description *** Uplink (Port-Channel-2 Members) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   channel-group 2 mode active
   no shutdown
exit

! # Paso 2: Configurar la interfaz Port-Channel lógica
interface Port-channel 2
   description *** Uplink (Trunk Port-Channel 2) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   no shutdown
exit

lldp run

end
write memory

!
!

````



## F0-SW-03 - Access Switch 3

````py
enable
conf t

hostname F0-SW-03
ip domain-name fz3r0.dojo

! DNS y gateway para administración L2 (sin ip routing)
ip name-server 8.8.8.8
ip name-server 8.8.4.4
ip default-gateway 192.168.1.254
ip route 0.0.0.0 0.0.0.0 192.168.1.254

! usuarios y secretos
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption

! proteger intentos
login block-for 60 attempts 3 within 60
login on-failure log
login on-success log

! claves y SSH
crypto key generate rsa modulus 2048
ip ssh version 2

! consola (si quieres que pida password en consola, agrega 'login')
line con 0
 logging synchronous
 no transport preferred
! login
! password Cisco.12345
exit

! VTY: SOLO SSH + usuario local en TODAS las líneas
line vty 0 15
 no transport preferred
 transport input ssh
 login local
 exec-timeout 10 0
exit

authentication mac-move permit

! VLANs
vlan 10
 name V10-BLUE
exit
vlan 20
 name V20-RED
exit
vlan 30
 name V30-GREEN
exit
vlan 40
 name V40-VOIP
exit
vlan 50
 name V50-WIRELESS-TUNNEL
exit
vlan 66
 name V66-MANAGEMENT
exit
vlan 888
   name V888-RSPAN
   remote-span
exit
vlan 999
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native
! # <Auto-Exit>

! (global) taggear la native (si lo necesitas)
vlan dot1q tag native

interface Vlan1
   no ip address
   shutdown
exit

! SVI de management
interface Vlan 66
 description *** V66-MANAGEMENT - GATEWAY ***
 ip address 192.168.1.13 255.255.255.0
 no shutdown
exit

! Puerto hacia tu mgmt
interface range GigabitEthernet1/0/1-36
 description *** V66 MANAGEMENT ***
 switchport
 switchport mode access
 switchport access vlan 66
 spanning-tree portfast
   no shutdown
exit

! Trunks solitos
interface range GigabitEthernet1/0/37-40
 description *** Trunk ***
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan all
   no shutdown
exit

! Trunks port channel

interface range GigabitEthernet1/0/41-44
   description *** Uplink (Port-Channel-1 Members) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   channel-group 1 mode active
   no shutdown
exit
!
! # Paso 2: Configurar la interfaz Port-Channel lógica
interface Port-channel 1
   description *** Uplink (Trunk Port-Channel 1) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   no shutdown
exit

interface range GigabitEthernet1/0/45-48
   description *** Uplink (Port-Channel-2 Members) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   channel-group 2 mode active
   no shutdown
exit
!
! # Paso 2: Configurar la interfaz Port-Channel lógica
interface Port-channel 2
   description *** Uplink (Trunk Port-Channel 2) ***
   switchport
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan all
   no shutdown
exit

lldp run

end
write memory

!
!

````

# 🗃️ Resources

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

