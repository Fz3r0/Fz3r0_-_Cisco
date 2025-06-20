




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
      ip address 10.10.1.210 255.255.255.254
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

feature lldp



!#  TRUNK INTERFACE @ ROUTER SUB INTERFACE

#! Sub-Interface @ ROUTER
interface Ethernet1/3
   description ** Physical Link to -> ROUTER **
   no shutdown
   duplex full
 ip nat inside
exit

interface Ethernet1/3.11
   description ** V10 Sub-Interface for -> NEXUS **
   no shutdown
      encapsulation dot1Q 11
      ip address 10.10.1.211 255.255.255.254
         ip nat inside
exit




end
copy running-config startup-config

!
!


````



