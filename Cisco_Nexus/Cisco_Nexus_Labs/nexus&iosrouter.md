







Funciona!

## Router

````py
enable
configure terminal
  hostname LAB-R1

  ! VRF-10
  vrf definition VRF-10
    rd 64900:10
    address-family ipv4
    exit-address-family
  !

  interface Ethernet0/0
    description *** Link → LAB-BGW1 Eth1/3 ***
    duplex full
    no shutdown
  exit

  interface Ethernet0/0.48
    no shutdown
    description *** VLAN48 → LAB-BGW1 ***
    encapsulation dot1Q 48
    vrf forwarding VRF-10
    ip address 10.10.1.10 255.255.255.254
  exit

  router ospf 10 vrf VRF-10
    router-id 10.10.1.10
    network 10.10.1.10 0.0.0.1 area 0
  exit

end
write memory

!
!



````

````
# router
show interface Ethernet0/0.48
!
ping vrf VRF-10 10.10.1.11 source 10.10.1.10
!
ping vrf VRF-10 10.10.1.11
!
show ip ospf neighbor
!
show ip interface brief

````

## Nexus

````py
configure terminal
  hostname LAB-BGW1

  !# INSIDE1 
  vrf context INSIDE1
    address-family ipv4 unicast
  exit

  feature ospf

  interface Ethernet1/3
    description *** Link → LAB-R1 Eth0/0 ***
    no switchport
    no shutdown
  exit

  interface Ethernet1/3.48
    description VLAN48 → LAB-R1
    no shutdown
    encapsulation dot1q 48
    vrf member INSIDE1
    ip address 10.10.1.11/31
    ip router ospf 10 area 0
  exit

  router ospf 10
    vrf INSIDE1
      router-id 10.10.1.11
    exit
  exit

end
copy running-config startup-config

!
!


````

````
# BGW
show ip ospf neighbor vrf INSIDE1
ping 10.10.1.10 vrf INSIDE1
````












---






## ROUTER SIMPLIFICADO

````
enable
configure terminal
  hostname LAB-R1

  interface Ethernet 0/0
    description *** Link → LAB-BGW1 Eth1/3 ***
    no shutdown
  exit

  interface Ethernet0/0.48
    description *** VLAN48 punto-a-punto ***
    encapsulation dot1Q 48
    ip address 10.10.1.10 255.255.255.254
  exit

  router ospf 10
    router-id 10.10.1.10
    network 10.10.1.10 0.0.0.1 area 0
  exit
end
write memory
````

## NEXUS
