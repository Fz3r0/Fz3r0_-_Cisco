


## SWL3-CORE-1

````py
!
!

enable
terminal width 500
configure terminal
hostname SWL3-CORE-1
lldp run

! #####################################################################################
! # VLANs                                                                             #
! #####################################################################################

!# VLAN
vlan 10
   name V10-BLUE
exit
vlan 20
   name V10-RED
exit
vlan 30
   name V10-GREEN
exit
!
vlan 40 
   name V40-VOIP
exit
!
vlan 50 
   name V50-WIRELESS
exit
!
vlan 66 
   name V66-MANAGEMENT
exit
!

! #####################################################################################
! # SWITCH VIRTUAL INTERFACES (SVI) - L3 LAN GATEWAYS                                 #
! #####################################################################################

! # SVI - VLAN 1 DEFAULT SHUTDOWN (for security)
interface Vlan1
   no ip address
   shutdown
exit

! # SVI - VLAN 10 (/23 = 510 hosts)
interface Vlan10
   description *** V10-BLUE - GATEWAY ***
   ip address 10.10.10.1 255.255.254.0
   no shutdown
exit
!
! # SVI - VLAN 20 (/23 = 510 hosts)
interface Vlan20
   description *** V20-RED - GATEWAY ***
   ip address 10.10.20.1 255.255.254.0
   no shutdown
exit
!
! # SVI - VLAN 30 (/23 = 510 hosts)
interface Vlan30
   description *** V30-GREEN - GATEWAY ***
   ip address 10.10.30.1 255.255.254.0
   no shutdown
exit

! # SVI - VLAN 40 (VOIP) (/23 = 510 hosts)
interface Vlan40
   description *** V40-VOIP-PHONES - GATEWAY ***
   ip address 10.10.40.1 255.255.254.0
   no shutdown
exit

! # SVI - VLAN 50 (WIRELESS) (/23 = 510 hosts)
interface Vlan50
   description *** V50-WIFI-WIRELESS - GATEWAY ***
   ip address 10.10.50.1 255.255.254.0
   no shutdown
exit

! # SVI - VLAN 66 (MANAGEMENT) (/23 = 510 hosts)
interface Vlan66
   description *** V66-MANAGEMENT - GATEWAY ***
   ip address 10.10.66.1 255.255.254.0
   no shutdown
exit

! #####################################################################################
! # DOMAIN & DNS                                                                      #
! #####################################################################################

! # DOMAIN = fz3r0.com
ip domain-name fz3r0.com

! # DNS 1 - Google DNS 
ip name-server 6.6.6.6
! # DNS 2 - Google DNS 2
ip name-server 6.6.3.3

! # Disable DNS lookup (For labs: Avoid delays when typing invalid commands)
no ip domain-lookup

 ######################################################################################
! # DEFAULT GATEWAY & IP ROUTE                                                        #
! #####################################################################################

! # L2 : Default Gateway @ GW - NOT NEEDED IN CORE, WE ARE ALREADY THE GATEWAY!
!ip default-gateway 10.10.66.1

! # L3 : Default Route @ GW->WAN - NOT NEEDED IN CORE, WE ARE ALREADY THE GATEWAY!
!ip route 0.0.0.0 0.0.0.0 10.10.66.1

! #####################################################################################
! #  ENABLE IP ROUTING                                                                #
! #####################################################################################

!# (For L3 Switches) Enables the router to forward packets BETWEEN DIFFERENT NETWORKS (turns on Layer 3 routing between management & default VRFs) - InterVLAN Routing
ip routing

! #####################################################################################
! #  Interfaces                                                                #
! #####################################################################################

interface range Ethernet0/0-1
   description ** TRUNK ALLOW ALL **
   no shutdown
   switchport trunk encapsulation dot1q
   switchport mode trunk
exit
!

interface Ethernet 0/2
   description ** ACESS WIRLESS MAYBE BAD **
   no shutdown
   switchport mode access 
   switchport access vlan 50
exit

! #####################################################################################
! #  Save                                                               #
! #####################################################################################


end
wr

!
!

````







## SWL2-MEETING-ROOM

````py
!
!

enable
terminal width 500
configure terminal
hostname SWL2-MEETING-ROOM
lldp run

! #####################################################################################
! # VLANs                                                                             #
! #####################################################################################

!# VLAN
vlan 10
   name V10-BLUE
exit
vlan 20
   name V10-RED
exit
vlan 30
   name V10-GREEN
exit
!
vlan 40 
   name V40-VOIP
exit
!
vlan 50 
   name V50-WIRELESS
exit
!
vlan 66 
   name V66-MANAGEMENT
exit
!

! #####################################################################################
! # SWITCH VIRTUAL INTERFACES (SVI) - L3 LAN GATEWAYS                                 #
! #####################################################################################

! # SVI - VLAN 1 DEFAULT SHUTDOWN (for security)
interface Vlan1
   no ip address
   shutdown
exit

! # SVI - VLAN 10 
interface Vlan10
   description *** V10-BLUE - L2 ***
   no ip address
   no shutdown
