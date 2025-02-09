# 🔥🧱🛡️ Cisco: `Static Routing @ Packet Tracer`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Static Routing` `Packet Tracer`

---

<br>



- 

# `Static Routing @ Packet Tracer`

## Topology 

![image](https://github.com/user-attachments/assets/6f712314-9a24-4a81-868c-3768fe7c2bb1)


## Devices & Addressing

These initial steps configure three interconnected routers, each with two interfaces, to set up a network ready for static routing. The IPs on the router interfaces enable communication between the routers, which is essential for the next step: configuring static routes to route traffic between the local networks 192.168.1.0/24 and 192.168.2.0/24.

| Device       | Interface     | IP Address         | Subnet Mask       | Network Address with CIDR |
|--------------|---------------|--------------------|-------------------|---------------------------|
| **R1-LEFT**  | fa 0/0        | 10.1.0.1           | 255.255.255.252   | 10.1.0.0/30               |
|              | fa 0/1        | 192.168.1.1        | 255.255.255.0     | 192.168.1.0/24            |
| **R2-CENTER**| fa 0/0        | 10.1.0.2           | 255.255.255.252   | 10.1.0.0/30               |
|              | fa 0/1        | 10.2.0.2           | 255.255.255.252   | 10.2.0.0/30               |
| **R3-RIGHT** | fa 0/0        | 10.2.0.1           | 255.255.255.252   | 10.2.0.0/30               |
|              | fa 0/1        | 192.168.2.1        | 255.255.255.0     | 192.168.2.0/24            |

### Router-1 Config: 

````py
! ## ROUTER 1 (LEFT)
!
enable
configure terminal
!
hostname R1-LEFT
!
interface fa 0/0
ip address 10.1.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!


````

### Router-2 Config: 

````py
! ## ROUTER 2 (CENTER)
!
enable
configure terminal
!
hostname R2-CENTER
!
interface fa 0/0
ip address 10.1.0.2 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 10.2.0.2 255.255.255.252
no shutdown
end
!
write memory
!
show ip interface brief
!


````

### Router-3 Config: 

````py
! ## ROUTER 3 (RIGHT)
!
enable
configure terminal
!
hostname R3-RIGHT
!
interface fa 0/0
ip address 10.2.0.1 255.255.255.252
no shutdown
exit
!
interface fa 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
end
!
write memory
!
show ip interface brief
!


````

# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=7jJylCfmVvQ



  
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





















These initial steps configure three interconnected routers, each with two interfaces, to set up a network ready for static routing. The IPs on the router interfaces enable communication between the routers, which is essential for the next step: configuring static routes to route traffic between the local networks 192.168.1.0/24 and 192.168.2.0/24.
- https://www.youtube.com/watch?v=wfQyLosNTVo
