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






## F0-RT-DC-01 - DC ROUTER 

````py
enable
configure terminal
hostname F0-RT-DC-01
ip domain-name fz3r0.dojo
ip routing
lldp run

! DNS opcional
ip name-server 8.8.8.8
ip name-server 8.8.4.4

! OUTSIDE hacia Telmex por VLAN 99
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

! Enlace L3 al switch DC
default interface Gi0/0/1
interface Gi0/0/1
 description *** L3 to F0-SW-DC-01 ***
 ip address 10.255.97.1 255.255.255.252
 ip nat inside
 no shutdown

! Loopback de mgmt (opcional)
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.2 255.255.255.255
 ip nat inside

! NAT PAT para redes del DC
ip access-list standard LAB-DC-NETS
 permit 192.168.1.0 0.0.0.255
 permit 10.10.10.0 0.0.0.255
 permit 10.10.20.0 0.0.0.255
 permit 10.255.0.0 0.0.255.255
!
ip nat inside source list LAB-DC-NETS interface Gi0/0/0.99 overload

! Default a la puerta del módem
ip route 0.0.0.0 0.0.0.0 192.168.0.254

! Rutas estáticas hacia las LAN del DC
ip route 192.168.1.0 255.255.255.0 10.255.97.2
ip route 10.10.10.0 255.255.255.0 10.255.97.2
ip route 10.10.20.0 255.255.255.0 10.255.97.2
! (si quieres llegar a la loopback del switch)
ip route 10.255.0.11 255.255.255.255 10.255.97.2

! Endurecimiento + SSH
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




## F0-SW-DC-01 - DC CORE SWITCH L3 (LAN GATEWAY & UPLINK TO ROUTER)

````py

enable
configure terminal
hostname F0-SW-DC-01
ip domain-name fz3r0.dojo
lldp run
ip routing

! VLANs
vlan 66
 name DC-MGMT
vlan 10
 name DC-TUNNEL-ENT
vlan 20
 name DC-TUNNEL-PSK

! Gateways
interface Vlan66
 description *** DC MGMT GATEWAY ***
 ip address 192.168.1.254 255.255.255.0
 no shutdown
interface Vlan10
 description *** DC USERS TUNNELED VLAN10 ***
 ip address 10.10.10.1 255.255.255.0
 no shutdown
interface Vlan20
 description *** DC USERS TUNNELED VLAN20 ***
 ip address 10.10.20.1 255.255.255.0
 no shutdown

! Enlace L3 al router
interface GigabitEthernet1/0/24
 description *** L3 to F0-RT-DC-01 ***
 no switchport
 ip address 10.255.97.2 255.255.255.252
 no shutdown

! Puertos acceso mgmt (opcional)
interface range GigabitEthernet1/0/1 - 12
 description *** MGMT INTERFACES DATACENTER ***
 switchport
 switchport mode access
 switchport access vlan 66
 spanning-tree portfast
 no shutdown

! Trunks futuros (opcional)
interface range GigabitEthernet1/0/13 - 23
 description *** TRUNK INTERFACES (FUTURE USE) ***
 switchport
 switchport mode trunk
 spanning-tree portfast trunk
 no shutdown

! Loopback mgmt (opcional)
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.11 255.255.255.255

! Default al router
ip route 0.0.0.0 0.0.0.0 10.255.97.1

! Endurecimiento + SSH
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
















---

# `BRANCH SIDE`




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

