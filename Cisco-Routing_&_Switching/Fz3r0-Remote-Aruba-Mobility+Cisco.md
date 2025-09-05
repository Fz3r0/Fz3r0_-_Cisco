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

https://excalidraw.com/#json=RA13mWpfVUtJBuDdCcnPn,ssxNO5tkG30Av_14nBCb2A

<img width="1531" height="1041" alt="image" src="https://github.com/user-attachments/assets/96d67cc4-e19d-4be2-87da-b0525fa6b798" />

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

## F0-ROUTER-01 - UNIQUE ROUTER

````py

enable
configure terminal
hostname F0-ROUTER-01
ip domain name fz3r0.dojo
lldp run
ip routing


! ¡MGMT!
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.1 255.255.255.255

interface GigabitEthernet0/0/0
 description *** TO TELMEX MODEM ***
 ip address 192.168.0.11 255.255.255.0
 ip nat outside
 no shutdown

interface GigabitEthernet0/0/1
 description *** TO DC L3 SWITCH ***
 ip address 10.255.97.1 255.255.255.252
 ip nat inside
 no shutdown

interface GigabitEthernet0/2/0
 description *** TO BRANCH L3 SWITCH ***
 ip address 10.255.98.1 255.255.255.252
 ip nat inside
 no shutdown

! Rutas a las LANs en DC
ip route 192.168.1.0 255.255.255.0 10.255.97.2

! Rutas a las LANs en Branch
ip route 10.10.100.0 255.255.255.0 10.255.98.2
ip route 10.10.30.0   255.255.255.0 10.255.98.2
ip route 10.10.40.0   255.255.255.0 10.255.98.2
ip route 10.10.130.0  255.255.255.0 10.255.98.2

! Default a Internet
ip route 0.0.0.0 0.0.0.0 192.168.0.254

! NAT de salida para que DC y Branch lleguen a 8.8.8.8
ip access-list standard LAN-ALL
 permit 192.168.1.0 0.0.0.255
 permit 10.10.0.0 0.0.255.255
 permit 10.255.0.0 0.0.255.255
ip nat inside source list LAN-ALL interface GigabitEthernet0/0/0 overload

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






## F0-SW01-DC - DC SWITCH

````py
enable
configure terminal
hostname F0-SW01-DC
ip domain-name fz3r0.dojo
lldp run
ip routing

vlan 66
 name DC-MGMT


! Gateways
interface Vlan66
 description *** DC MGMT ***
 ip address 192.168.1.254 255.255.255.0
 no shutdown

! Enlace a R1
interface GigabitEthernet1/0/24
 description *** L3 TO R1 ***
 no switchport
 ip address 10.255.97.2 255.255.255.252
 no shutdown

! ¡MGMT!
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.11 255.255.255.255

! Default al router
ip route 0.0.0.0 0.0.0.0 10.255.97.1

! INTERFACES ACCESS
interface range GigabitEthernet1/0/1-12
 description *** ACCESS 66 MANAGEMENT ***
 switchport
 switchport mode access
 switchport access vlan 66
 no shutdown

! INTERFACES TRUNK
interface range GigabitEthernet1/0/13-23
 description *** TRUNK UPLINK ***
 no switchport
 switchport
 switchport mode trunk
 no shutdown


! ===== DHCP SERVER EN EL SWITCH DC =====
service dhcp
ip dhcp ping packets 1

! --- DC-MGMT (VLAN 66) 192.168.1.0/24 ---
ip dhcp excluded-address 192.168.1.1 192.168.1.100
ip dhcp excluded-address 192.168.1.201 192.168.1.254
ip dhcp pool DC-MGMT
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.254
 dns-server 8.8.8.8 1.1.1.1
 domain-name fz3r0.dojo
 lease 0 8 0


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




## F0-SW02-BRANCH - BRANCH SWITCH


````py
enable
configure terminal
hostname F0-SW02-BRANCH
ip domain-name fz3r0.dojo
lldp run
ip routing

vlan 30
 name BR-ENT
vlan 40
 name BR-PSK
vlan 100
 name BR-MGMT
vlan 300
 name BR-WLAN-MGMT

