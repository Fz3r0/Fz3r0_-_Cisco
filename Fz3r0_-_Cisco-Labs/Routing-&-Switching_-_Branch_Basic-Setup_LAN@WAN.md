# 🚀🔄🔧 Cisco: `Dynamic Routing` > `Link State` :: `OSPF`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Dynamic Routing` `OSPF` `Link State` `Packet Tracer` `CCNA` `CCNP` `Cisco`

---

<br>

# 📖 Fz3r0 Network Lab: Basic Branch LAN to WAN configuration


## Network Description

## Topology



## Numenclature Standard

Standard: `Device #` - `Zone #` - `Building # / Level` - `Project`

Example: `Router 1` - `MDF 1` - `Building 1 / Level 0` - `Fz3r0`

Nomenclature Creation: `xxn` - `xxxn` - `xn xn` - `xx` 

Result: `RT1-MDF1-B1L0-F0`

## Addressing Table




## VMware Virtual Interfaces

![image](https://github.com/user-attachments/assets/acc53fe8-4366-439c-8340-768c1adf1416)





# Configuration


## Configuration: `Routers`

### RT1 :: (Router 1) :: `RT1-MDF1-B1L0-F0`

```py
!
! ############################
! ## ROUTER 1 CONFIGURATION ##
! ############################
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 1. Initialize Router:
!
enable
configure terminal
!
hostname RT1-MDF1-B1L0-F0
!
banner motd #
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
             
             Fz3r0 @ Cisco CCNA/CCNP Labs
           
             Twitter : @Fz3r0_OPs
             Github  : github.com/Fz3r0
             
             Device  : RT1-MDF1-B1L0-F0
             
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
#
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 2. Interface 0/0 Configuration: From Router <--> To Switch
!
interface ethernet 0/0
   description Trunk Link to LAN (Switch)
   no shutdown
!
! - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
!
! ### 2.1 Create Sub-interfaces on Interface for VLANs:
!
interface ethernet 0/0.10
   description VLAN 10 - ALFA
   encapsulation dot1Q 10
      ip address 10.10.0.1 255.255.0.0
   no shutdown
exit
!
interface ethernet 0/0.20
   description VLAN 20 - BRAVO
   encapsulation dot1Q 20
      ip address 10.20.0.1 255.255.0.0
   no shutdown
exit
!
interface ethernet 0/0.30
   description VLAN 30 - BRAVO
   encapsulation dot1Q 30
      ip address 10.30.0.1 255.255.0.0
   no shutdown
exit
!
interface ethernet 0/0.40
   description VLAN 40 - BRAVO
   encapsulation dot1Q 40
      ip address 10.40.0.1 255.255.0.0
   no shutdown
exit
!
interface ethernet 0/0.50
   description VLAN 50 - VOICE
   encapsulation dot1Q 50
      ip address 10.50.0.1 255.255.0.0
   no shutdown
exit
!
interface ethernet 0/0.66
   description VLAN 66 - BRAVO
   encapsulation dot1Q 66
      ip address 10.66.0.1 255.255.0.0
   no shutdown
exit
!
!
interface ethernet 0/0.99
   description VLAN 99 - NATIVE VLAN
   encapsulation dot1Q 99 native
      no ip address
   no shutdown
exit
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 3. Interface 0/3 Configuration: From Router (LAN) <--> To ISP (WAN)
!
interface ethernet 0/3
   description WAN (From: ROUTER -> To: ISP)
      ip address 123.123.123.1 255.255.255.252
   no shutdown
exit
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 3. NAT: Translate Local Private Subnets (VLANs) to WAN public IP
!
! # Enable NAT on the WAN interface (WAN: towards the ISP / Outside)
interface ethernet 0/3
   ip nat outside
!
! # Enable NAT on the LAN interfaces (LAN: VLAN subinterfaces / Inside)
interface ethernet 0/0.10
   ip nat inside
interface ethernet 0/0.20
   ip nat inside
interface ethernet 0/0.30
   ip nat inside
interface ethernet 0/0.40
   ip nat inside
interface ethernet 0/0.50
   ip nat inside
interface ethernet 0/0.66
   ip nat inside
exit
!
! # Create an access list to define which networks will be translated (private IPs) (ACL 1-99 private ACL)
access-list 69 permit 10.10.0.0 0.0.255.255
access-list 69 permit 10.20.0.0 0.0.255.255
access-list 69 permit 10.30.0.0 0.0.255.255
access-list 69 permit 10.40.0.0 0.0.255.255
access-list 69 permit 10.50.0.0 0.0.255.255
access-list 69 permit 10.66.0.0 0.0.255.255
!
! # Configure NAT overload (PAT) to translate multiple private IPs into the public IP of R1
ip nat inside source list 69 interface ethernet 0/3 overload
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 4. Default Route: Inject a default route in the LAN border router to get Internet
!
ip route 0.0.0.0 0.0.0.0 123.123.123.2
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 5. Secure Login & SSH Configuration:
!
! # Create Access List to only Permit VLAN 66 to access SSH
access-list 101 permit tcp 10.66.0.0 0.0.255.255 any eq 22
access-list 101 deny ip any any
!
! # Add domain to generate RSA key
ip domain-name Fz3r0.domain
crypto key generate rsa general-keys modulus 2048
!
! # Admin user + password
username fz3r0 privilege 15 secret cisco.12345
!
! # Enable CLI password
enable secret fz3r0.12345
service password-encryption
security passwords min-length 10
login block-for 120 attempts 3 within 60
!
! # Console port password
line console 0
   password fz3r0.12345
   login local
   logging synchronous
   exec-timeout 5 30
exit
!
! # AUX port = OFF
line aux 0
   privilege level 1
   transport input none
   transport output none
   login local
   no exec
exit
!
! # VTY: Five simultaneous remote connections 0 to 5 / Using ACL:666 (Only Subnet 66)
line vty 0 4
    access-class 666 in
    transport input ssh
    login local
    logging synchronous
    exec-timeout 5 30
exit
!
! # SSH Version
ip ssh version 2
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 6. Save & Reload
!
end
write memory
!
reload
yes


!
!


```


---


### ISP :: (Router ISP) :: `ISP`

```py
!
! ################
! ## ROUTER ISP ##
! ################
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 1. Initialize Router:
!
enable
configure terminal
!
hostname ISP
!
banner motd #
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
             
             Fz3r0 @ Cisco CCNA/CCNP Labs
           
             Twitter : @Fz3r0_OPs
             Github  : github.com/Fz3r0
             
             Device  : ISP
             
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
#
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## 1. Configure Interface: e0/0 ISP <--> Branch LAN 
!
interface ethernet 0/0
ip address 123.123.123.2 255.255.255.252
no shutdown
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## 2. Default Route: ISP To -> LAN R1
!
ip route 0.0.0.0 0.0.0.0 123.123.123.1
exit
!
! =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
!
! ## LOOPBACK INTERFACES
!
! # ISP DNS
interface Loopback0
ip address 69.69.69.69 255.255.255.255
exit
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 9. Save & Reload
!
end
write memory
!
reload
yes


```


















## Configuration: `Switches`

### SW1 :: (Switch 1 - Core/Distribution) :: `SW1-MDF1-B1L0-F0`

````py
!
! ############################
! ## SWITCH 1 CONFIGURATION ##
! ############################
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 1. Initialize Switch:
!
enable
configure terminal
!
hostname SW1-MDF1-B1L0-F0
!
banner motd #

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

             Fz3r0 @ Cisco CCNA/CCNP Labs
           
             Twitter : @Fz3r0_OPs
             Github  : github.com/Fz3r0

             Device  : SW1-MDF1-B1L0-F0

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

#
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 2. Disable Unnecessary Services:
!
no ip domain-lookup
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 3. LAN Creation & Naming:
!
vlan 10
   name VLAN-10-ALFA
vlan 20
   name VLAN-20-BRAVO
vlan 30
   name VLAN-30-CHARLY
vlan 40
   name VLAN-40-DELTA
vlan 50
   name VLAN-50-VOICE
vlan 66
   name VLAN-66-MANAGEMENT
vlan 99
   name VLAN-99-NATIVE-TRUNK
exit
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 4. Create Access Interfaces + Assign Access & Voice VLANs:
!
interface range ethernet 0/0-3
   description VLAN-10-ALFA
      switchport mode access
      switchport access vlan 10
      switchport voice vlan 50
   no shutdown
exit
!
interface range ethernet 1/0-3
   description VLAN-20-BRAVO
      switchport mode access
      switchport access vlan 20
      switchport voice vlan 50
   no shutdown
exit
!
interface range ethernet 2/0-3
   description VLAN-30-CHARLY
      switchport mode access
      switchport access vlan 30
      switchport voice vlan 50
   no shutdown
exit
!
interface range ethernet 3/0-2
   description VLAN-40-DELTA
      switchport mode access
      switchport access vlan 40
      switchport voice vlan 50
   no shutdown
exit
!
interface ethernet 3/3
   description VLAN-66-MANAGEMENT
      switchport mode access
      switchport access vlan 66
      switchport voice vlan 50
   no shutdown
exit
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 5. Create Trunk Interfaces + Assign Native VLAN & Allowed VLANs:
!
interface range ethernet 4/0-3
   description TRUNK_LINK_(VLANS-10,20,30,40,50,66(n=99))
      switchport trunk encapsulation dot1q
      switchport mode trunk
      switchport trunk allowed vlan 10,20,30,40,50,66
      switchport trunk native vlan 99
   no shutdown
exit
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 6. Create Management SVI:
!
interface vlan 66
   description MANAGEMENT_SVI-VLAN66
   ip address 10.66.0.10 255.255.255.0
   no shutdown
exit
!
ip default-gateway 10.66.0.1
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 7. Secure Login & SSH Configuration:
!
ip domain-name Fz3r0.domain
crypto key generate rsa general-keys modulus 2048
!
username fz3r0 privilege 15 secret cisco.12345
!
enable secret fz3r0.12345
service password-encryption
security passwords min-length 10
login block-for 120 attempts 3 within 60
!
line console 0
   password fz3r0.12345
   login local
   logging synchronous
   exec-timeout 5 30
exit
!
line aux 0
   privilege level 1
   transport input none
   transport output none
   login local
   no exec
exit
!
line vty 0 4
   transport input ssh
   login local
   logging synchronous
   exec-timeout 5 30
exit
!
ip ssh version 2
!
! # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
!
! ### 9. Save & Reload
!
end
write memory
!
reload
yes


!
!

````
































````
! # :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
!
show vlan
!
! # :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
!
show interfaces trunk
!
! # :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
!
show ip interface brief
!
show running-config | include ssh
!
!
````


RT1-MDF1-B1L0-F0   - 10.66.0.1/16 (Gateway)
SW1-MDF1-B1L0-F0  - 10.66.0.10/16
SW2-IDF1-B2L1-F0   - 10.66.0.11/16
SW3-IDF2-B3L1-F0   - 10.66.0.12/16
SW4-IDF3-B3L3-F0  - 10.66.0.13/16
Fz3r0-ADMIN-PC       -  10.66.0.100/16





## Configuration: `Hosts`



RT1-MDF1-B1L0-F0   - 10.66.0.1/16 (Gateway)
SW1-MDF1-B1L0-F0  - 10.66.0.10/16
SW2-IDF1-B2L1-F0   - 10.66.0.11/16
SW3-IDF2-B3L1-F0   - 10.66.0.12/16
SW4-IDF3-B3L3-F0  - 10.66.0.13/16
Fz3r0-ADMIN-PC       -  10.66.0.100/16
























=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
                 IP ADDRESSING & VLAN TABLE
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
NETWORKS:

VLAN 10 - 10.10.0.0/16 - ALFA         
VLAN 20 - 10.20.0.0/16 - BRAVO     
VLAN 30 - 10.30.0.0/16 - CHARLY   
VLAN 40 - 10.40.0.0/16 - DELTA
VLAN 50 - 10.50.0.0/16 - VOICE                   
VLAN 66 - 10.66.0.0/16 - MANAGEMENT
VLAN 99 -  (N/A)            - NATIVE(TRUNK)
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
HOSTS:

RT1-MDF1-F0-B1L0   - 10.66.0.1/16 (Gateway)
SW1-MDF1-F0-B1L0  - 10.66.0.10/16
SW2-IDF1-F0-B2L1    - 10.66.0.11/16
SW3-IDF2-F0-B3L1    - 10.66.0.12/16
SW4-IDF3-F0-B3L3    - 10.66.0.13/16
Fz3r0-ADMIN-PC       -  10.66.0.100/16
ALFA VLAN10             - 10.10.0.101 - 254 (DHCP pool)
BRAVO VLAN20         - 10.20.0.101 - 254 (DHCP pool)
CHARLY VLAN30       - 10.30.0.101 - 254 (DHCP pool)
DELTA VLAN40          - 10.40.0.101 - 254 (DHCP pool)
VOICE (VoIP)             - 10.50.0.101 - 100 (DHCP pool)
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=





=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
       ROUTER CONFIG
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e0/0
SUB-INTERFACES (VLANS)
e0/0.10 ::  192.168.10.1/24
e0/0.20 ::  192.168.20.1/24
e0/0.30 ::  192.168.30.1/24
e0/0.40 ::  192.168.40.1/24
e0/0.50 ::  192.168.50.1/24
e0/0.66 ::  192.168.66.1/24
e0/0.99 ::  (Native / No IP)
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e0/3
123.123.123.1/30 (Edge)
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
DEFAULT ROUTE @ WAN
0.0.0.0/24 (ALL)
@ 123.123.123.2 (ISP e0/0)
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
NAT@ WAN
nat from inside (LAN)
@ e0/3 (WAN) overload
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=







=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
     ISP ROUTER CONFIG
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e0/3
123.123.123.2/30 (@ Branch)
- - - - - - - - - - - - - - - - - - - - - -
DEFAULT ROUTE @ Branch
0.0.0.0/0 (ALL)
@ 123.123.123.1 (RT1 e0/3)
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e3/3
1.0.0.1/30 (@ Internet)
- - - - - - - - - - - - - - - - - - - - - -
DEFAULT ROUTE @ Internet
0.0.0.0/0 (ALL)
@ 1.0.0.2/30 (RT1 e0/3)
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=









=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
       SWITCH CONFIG
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACES :: e4/1-3
TRUNK SWITCHPORT
Native(Untagged) : VLAN 99
Allowed(Tagged)  : VLAN 10
Allowed(Tagged)  : VLAN 20
Allowed(Tagged)  : VLAN 30
Allowed(Tagged)  : VLAN 40
Allowed(Tagged)  : VLAN 50
Allowed(Tagged)  : VLAN 66
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e0/0-3
ACCESS SWITCHPORT
Access (ALFA)     : VLAN 10
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e1/0-3
ACCESS SWITCHPORT
Access (BRAVO)  : VLAN 20
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e2/0-3
ACCESS SWITCHPORT
Access (ALFA)     : VLAN 30
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: e3/0-2
ACCESS SWITCHPORT
Access (ALFA)     : VLAN 40
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE :: ALL ACCESS
VOICE VLAN
Voice (VOICE)     : VLAN 50
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
INTERFACE     :: e3/3
ACCESS SWITCHPORT
Access (MGMT)  : VLAN 66
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
SVI (MGMT) :: VLAN 66
IP   : : 10.66.0.xxx/24
GW :: 10.66.0.1
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=











=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
       ADMINISTRATION
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
DEVICES CREDENTIALS
fz3r0
cisco.12345
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
GATEWAY
10.66.0.1/24
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
SSH
VTY 1-2
- - - - - - - - - - - - - - - - - - - - - -
Telnet = OFF
HTTP  = ON
























# 📚🗂️🎥 Resources

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





