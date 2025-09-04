# 🧠🏗️🌐 Cisco IOS & Aruba Mobility: `Fz3r0 Remote Aruba Mobility + Cisco Lab`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` 

---

<br>

# 📄 `Index`

#  Cisco IOS & Aruba Mobility :: `Fz3r0 Remote Aruba Mobility + Cisco Lab`

## IMPORTANT NOTES!!

### `NATIVE VLANS FOR TRUNKS`

- Aruba APs expect their **management VLAN to be untagged** on the switch port.
- Therefore, the **switch port to the AP must use VLAN 300 as the native VLAN** (so the AP gets an IP in 10.10.130.0/24).
- On **switch-to-switch trunks**, VLAN 300 must be allowed **tagged**, so it reaches the access switch intact.

**Summary:**

- **AP = Switch**: VLAN 300 = native.
- **Switch = Switch**: VLAN 300 = tagged (allowed).

---



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
!switch 2 renumber 1

! # Reloads the device for a full factory‑default reset.
reload
````

# `DC SIDE`

## F0-SW-WAN-01 - WAN SWITCH L2

````py
! # ========= BASE =========
enable
configure terminal
! # Hostname
hostname F0-SW-WAN-01
! # Dominio para SSH
ip domain-name fz3r0.dojo
! # Descubrimiento L2
lldp run
! # Este switch será solo L2
no ip routing

! # ========= VLANING =========
! # VLAN de la LAN del módem
vlan 99
 name WAN-LAN

! # ========= MGMT DEL SWITCH =========
! # IP de administración en la LAN del módem
interface Vlan99
 description *** MGMT on WAN-LAN toward Telmex ***
 ip address 192.168.0.10 255.255.255.0
 no shutdown
! # Default-gateway L2 para administrar el switch
ip default-gateway 192.168.0.254

! # ========= PUERTO AL MÓDEM =========
default interface Gi1/0/1
interface Gi1/0/1
 description *** TO TELMEX HOME MODEM ***
 switchport
 switchport mode access
 switchport access vlan 99
 spanning-tree portfast
 no shutdown

! # ========= TRUNK AL DC ROUTER (SOLO VLAN 99) =========
default interface Gi1/0/47
interface Gi1/0/47
 description *** TO DC ROUTER (VLAN 99 only) ***
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 99
 spanning-tree portfast trunk
 no shutdown

! # ========= TRUNK AL BRANCH ROUTER FUTURO (SOLO VLAN 99) =========
default interface Gi1/0/48
interface Gi1/0/48
 description *** TO BRANCH ROUTER (VLAN 99 only, future) ***
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 99
 spanning-tree portfast trunk
 no shutdown

! # ========= ENDURECIMIENTO BASICO + SSH =========
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption
crypto key generate rsa modulus 2048
ip ssh version 2

line con 0
 logging synchronous
 password Cisco.12345
 login
line vty 0 15
 transport input ssh
 login local
 exec-timeout 10 0

no ip http server
no ip http secure-server
end
wr

!
!


````






## F0-RT-DC-01 - DC ROUTER (learns default via OSPF from WAN L3)

````py
! # ========= BASE =========
enable
configure terminal
! # Hostname
hostname F0-RT-DC-01
! # Dominio para SSH
ip domain name fz3r0.dojo
! # Routing L3 activo
ip routing
! # Descubrimiento L2
lldp run
! # DNS opcional
ip name-server 8.8.8.8
ip name-server 8.8.4.4

! # ========= OUTSIDE HACIA EL MÓDEM EN VLAN 99 (TRUNK CON EL SW-WAN) =========
default interface Gi0/0/0
interface Gi0/0/0
 no ip address
 no shutdown
!
interface Gi0/0/0.99
 description *** OUTSIDE to Telmex LAN (VLAN 99) ***
 encapsulation dot1q 99
 ip address 192.168.0.11 255.255.255.0
 ip nat outside
 no shutdown

! # ========= ENLACE L3 AL SWITCH DC =========
default interface Gi0/0/1
interface Gi0/0/1
 description *** L3 to F0-SW-DC-00 ***
 ip address 10.255.97.1 255.255.255.252
 ip nat inside
 no shutdown