exit
!
! # SVI - VLAN 20
interface Vlan20
   description *** V20-RED - L2 ***
   no ip address
   no shutdown
exit
!
! # SVI - VLAN 30
interface Vlan30
   description *** V30-GREEN - L2 ***
   no ip address
   no shutdown
exit

! # SVI - VLAN 40 
interface Vlan40
   description *** V40-VOIP-PHONES - L2 ***
   no ip address
   no shutdown
exit

! # SVI - VLAN 50 
interface Vlan50
   description *** V50-WIFI-WIRELESS - L2 ***
   no ip address
   no shutdown
exit

! # SVI - VLAN 66 
interface Vlan66
   description *** V66-MANAGEMENT - L2 ***
   ip address 10.10.66.10 255.255.254.0
   no shutdown
exit

! #####################################################################################
! # DOMAIN & DNS                                                                      #
! #####################################################################################

! # DOMAIN = fz3r0.com
ip domain-name fz3r0.com

! # DNS 1 - Google DNS 
ip name-server 6.6.6.6
! # DNS 2 - Google DNS 2
ip name-server 6.6.3.3

! # Disable DNS lookup (For labs: Avoid delays when typing invalid commands)
no ip domain-lookup

 ######################################################################################
! # DEFAULT GATEWAY & IP ROUTE                                                        #
! #####################################################################################

! # L2 : Default Gateway @ GW - NOT NEEDED IN CORE, WE ARE ALREADY THE GATEWAY!
ip default-gateway 10.10.66.1

! # L3 : Default Route @ GW->WAN - NOT NEEDED IN CORE, WE ARE ALREADY THE GATEWAY!
ip route 0.0.0.0 0.0.0.0 10.10.66.1

! #####################################################################################
! #  Interfaces                                                                #
! #####################################################################################

interface range Ethernet0/0-1
   description ** TRUNK ALLOW ALL **
   no shutdown
   switchport trunk encapsulation dot1q
   switchport mode trunk
exit

interface range Ethernet0/2-3
   description ** ACCESS WIRLESS MAYBE BAD **
   no shutdown
   switchport mode access 
   switchport access vlan 50
exit

! #####################################################################################
! #  Save                                                               #
! #####################################################################################


end
wr

!
!

````






## Aruba AP Brdged mode (trunk)

````py

!
!

configure terminal
hostname AP-BRIDGE-ARUBA
lldp run

! #####################################################################################
! # DOMAIN & DNS                                                                      #
! #####################################################################################

ip dns domain-name fz3r0.com
ip dns server-address 6.6.6.6
ip dns server-address 6.6.3.3
ip route 0.0.0.0/0 10.10.66.1

! #####################################################################################
! # VLANs                                                                             #
! #####################################################################################

!# VLAN
vlan 10
   name V10-BLUE
exit
vlan 20
   name V10-RED
exit
vlan 30
   name V10-GREEN
exit
!
vlan 40 
   name V40-VOIP
exit
!
vlan 50 
   name V50-WIRELESS
exit
!
vlan 66 
   name V66-MANAGEMENT
exit
!


! #####################################################################################
! # SWITCH VIRTUAL INTERFACES (SVI) - L3 LAN GATEWAYS                                 #
! #####################################################################################

interface vlan 66
   description *** V66-MANAGEMENT L2 *** 
   ip address 10.10.66.98/23
exit


! #####################################################################################
! #  Interfaces                                                                #
! #####################################################################################

interface  1/1/1
   description *** TRUNK ALL VLANS ***
      no routing
         vlan trunk allowed all
         vlan trunk native 1
exit

interface  1/1/2
   description *** TRUNK ALL VLANS ***
      no routing
         vlan trunk allowed all
         vlan trunk native 1
exit

interface  1/1/3
   description *** ACCESS VLAN 10 ***
      no routing
         vlan access 10
exit

interface  1/1/4
   description *** ACCESS VLAN 20 ***
      no routing
         vlan access 20
exit

interface  1/1/5
   description *** ACCESS VLAN 30 ***
      no routing
         vlan access 30
exit

interface  1/1/6
   description *** ACCESS VLAN 40 ***
      no routing
         vlan access 30
exit

interface  1/1/7
   description *** ACCESS VLAN 50 ***
      no routing
         vlan access 30
exit

interface  1/1/8
   description *** ACCESS VLAN 60 ***
      no routing
         vlan access 30
exit

exit
write memory


!
!


````











# WLAN CLIENTS

L3 SIMULATION FOR WIRELESS CLIENT IN DIFFERENT SSIDS: 

## CLIENT-STA-1

````py
!
enable
configure terminal
lldp run

hostname CLIENT-STA-1

no ip domain-lookup

! # Configure interface as if it's the PC's network card
interface Ethernet 0/0
   description ** WIRELESS STA **
   ip address 10.10.10.101 255.255.254.0
   duplex full
   no shutdown
exit

! # Set a default route pointing to the gateway (usually your lab router)
ip route 0.0.0.0 0.0.0.0 10.10.10.1

! # Exit & Save
end
write memory


!
!



````





