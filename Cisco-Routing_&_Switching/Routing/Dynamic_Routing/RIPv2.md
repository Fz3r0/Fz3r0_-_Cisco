# 🔥🧱🛡️ Cisco: `Static & Default Routing @ Packet Tracer`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Static Routing` `Default Routing` `Packet Tracer`

---

<br>



- 

# CCNA Lab: `Dynamic Routing` :: `RIPv2`



## Lab Files

- [Download Cisco Packet Tracer Fz3r0 Labs :: ***]()

# `Dynamic Routing @ Packet Tracer`



## Init Config


````py
! ## ROUTER 1
!
enable
configure terminal
!
hostname R1
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.1.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.5.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.1.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````


````py
! ## ROUTER 2
!
enable
configure terminal
!
hostname R2
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.2.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.1.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.2.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````

````py
! ## ROUTER 3
!
enable
configure terminal
!
hostname R3
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.3.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.2.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.3.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````


````py
! ## ROUTER 4
!
enable
configure terminal
!
hostname R4
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.4.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.3.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.4.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````


````py
! ## ROUTER 5
!
enable
configure terminal
!
hostname R5
!
! ## WAN SIDE
!
interface fa 0/0
ip address 10.5.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.4.0.2 255.255.255.252
no shutdown
exit
!
!
! ## LAN SIDE
!
interface fa 1/1
ip address 192.168.5.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!

````









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



