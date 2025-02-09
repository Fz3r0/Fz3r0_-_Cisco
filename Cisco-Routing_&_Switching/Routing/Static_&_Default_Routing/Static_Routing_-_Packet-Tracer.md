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

![image](https://github.com/user-attachments/assets/51b9e345-3523-4954-a0c4-bb5318af2988)



## Devices Init Setup & Addressing

These initial steps configure three interconnected routers, each with two interfaces, to set up a network ready for static routing. The IPs on the router interfaces enable communication between the routers, which is essential for the next step: configuring static routes to route traffic between the local networks 192.168.1.0/24 and 192.168.2.0/24.

| Device       | Interface     | IP Address         | Subnet Mask       | Network Address with CIDR |
|--------------|---------------|--------------------|-------------------|---------------------------|
| **R1-LEFT**  | fa 0/0        | 10.1.0.1           | 255.255.255.252   | 10.1.0.0/30               |
|              | fa 0/1        | 192.168.1.1        | 255.255.255.0     | 192.168.1.0/24            |
| **R2-CENTER**| fa 0/0        | 10.1.0.2           | 255.255.255.252   | 10.1.0.0/30               |
|              | fa 0/1        | 10.2.0.2           | 255.255.255.252   | 10.2.0.0/30               |
| **R3-RIGHT** | fa 0/0        | 10.2.0.1           | 255.255.255.252   | 10.2.0.0/30               |
|              | fa 0/1        | 192.168.2.1        | 255.255.255.0     | 192.168.2.0/24            |

- **PC1 (Site A)** = 192.168.1.100/24
- **PC2 (Site B)** = 192.168.2.100/24

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



## Static Route Configuration

### Route :: From `Site A` ➡️ To `Site B`

![image](https://github.com/user-attachments/assets/9416672b-9422-4130-9494-84ff9b7d0dae)

````py
! ## ROUTER 1 (LEFT)
!
! Destination (Site B - LAN)      = 192.168.2.0/24 
! Next Hop: Static to (R2-Center) = R2 - Fa0/0 - 10.1.0.2/30 
!
enable
configure terminal
!
ip route 192.168.2.0 255.255.255.0 10.1.0.2
end
!
write memory
!




! ## ROUTER 2 (CENTER)
!
! Destination (Site A - LAN)      = 192.168.1.0/24 
! Next Hop: Static to (R1-Left)   = R1 - Fa0/0 - 10.1.0.1/30 
! ---
! Destination (Site B - LAN)      = 192.168.2.0/24 
! Next Hop: Static to (R3-Right)  = R3 - Fa0/0 - 10.2.0.1/30 
!
enable
configure terminal
!
! # Route to Site A:
ip route 192.168.1.0 255.255.255.0 10.1.0.1
!
! # Route to Site B:
ip route 192.168.2.0 255.255.255.0 10.2.0.1
end
!
write memory
!





! ## ROUTER 3 (RIGHT)
!
! Destination (Site A - LAN)      = 192.168.1.0/24 
! Next Hop: Static to (R2-Center) = R2 - Fa0/1 - 10.2.0.2/30 
!
enable
configure terminal
!
ip route 192.168.1.0 255.255.255.0 10.2.0.2
end
!
write memory
!


````


### Route :: From `R2-Center` To `Site A & Site B`

![image](https://github.com/user-attachments/assets/e7baa391-9499-4d48-a58a-192a5cc330c1)





### Route :: To `Site A` ⬅️ From `Site B`

![image](https://github.com/user-attachments/assets/859055d9-b669-4c2e-a85d-379add576e5c)






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
