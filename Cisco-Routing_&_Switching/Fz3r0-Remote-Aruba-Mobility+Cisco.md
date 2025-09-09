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

[https://excalidraw.com/#json=YJCGVG31cd2H5FOHVY-Ya,4MR5YVi_UY311SM00dEQdw](https://excalidraw.com/#json=CQFkuw8E_ax7vJdlO_EG2,XvmWGNiLgc-w5I19ZIupvQ)

<img width="1531" height="1041" alt="image" src="https://github.com/user-attachments/assets/4481982b-ee67-4227-a00d-8fec61686bb6" />

## IMPORTANT NOTES!!

### `NATIVE VLANS FOR TRUNKS`

- Aruba APs expect their **management VLAN to be untagged** on the switch port.
- Therefore, the **switch port to the AP must use VLAN 300 as the native VLAN** (so the AP gets an IP in 10.10.130.0/24).
- On **switch-to-switch trunks**, VLAN 300 must be allowed **tagged**, so it reaches the access switch intact.

**Summary:**

- **AP = Switch**: VLAN 300 = native.
- **Switch = Switch**: VLAN 300 = tagged (allowed).

## IP Table: 

- RT1 - Lo0 = `10.255.0.1` <br><br>
- SW2 - Lo0 = `10.255.0.21` 
- SW2 - V100 = `10.10.100.254` <br><br>
- SW3 - Lo0 = `10.255.0.22` 
- SW3 - V100 = `10.10.100.22` <br><br>
- SW1 - Lo0 = `10.255.0.11`
- SW1 - V66 = `192.168.1.254`

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

- RT1 - Lo0 = `10.255.0.1`


````py
!
!


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

! Default a Internet
ip route 0.0.0.0 0.0.0.0 192.168.0.254

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
ip route 10.10.30.0   255.255.255.0 10.255.98.2
ip route 10.10.40.0   255.255.255.0 10.255.98.2
ip route 10.10.100.0  255.255.255.0 10.255.98.2
ip route 10.10.130.0  255.255.255.0 10.255.98.2

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

- SW1 - Lo0 = `10.255.0.11`
- SW1 - V66 = `192.168.1.254`

````py
!
!


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
   spanning-tree portfast
   no shutdown

! INTERFACES TRUNK
interface range GigabitEthernet1/0/13-23
   description *** TRUNK UPLINK ***
   switchport
   switchport mode trunk
   spanning-tree portfast trunk
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

- Lo0 = `10.255.0.21` 
- V100 = `10.10.100.254`

````py
!
!


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
   description *** BR USERS VLAN30 (ENTERPRISE) ***
   ip address 10.10.30.254 255.255.255.0
   no shutdown

interface Vlan40
   description *** BR USERS VLAN40 (PSK) ***
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
   spanning-tree portfast
   no shutdown

! INTERFACES ACCESS
interface range GigabitEthernet1/0/9-12
   description *** ACCESS 300 WLAN ***
   switchport
   switchport mode access
   switchport access vlan 300
   spanning-tree portfast
   no shutdown

! INTERFACES TRUNK (DEFAULT NATIVE)
interface range GigabitEthernet1/0/13-16
   description *** TRUNK DEFAULT NATIVE 1 - ALLOWED PRUNED ***
   switchport
   switchport mode trunk
   spanning-tree portfast trunk
   switchport trunk allowed vlan 30,40,100,300
   no shutdown

! INTERFACES TRUNK (v300 WI-FI NATIVE + PRUNED = GOOD PRACTICES)
interface range GigabitEthernet1/0/17-20
  description *** TRUNK NATIVE 300 WI-FI - GOOD PRACTICES!!! :D ***
   switchport
   switchport mode trunk
   spanning-tree portfast trunk
   switchport trunk allowed vlan 30,40,100,300
   switchport trunk native vlan 300
   no shutdown

! INTERFACES TRUNK (ALLOWED BUT DEFAULT NATIVE VLAN)
interface range GigabitEthernet1/0/21-23
   description *** TRUNK DEFAULT NATIVE 1 ALL ALLOWED ***
   switchport
   switchport mode trunk
   spanning-tree portfast trunk
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
   lease 0 8 0

! --- BR-ENT (VLAN 30) 10.10.30.0/24 ---
ip dhcp excluded-address 10.10.30.1 10.10.30.100
ip dhcp excluded-address 10.10.30.201 10.10.30.254
ip dhcp pool BR-ENT
   network 10.10.30.0 255.255.255.0
   default-router 10.10.30.254
   dns-server 8.8.8.8 1.1.1.1
   domain-name fz3r0.dojo
   lease 0 8 0

! --- BR-PSK (VLAN 40) 10.10.40.0/24 ---
ip dhcp excluded-address 10.10.40.1 10.10.40.100
ip dhcp excluded-address 10.10.40.201 10.10.40.254
ip dhcp pool BR-PSK
   network 10.10.40.0 255.255.255.0
   default-router 10.10.40.254
   dns-server 8.8.8.8 1.1.1.1
   domain-name fz3r0.dojo
   lease 0 8 0

! --- BR-WLAN-MGMT (VLAN 300) 10.10.130.0/24 ---
ip dhcp excluded-address 10.10.130.1 10.10.130.100
ip dhcp excluded-address 10.10.130.201 10.10.130.254
ip dhcp pool BR-WLAN-MGMT
   network 10.10.130.0 255.255.255.0
   default-router 10.10.130.254
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


## F0-SW03-BRANCH - BRANCH SWITCH ACCESS POE

- Lo0 = `10.255.0.22` 
- V100 = `10.10.100.22`

````py
!
!


enable
configure terminal
hostname F0-SW03-BRANCH
ip domain-name fz3r0.dojo
lldp run

vlan 30
   name BR-ENT
vlan 40
   name BR-PSK
vlan 100
   name BR-MGMT
vlan 300
   name BR-WLAN-MGMT

! ¡MGMT!
interface Loopback0
   description *** MGMT LOOPBACK ***
   ip address 10.255.0.22 255.255.255.255

! Management por VLAN 100
interface Vlan100
 description *** BR MGMT ***
 ip address 10.10.100.22 255.255.255.0
 no shutdown

! Default al gateway
ip default-gateway 10.10.100.254

! INTERFACES ACCESS
interface range GigabitEthernet1/0/1-8
   description *** ACCESS 100 MANAGEMENT ***
   switchport
   switchport mode access
   switchport access vlan 100
   spanning-tree portfast
   no shutdown

! INTERFACES ACCESS
interface range GigabitEthernet1/0/9-12
   description *** ACCESS 300 WLAN ***
   switchport
   switchport mode access
   switchport access vlan 300
   spanning-tree portfast
   no shutdown

! INTERFACES TRUNK (DEFAULT NATIVE)
interface range GigabitEthernet1/0/13-16
   description *** TRUNK DEFAULT NATIVE 1 - ALLOWED PRUNED ***
   switchport
   switchport mode trunk
   spanning-tree portfast trunk
   switchport trunk allowed vlan 30,40,100,300
   no shutdown

! INTERFACES TRUNK (v300 WI-FI NATIVE + PRUNED = GOOD PRACTICES)
interface range GigabitEthernet1/0/17-20
  description *** TRUNK NATIVE 300 WI-FI - GOOD PRACTICES!!! :D ***
   switchport
   switchport mode trunk
   spanning-tree portfast trunk
   switchport trunk allowed vlan 30,40,100,300
   switchport trunk native vlan 300
   no shutdown

! INTERFACES TRUNK (ALLOWED BUT DEFAULT NATIVE VLAN)
interface range GigabitEthernet1/0/21-48
   description *** TRUNK DEFAULT NATIVE 1 ALL ALLOWED ***
   switchport
   switchport mode trunk
   spanning-tree portfast trunk
   no shutdown

! Endurecimiento + SSH
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





## SPAConfig

````
enable
configure terminal

! # 1. Sesión SPAN local: fuentes (APs) 

! #  -- Fuentes: Gi1/0/11 (AP-1) y Gi1/0/19 (AP-2)
monitor session 5 source interface Gi1/0/11 both
monitor session 5 source interface Gi1/0/19 both

! # 2. Sesión SPAN local: destino (Wireshark) 

! #  -- Destino: Gi1/0/5 hacia tu laptop con Wireshark
monitor session 5 destination interface Gi1/0/5

! # 3. Guardar

end
wr

!

! # 4. Verificación

show monitor session 10
show interfaces status | include Gi1/0/5

!
!


````

## 🛠️ `RSPAN Configuration` 

1. Create the RSPAN VLAN and allow it on trunks (both switches)
2. Create the source sessions (the ports you want to mirror, e.g. APs, laptops, servers, etc)
3. Create the destination session (where Wireshark is running)

### ⭕ 1) `Configure the RSPAN VLAN and trunks`

- Create the dedicated remote-span VLAN on both switches (VLAN 888):

#### F0-SW02-BRANCH - BRANCH SWITCH:

```py
! ! # VLAN 888 RSPAN:
enable
configure terminal
!
vlan 888
   name RSPAN-888
   remote-span
exit
!
! # TRUNK PORTS:
interface range GigabitEthernet1/0/21-23
   switchport trunk allowed vlan ADD 888
end
!
wr

!
!

```

#### F0-SW03-BRANCH - BRANCH SWITCH:

```py
! ! # VLAN 888 RSPAN:
enable
configure terminal
!
vlan 888
   name RSPAN-888
   remote-span
exit
!
! # TRUNK PORTS:
interface range GigabitEthernet1/0/21-48
   switchport trunk allowed vlan ADD 888
end
!
wr

!
!

```

Validate:

```py
show interfaces trunk | include 888
show vlan remote-span
```

---

### ⭕ 2) `Configure the Source sessions` (mirror the AP ports)

#### 📡 `AP-1 on Branch Switch-2` :: Session 11

```py
enable
configure terminal
!
! # Session 11: source = Gi 1/0/11 (both directions)
monitor session 11 source interface Gi 1/0/11 both
! # Destination = remote RSPAN VLAN 888
monitor session 11 destination remote vlan 888
end
```
```py
! Verify
show monitor session 11
show monitor session all
```

#### 📡 `AP-2 on Switch-2` :: Session 12

```py
enable
configure terminal
!
! # Session 12: source = Gi 1/0/19 (both directions)
monitor session 12 source interface Gi 1/0/19 both
! # Destination = remote RSPAN VLAN 888
monitor session 12 destination remote vlan 888
end
```
```py
! Verify
show monitor session 12
show monitor session all
```

---

### ⭕ 3) Configure the Destination session (Wireshark analyzer on Switch-2)

#### 🦈 Wireshark / Laptop

```py
enable
configure terminal
!
! # VSession 10: source = remote RSPAN VLAN 888
monitor session 10 source remote vlan 888
! # VDestination = analyzer interface (plug your Wireshark laptop here)
monitor session 10 destination interface gi 1/0/5
end
```
```py
! # Verify
show monitor session 10
show monitor session all
show interface status | include Gi1/0/5
```

### ⭕ Remove RSPANs

```py
! # Verify
no monitor session 10
no monitor session 11
no monitor session 12
```



# 🗃️ Resources

- Aruba good practices for bridged and tunneled!!! https://arubanetworking.hpe.com/techdocs/VSG/docs/035-campus-migrate/esp-campus-migrate-060-aos8to10upgrade-steps/?utm_source=chatgpt.com#bridge-mode-wlans

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