! Gateways


interface Vlan30
 description *** BR USERS VLAN701 ***
 ip address 10.10.30.254 255.255.255.0
 no shutdown
interface Vlan40
 description *** BR USERS VLAN702 ***
 ip address 10.10.40.254 255.255.255.0
 no shutdown

interface Vlan100
 description *** BR MGMT ***
 ip address 10.10.100.254 255.255.255.0
 no shutdown
interface Vlan300
 description *** BR WLAN MGMT ***
 ip address 10.10.130.254 255.255.255.0
 no shutdown

! Enlace a R1
interface GigabitEthernet1/0/24
 description *** L3 TO R1 ***
 no switchport
 ip address 10.255.98.2 255.255.255.252
 no shutdown

! ¡MGMT!
interface Loopback0
 description *** MGMT LOOPBACK ***
 ip address 10.255.0.21 255.255.255.255

! Default al router
ip route 0.0.0.0 0.0.0.0 10.255.98.1

! INTERFACES ACCESS
interface range GigabitEthernet1/0/1-8
 description *** ACCESS 100 MANAGEMENT ***
 switchport
 switchport mode access
 switchport access vlan 100
 no shutdown

! INTERFACES ACCESS
interface range GigabitEthernet1/0/9-12
 description *** ACCESS 300 WLAN ***
 switchport
 switchport mode access
 switchport access vlan 300
 no shutdown

! INTERFACES TRUNK (DEFAULT NATIVE)
interface range GigabitEthernet1/0/13-16
 description *** TRUNK DEFAULT NATIVE 1 ***
 switchport
 switchport mode trunk
 no shutdown

! INTERFACES TRUNK (v300 WI-FI NATIVE + PRUNED = GOOD PRACTICES)
interface range GigabitEthernet1/0/17-20
 description *** TRUNK NATIVE 300 WI-FI ***
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 30,40,100,300
 switchport trunk native vlan 300
 no shutdown

! INTERFACES TRUNK (ALLOWED BUT DEFAULT NATIVE VLAN)
interface range GigabitEthernet1/0/21-23
 description *** TRUNK UPLINK ***
 no switchport
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 30,40,100,300
 no shutdown



! ===== DHCP SERVER (en el propio switch) =====
service dhcp
ip dhcp ping packets 1

! --- BR-MGMT (VLAN 100) 10.10.100.0/24 ---
ip dhcp excluded-address 10.10.100.1 10.10.100.100
ip dhcp excluded-address 10.10.100.201 10.10.100.254
ip dhcp pool BR-MGMT
 network 10.10.100.0 255.255.255.0
 default-router 10.10.100.254
 dns-server 8.8.8.8 1.1.1.1
 domain-name fz3r0.dojo
 lease 7

! --- BR-ENT (VLAN 30) 10.10.30.0/24 ---
ip dhcp excluded-address 10.10.30.1 10.10.30.100
ip dhcp excluded-address 10.10.30.201 10.10.30.254
ip dhcp pool BR-ENT
 network 10.10.30.0 255.255.255.0
 default-router 10.10.30.254
 dns-server 8.8.8.8 1.1.1.1
 domain-name fz3r0.dojo
 lease 7

! --- BR-PSK (VLAN 40) 10.10.40.0/24 ---
ip dhcp excluded-address 10.10.40.1 10.10.40.100
ip dhcp excluded-address 10.10.40.201 10.10.40.254
ip dhcp pool BR-PSK
 network 10.10.40.0 255.255.255.0
 default-router 10.10.40.254
 dns-server 8.8.8.8 1.1.1.1
 domain-name fz3r0.dojo
 lease 7

! --- BR-WLAN-MGMT (VLAN 300) 10.10.130.0/24 ---
ip dhcp excluded-address 10.10.130.1 10.10.130.100
ip dhcp excluded-address 10.10.130.201 10.10.130.254
ip dhcp pool BR-WLAN-MGMT
 network 10.10.130.0 255.255.255.0
 default-router 10.10.130.254
 dns-server 8.8.8.8 1.1.1.1
 domain-name fz3r0.dojo
 lease 7

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

