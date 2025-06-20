




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