! # ========= LOOPBACK DE MGMT =========
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.2 255.255.255.255
 ip nat inside

! # ========= NAT PAT PARA LAS REDES DEL DC =========
ip access-list standard LAB-DC-NETS
 ! VLAN10 usuarios
 permit 10.10.10.0 0.0.0.255
 ! VLAN20 usuarios
 permit 10.10.20.0 0.0.0.255
 ! VLAN66 mgmt DC
 permit 192.168.1.0 0.0.0.255
 ! Loopbacks internas
 permit 10.255.0.0 0.0.255.255
!
ip nat inside source list LAB-DC-NETS interface Gi0/0/0.99 overload

! # ========= DEFAULT ROUTE A LA PUERTA DEL MÓDEM =========
ip route 0.0.0.0 0.0.0.0 192.168.0.254

! # ========= OSPF SOLO CON EL SWITCH DC =========
router ospf 1
 router-id 10.255.0.2
 passive-interface default
 no passive-interface Gi0/0/1
 network 10.255.97.0 0.0.0.3 area 0
 network 10.255.0.2 0.0.0.0 area 0
! # No metas 192.168.0.0/24 al OSPF

! # ========= ENDURECIMIENTO BÁSICO + SSH =========
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption
crypto key generate rsa modulus 2048
ip ssh version 2
ip ssh source-interface Loopback0

line con 0
 logging synchronous
 password Cisco.12345
 login
line vty 0 15
 transport input ssh
 login local
 exec-timeout 10 0

no ip http server
no ip http secure-server
end
wr

!
!


````




## F0-SW-DC-00 - DC CORE SWITCH L3 (LAN GATEWAY & UPLINK TO ROUTER)

````py
! # F0-SW-DC-00 - DC L3 SWITCH (SVIs + SVI p2p VLAN 97 + OSPF p2p + default al router)
enable
configure terminal

! --- System / L3 ---
hostname F0-SW-DC-00
ip domain-name fz3r0.dojo
lldp run
ip routing

! --- DNS (opcional) ---
ip name-server 8.8.8.8
ip name-server 8.8.4.4

! --- No usamos Vlan1 ---
interface Vlan1
 no ip address
 shutdown

! --- VLANs del DC ---
vlan 66
name DC-MGMT
vlan 10
name DC-TUNNEL-ENT
vlan 20
name DC-TUNNEL-PSK
vlan 97
name P2P-DC-RTR

! --- SVIs (gateways del DC) ---
interface Vlan66
 description *** DC MGMT GATEWAY ***
 ip address 192.168.1.254 255.255.255.0
 no shut
!
interface Vlan10
 description *** DC USERS TUNNELED VLAN10 ***
 ip address 10.10.10.1 255.255.255.0
 no shut
!
interface Vlan20
 description *** DC USERS TUNNELED VLAN20 ***
 ip address 10.10.20.1 255.255.255.0
 no shut

! --- SVI p2p al DC Router (OSPF point-to-point) ---
interface Vlan97
 description *** P2P OSPF to F0-RT-DC-00 ***
 ip address 123.1.1.10 255.255.255.252
 ip ospf network point-to-point
 no shut

! --- Puerto al DC Router: TRUNK solo VLAN 97 ---
interface GigabitEthernet1/0/24
 description *** TO F0-RT-DC-00 (TRUNK SOLO VLAN 97) ***
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 97
 spanning-tree portfast trunk
 no shut

! --- Puertos de acceso MGMT (opcional) ---
interface range GigabitEthernet1/0/1 - 12
 description *** MGMT INTERFACES DATACENTER ***
 switchport
 switchport mode access
 switchport access vlan 66
 spanning-tree portfast
 no shut

! --- Trunks futuros (opcional) ---
interface range GigabitEthernet1/0/13 - 23
 description *** TRUNK INTERFACES (FUTURE USE) ***
 switchport
 switchport mode trunk
 ! switchport trunk allowed vlan 66,10,20
 spanning-tree portfast trunk
 no shut

