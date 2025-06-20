




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



