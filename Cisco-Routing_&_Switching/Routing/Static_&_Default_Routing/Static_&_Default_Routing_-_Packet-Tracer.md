# 🔥🧱🛡️ Cisco: `Static & Default Routing @ Packet Tracer`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Static Routing` `Default Routing` `Packet Tracer`

---

<br>



- 

# CCNA Lab: `Static Routing` & `Default Routing` @ Packet Tracer

This lab explores the fundamentals of static and default routing using Cisco Packet Tracer. Through a step-by-step approach, we configure routers to establish communication between different network segments. 

The goal is to understand how static routes define precise paths for packet forwarding and how default routes simplify network management.

## Lab Files

- [Download Cisco Packet Tracer Fz3r0 Labs :: **Static_&_Default_Routing_-_Packet-Tracer (.ZIP)**](https://github.com/Fz3r0/Fz3r0_-_Cisco/blob/main/Cisco-Routing_%26_Switching/Routing/Static_%26_Default_Routing/Static_%26_Default_Routing_-_Packet-Tracer(PT_Files).zip)

# `Static Routing @ Packet Tracer`

Static routing is a manual method of defining network paths. It provides complete control over traffic flow, making it ideal for small, predictable network environments. 

## Static Routing: Topology 

In this lab, we manually configure static routes on three routers to ensure communication between Site A (192.168.1.0/24) and Site B (192.168.2.0/24), with Router 2 acting as the central hub.

![image](https://github.com/user-attachments/assets/51b9e345-3523-4954-a0c4-bb5318af2988)



## Static Routing: Devices Init Setup & Addressing

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



## Static Routing: Static Route Configuration

In this lab, we are configuring static routes between three routers to establish communication between `Site A` <<->> `Site B`, and the `R2-Center` router, which serves as the intermediary. The routers are connected through specific interfaces, and static routing is manually configured to define the exact path for each packet to take based on its destination network.

- **Static routes are particularly useful in small networks, like this one, where the network topology is fixed, and we want precise control over the routing process. Each router is configured with routes to the other sites using the next-hop IP addresses for the appropriate routers. This ensures proper packet delivery between sites.**

### Route :: From `Site A` ➡️ To `Site B`

Router 1 (R1-LEFT) to Router 2 (R2-CENTER) using the IP 10.1.0.2 on interface Fa0/0 as the next hop.

![image](https://github.com/user-attachments/assets/9416672b-9422-4130-9494-84ff9b7d0dae)

````py
! ## ROUTER 1 (LEFT)
!
! # Destination (Site B - LAN)      = 192.168.2.0/24 
! # Next Hop: Static to (R2-Center) = R2 - Fa0/0 - 10.1.0.2/30 
!
enable
configure terminal
!
ip route 192.168.2.0 255.255.255.0 10.1.0.2
end
!
write memory
!

````


### Route :: From `R2-Center` To `Site A & Site B`

Router 2 (R2-CENTER) has two static routes:

- To Site A (using 10.1.0.1 on Router 1)
- To Site B (using 10.2.0.1 on Router 3)

![image](https://github.com/user-attachments/assets/e7baa391-9499-4d48-a58a-192a5cc330c1)

````py
! ## ROUTER 2 (CENTER)
!
! # Destination (Site A - LAN)      = 192.168.1.0/24 
! # Next Hop: Static to (R1-Left)   = R1 - Fa0/0 - 10.1.0.1/30 
! ---
! # Destination (Site B - LAN)      = 192.168.2.0/24 
! # Next Hop: Static to (R3-Right)  = R3 - Fa0/0 - 10.2.0.1/30 
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

````


### Route :: To `Site A` ⬅️ From `Site B`

Router 3 (R3-RIGHT) to Router 2 (R2-CENTER) using the IP 10.2.0.2 on interface Fa0/1 as the next hop.

![image](https://github.com/user-attachments/assets/859055d9-b669-4c2e-a85d-379add576e5c)

````py
! ## ROUTER 3 (RIGHT)
!
! # Destination (Site A - LAN)      = 192.168.1.0/24 
! # Next Hop: Static to (R2-Center) = R2 - Fa0/1 - 10.2.0.2/30 
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

## Static Routing: Validations

To verify the routing configuration and ensure connectivity between the sites, we will use various commands on the routers and perform ping and traceroute tests from PC-A and PC-B.

### Router Validation

![image](https://github.com/user-attachments/assets/008c265e-d367-42e1-8630-783bad09e9c7)

````sh
! ## Validation Commands on Router:
!
! Displays the router's routing table, showing all routes and their destinations.
show ip route
!
! Provides a quick overview of the router's interfaces, their IP addresses, and their status (up or down).
show ip interface brief
!

````

### PC-1 (Site-A) & PC-2 (Site-B) Validation

![image](https://github.com/user-attachments/assets/7380008c-27d4-4d71-80ef-2d0b0ca2b106)

- PC-1 (Site-A):

````sh
ping 192.168.2.100
tracert 192.168.2.100
````

- PC-2 (Site-B):

````sh
ping 192.168.1.100
tracert 192.168.1.100
````







# `Default Routing @ Packet Tracer`

Default routing is used when a router does not have a specific route for a destination, for example a simple route from "Site-XYZ" to the Internet. It forwards all unknown traffic to a designated next-hop router. This method is beneficial for simplifying routing in networks with a single exit point, reducing the need for multiple static routes. 

## Default Routing: Topology 

In this lab, we implement a default route on the edge routers to direct all traffic through the central router.

In this setup, Router 2 (R2-CENTER) acts as the core router handling static routes for Site A and Site B, while Router 1 (R1-LEFT) and Router 3 (R3-RIGHT) use a default route (0.0.0.0/0) to send all unknown traffic to R2, simulating a typical internet gateway scenario. This configuration simplifies routing for edge devices, as they rely on a single route for external connectivity.

![image](https://github.com/user-attachments/assets/29b4a3b8-0cfd-478f-94c3-9c6b9acbbe84)

## Default Routing: Default Route Configuration

### Route :: From `R2-Center` To `Site A & Site B`

_This is the simulation of "Internet"_

![image](https://github.com/user-attachments/assets/63579a53-3696-4795-bcce-244d24fd1bbe)

````py
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

````


### Route :: From `Site A` ➡️ To `ANY 0.0.0.0/0`

![image](https://github.com/user-attachments/assets/6e70311a-34d6-428b-be8e-9fd80dd20e12)


````py
! ## ROUTER 1 (LEFT)
!
! Destination = ANY               = 0.0.0.0/0
! Next Hop: Static to (R2-Center) = R2 - Fa0/0 - 10.1.0.2/30 
!
enable
configure terminal
!
ip route 0.0.0.0 0.0.0.0 10.1.0.2
end
!
write memory
!

````



### Route :: From `Site B` ➡️ To `ANY 0.0.0.0/0`

![image](https://github.com/user-attachments/assets/918e4724-fcf3-4ea1-9e8d-81a66fdf6e58)

````py
! ## ROUTER 3 (RIGHT)
!
! Destination = ANY               = 0.0.0.0/0
! Next Hop: Static to (R2-Center) = R2 - Fa0/1 - 10.2.0.2/30 
!
enable
configure terminal
!
ip route 0.0.0.0 0.0.0.0 10.2.0.2
end
!
write memory
!

````


## Default Routing: Validations

To verify the routing configuration and ensure connectivity between the sites, we will use various commands on the routers and perform ping and traceroute tests from PC-A and PC-B.

### Router Validation

![image](https://github.com/user-attachments/assets/91a19beb-460a-42ac-99b6-63f30829f11b)

````sh
! ## Validation Commands on Router:
!
! Displays the router's routing table, showing all routes and their destinations.
show ip route
!
! Provides a quick overview of the router's interfaces, their IP addresses, and their status (up or down).
show ip interface brief
!

````

### PC-1 (Site-A) & PC-2 (Site-B) Validation

![image](https://github.com/user-attachments/assets/c95a92cd-9298-4c59-a7da-1841587fdafc)

- PC-1 (Site-A):

````sh
ping 192.168.2.100
tracert 192.168.2.100
````

- PC-2 (Site-B):

````sh
ping 192.168.1.100
tracert 192.168.1.100
````









# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=7jJylCfmVvQ
- https://www.youtube.com/watch?v=wfQyLosNTVo


  
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