! --- Loopback de management ---
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.11 255.255.255.255

! --- Default: todo hacia el DC Router (que hace NAT por .99) ---
ip route 0.0.0.0 0.0.0.0 123.1.1.9

! --- OSPF: p2p en Vlan97 + loopback; SVIs de usuarios en pasivo ---
router ospf 1
 router-id 10.255.0.11
 passive-interface default
 no passive-interface Vlan97
 network 123.1.1.8 0.0.0.3 area 0     ! Vlan97 p2p al router
 network 192.168.1.0 0.0.0.255 area 0 ! VLAN66 mgmt
 network 10.10.10.0 0.0.0.255 area 0  ! VLAN10
 network 10.10.20.0 0.0.0.255 area 0  ! VLAN20
 network 10.255.0.11 0.0.0.0 area 0   ! Loopback

! --- Admin + SSH ---
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption
crypto key generate rsa modulus 2048
ip ssh version 2
ip ssh source-interface Loopback0
line con 0
 logging synchronous
 password Cisco.12345
 login
line vty 0 15
 transport input ssh
 login local
 exec-timeout 10 0
no ip http server
no ip http secure-server

end
wr





````
















---

# `BRANCH SIDE`

````py
! # F0-RT-BR-00 - BRANCH ROUTER (trunk al WAN: 98 OSPF p2p + 99 NAT; default -> modem)
enable
configure terminal

! --- System / L3 ---
hostname F0-RT-BR-00
ip domain name fz3r0.dojo
lldp run
ip routing

! --- (Opcional) DNS ---
ip name-server 8.8.8.8
ip name-server 8.8.4.4

! --- Mismo puerto al WAN switch con subinterfaces ---
default interface Gi0/0/0
interface Gi0/0/0
 no ip address
 no shut

! --- Subif OSPF p2p (VLAN 98) ---
interface Gi0/0/0.98
 description *** OSPF P2P to WAN L3 (VLAN 98) ***
 encapsulation dot1q 98
 ip address 123.2.2.2 255.255.255.252
 ip ospf network point-to-point
 no shut

! --- Subif “LAN del módem” para NAT (VLAN 99) ---
interface Gi0/0/0.99
 description *** OUTSIDE to Telmex LAN (VLAN 99) ***
 encapsulation dot1q 99
 ip address 192.168.0.11 255.255.255.0
 ip nat outside
 no shut

! --- Enlace al BRANCH L3 SWITCH (tu /30 existente) ---
interface Gi0/0/1
 description *** TO F0-SW-BR-00 ***
 ip address 123.2.2.9 255.255.255.252
 ip nat inside
 ip ospf network point-to-point
 no shut

! --- Loopback de management (opcional; la marcamos inside para pruebas) ---
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.3 255.255.255.255
 ip nat inside

! --- NAT (PAT) de redes del branch ---
ip access-list standard LAB-BR-NETS
 permit 10.10.30.0 0.0.0.255
 permit 10.10.40.0 0.0.0.255
 permit 10.10.100.0 0.0.0.255
 permit 10.10.130.0 0.0.0.255
 permit 10.255.0.0 0.0.255.255
!
ip nat inside source list LAB-BR-NETS interface Gi0/0/0.99 overload

! --- Default directa al módem (para que NAT salga por .99) ---
ip route 0.0.0.0 0.0.0.0 192.168.0.254

! --- OSPF: solo p2p + loopback; NO anunciar .99 ---
router ospf 1
 router-id 10.255.0.3
 passive-interface default
 no passive-interface Gi0/0/0.98
 no passive-interface Gi0/0/1
 network 123.2.2.0 0.0.0.3 area 0
 network 123.2.2.8 0.0.0.3 area 0
 network 10.255.0.3 0.0.0.0 area 0

! --- Admin + SSH ---
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption
crypto key generate rsa modulus 2048
ip ssh version 2
ip ssh source-interface Loopback0
line con 0
 logging synchronous
 password Cisco.12345
 login
line vty 0 15
 transport input ssh
 login local
 exec-timeout 10 0
no ip http server
no ip http secure-server

end
wr

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

