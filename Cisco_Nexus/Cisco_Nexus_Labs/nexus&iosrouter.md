
Router

````py
enable
configure terminal
hostname IOS-RT-WAN

ip routing


#! Gateway Sub-Interface @ LAN
interface Ethernet0/0
   description ** Physical Link to -> NEXUS **
   no shutdown
   duplex full
 ip nat inside
exit

interface Ethernet0/0.10
   description ** V10 Sub-Interface for -> NEXUS **
   no shutdown
      encapsulation dot1Q 10
      ip address 10.10.1.254 255.255.255.0
         ip nat inside
exit

end
write memory

!
!
````

Nexus


````py
configure terminal
hostname NX9-BGW-1A
no password strength-check
username admin password admin.cisco role network-admin
cdp enable

feature interface-vlan
feature lldp


!# SVI CONFIG

vlan 10
   name VLAN10
exit
interface vlan 10
   description ** SVI-V10**
   no shutdown
      ip address 10.10.1.11/24 
exit
#! Static default route (Default Gateway) for OSPF underlay in default VRF
ip route 0.0.0.0/0 10.10.1.254

!#  TRUNK INTERFACE @ ROUTER SUB INTERFACE

interface ethernet1/3
   description ** WAN Interface Trunk @ ROUTER **
   no shutdown
   speed 10000
   duplex full
      switchport
      switchport mode trunk
      switchport trunk allowed vlan 10
exit

end
copy running-config startup-config

!
!


````

















---


real:




````
! Nexus
interface Ethernet 1/3
  description Link to Router
  speed 1000
  no negotiate auto
  no shutdown
exit

interface Ethernet 1/3.10
  description Link to TK2-WAN-R1 Fo0/2/8.48
  encapsulation dot1q 10
  vrf member vrf-inside
  ip address 10.10.1.11 255.255.255.254
  no shutdown
exit
````



````
! Router
interface Ethernet 0/0
 description Link to BGW-Fabric
 no ip address
 negotiation auto

interface Ethernet 0/0.10
 description Link to BGW-Fabric
 encapsulation dot1Q 10
 vrf forwarding vrf-router
 ip address 10.10.1.10 255.255.255.254
 no ip proxy-arp
 ip ospf network point-to-point
 ip ospf 10 area 0
 bfd interval 50 min_rx 150 multiplier 3
!

````
