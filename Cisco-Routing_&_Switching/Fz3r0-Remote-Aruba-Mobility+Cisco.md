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

## F0-SW-WAN-00 - WAN SWITCH L3 @ INTERNET

````py
! # F0-SW-WAN-00 - L3 WAN SWITCH (SVI VLAN99 + trunks a routers; OSPF p2p)
enable
configure terminal

! --- System / L3 ---
hostname F0-SW-WAN-00
ip domain-name fz3r0.dojo
lldp run
ip routing

! --- DNS + default propia ---
ip name-server 8.8.8.8
ip name-server 8.8.4.4
ip route 0.0.0.0 0.0.0.0 192.168.0.254

! --- No usamos Vlan1 ---
interface Vlan1
 no ip address
 shutdown

! --- VLANs ---
vlan 99
 name WAN-LAN
vlan 97
 name P2P-DC
vlan 98
 name P2P-BR

! --- Puerto al MÓDEM en L2 (VLAN 99) ---
default interface Gi1/0/1
interface Gi1/0/1
 description *** TO TELMEX HOME MODEM (L2, VLAN 99) ***
 switchport
 switchport mode access
 switchport access vlan 99
 spanning-tree portfast
 no shut

! --- TRUNK a DC ROUTER: permite VLAN 99 y 97 ---
default interface Gi1/0/47
interface Gi1/0/47
 description *** TO DC ROUTER (TRUNK 99 + 97) ***
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 99,97
 spanning-tree portfast trunk
 no shut

! --- TRUNK a BRANCH ROUTER: permite VLAN 99 y 98 ---
default interface Gi1/0/48
interface Gi1/0/48
 description *** TO BRANCH ROUTER (TRUNK 99 + 98) ***
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 99,98
 spanning-tree portfast trunk
 no shut

! --- SVI de la LAN del módem ---
interface Vlan99
 description *** WAN LAN toward Telmex ***
 ip address 192.168.0.99 255.255.255.0
 no shut

! --- SVIs p2p para OSPF hacia los routers ---
interface Vlan97
 description *** P2P OSPF to DC ROUTER ***
 ip address 123.1.1.1 255.255.255.252
 ip ospf network point-to-point
 no shut

interface Vlan98
 description *** P2P OSPF to BRANCH ROUTER ***
 ip address 123.2.2.1 255.255.255.252
 ip ospf network point-to-point
 no shut

! --- Loopback de management ---
interface Loopback0
 description *** MANAGEMENT LOOPBACK ***
 ip address 10.255.0.1 255.255.255.255

! --- OSPF: solo p2p + loopback; NO incluir Vlan99 ---
router ospf 1
 router-id 10.255.0.1
 passive-interface default
 no passive-interface Vlan97
 no passive-interface Vlan98
 network 123.1.1.0 0.0.0.3 area 0
 network 123.2.2.0 0.0.0.3 area 0
 network 10.255.0.1 0.0.0.0 area 0
 ! default-information originate   ! (déjalo apagado: NAT va en routers)

! --- Admin + SSH ---
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption
login block-for 60 attempts 3 within 60
login on-failure log
login on-success log
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






## F0-RT-DC-00 - DC ROUTER (learns default via OSPF from WAN L3)

````py
! # F0-RT-DC-00 - DC ROUTER (trunk al WAN: 97 OSPF p2p + 99 NAT; default -> modem)
enable
configure terminal

! --- System / L3 ---
hostname F0-RT-DC-00
ip domain name fz3r0.dojo
lldp run
ip routing

! --- (Opcional) DNS ---
ip name-server 8.8.8.8
ip name-server 8.8.4.4

! --- Mismo PUERTO al WAN switch, pero con SUBINTERFACES dot1q ---
default interface Gi0/0/0
interface Gi0/0/0
 no ip address
 no shut

! --- Subif OSPF p2p (VLAN 97) hacia F0-SW-WAN-00 ---
interface Gi0/0/0.97
 description *** OSPF P2P to WAN L3 (VLAN 97) ***
 encapsulation dot1q 97
 ip address 123.1.1.2 255.255.255.252
 ip ospf network point-to-point
 no shut

! --- Subif "LAN del modem" para NAT (VLAN 99) ---
interface Gi0/0/0.99
 description *** OUTSIDE to Telmex LAN (VLAN 99) ***
 encapsulation dot1q 99
 ip address 192.168.0.10 255.255.255.0
 ip nat outside
 no shut

! --- Enlace L3 al DC L3 SWITCH (tu /30 existente) ---
interface Gi0/0/1
 description *** TO F0-SW-DC-00 ***
 no ip address 
 no shut
exit
 
interface Gi0/0/1.97
 description *** P2P to F0-SW-DC-00 (VLAN 97) ***
 encapsulation dot1q 97
 ip address 123.1.1.9 255.255.255.252
 ip nat inside
 ip ospf network point-to-point
 no shut




! --- Loopback de management (úsala como source de SSH/ping) ---
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.2 255.255.255.255
 ip nat inside

! --- NAT (PAT) de las redes internas del DC ---
ip access-list standard LAB-DC-NETS
   ! VLAN10 tunneled
   permit 10.10.10.0 0.0.0.255
   ! VLAN20 tunneled   
   permit 10.10.20.0 0.0.0.255
   ! DC mgmt LAN     
   permit 192.168.1.0 0.0.0.255
   ! loopbacks/mgmt   
   permit 10.255.0.0 0.0.255.255   
   !
ip nat inside source list LAB-DC-NETS interface Gi0/0/0.99 overload

! --- Default directa al modem (para que el NAT salga por .99) ---
ip route 0.0.0.0 0.0.0.0 192.168.0.254

! --- OSPF: solo p2p + loopback; NO anunciar VLAN 99 ---
router ospf 1
 router-id 10.255.0.2
 passive-interface default
 no passive-interface Gi0/0/0.97
 no passive-interface Gi0/0/1.97
 ! Gi0/0/0.97
 network 123.1.1.0 0.0.0.3 area 0
 ! Gi0/0/1  
 network 123.1.1.8 0.0.0.3 area 0
 ! Loopback0   
 network 10.255.0.2 0.0.0.0 area 0   


! --- Admin + SSH ---
username admin privilege 15 secret Cisco.12345
enable secret Cisco.12345
service password-encryption
login block-for 60 attempts 3 within 60
login on-failure log
login on-success log
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
vlan 66  name DC-MGMT
vlan 10  name DC-TUNNEL-ENT
vlan 20  name DC-TUNNEL-PSK
vlan 97  name P2P-DC-RTR

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
default interface GigabitEthernet1/0/24
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

