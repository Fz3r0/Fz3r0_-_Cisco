# 🔥🧱🛡️ Cisco: `Dynamic Routing : RIPv2 @ Packet Tracer`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Dynamic Routing` `RIPv2` `Packet Tracer`

---

<br>



- 

# CCNA Lab: `Dynamic Routing` :: `RIPv2`

Configuring **RIP v2** is useful for learning dynamic routing in small networks. However, for larger, more complex networks, protocols like **OSPF or EIGRP** are preferred due to their scalability and efficiency.

- **RIPv2 operates by simply advertising its own networks. In this scenario, each router has three different networks—one for each interface (one LAN and two WAN). RIPv2 sends advertisements of these networks to its neighboring routers, which then propagate the updates to their neighbors, and so on, forming a continuous update process. This propagation continues until reaching the maximum limit of 15 hops, as any route with 16 hops is considered unreachable.**

## Lab Files

- [Download Cisco Packet Tracer Fz3r0 Labs :: ***]()

# `Dynamic Routing @ Packet Tracer`

## Dynamic Routing: Topology

This setup consists of **five routers** (R1 to R5) connected in a **ring topology**, each with:

- **Two WAN interfaces** using `/30 subnets` (point-to-point links between routers).
- **One LAN interface** using `/24 subnets` (local network per router).
- **Each LAN** have a PC for testing purposes. 

![image](https://github.com/user-attachments/assets/b69ec954-6c59-462b-a89d-6539668b0b2d)

## 📋 **IP Addressing Table**

| Device  | Interface | IP Address   | Subnet Mask      | Network Address with CIDR |
|---------|-----------|--------------|------------------|---------------------------|
| **R1**  | Fa0/0    | 10.1.0.1      | 255.255.255.252  | 10.1.0.0/30               |
|         | Fa0/1    | 10.5.0.2      | 255.255.255.252  | 10.5.0.0/30               |
|         | Fa1/1    | 192.168.1.1   | 255.255.255.0    | 192.168.1.0/24            |
| **R2**  | Fa0/0    | 10.2.0.1      | 255.255.255.252  | 10.2.0.0/30               |
|         | Fa0/1    | 10.1.0.2      | 255.255.255.252  | 10.1.0.0/30               |
|         | Fa1/1    | 192.168.2.1   | 255.255.255.0    | 192.168.2.0/24            |
| **R3**  | Fa0/0    | 10.3.0.1      | 255.255.255.252  | 10.3.0.0/30               |
|         | Fa0/1    | 10.2.0.2      | 255.255.255.252  | 10.2.0.0/30               |
|         | Fa1/1    | 192.168.3.1   | 255.255.255.0    | 192.168.3.0/24            |
| **R4**  | Fa0/0    | 10.4.0.1      | 255.255.255.252  | 10.4.0.0/30               |
|         | Fa0/1    | 10.3.0.2      | 255.255.255.252  | 10.3.0.0/30               |
|         | Fa1/1    | 192.168.4.1   | 255.255.255.0    | 192.168.4.0/24            |
| **R5**  | Fa0/0    | 10.5.0.1      | 255.255.255.252  | 10.5.0.0/30               |
|         | Fa0/1    | 10.4.0.2      | 255.255.255.252  | 10.4.0.0/30               |
|         | Fa1/1    | 192.168.5.1   | 255.255.255.0    | 192.168.5.0/24            |

- `PC-1 (Site-A)` :: 192.168.1.100/24
- `PC-2 (Site-B)` :: 192.168.2.100/24
- `PC-3 (Site-C)` :: 192.168.3.100/24
- `PC-4 (Site-D)` :: 192.168.4.100/24
- `PC-5 (Site-E)` :: 192.168.5.100/24
  
---

## Init Config


### Init Setup: `Router 1`

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

### Init Setup: `Router 2`

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

### Init Setup: `Router 3`

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

### Init Setup: `Router 4`

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

### Init Setup: `Router 5`

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







# 📌 Configuring RIP v2 in Packet Tracer

## 🔹 Introduction to RIP v2

The **Routing Information Protocol (RIP)** is a dynamic routing protocol that uses **hop count** as its metric to determine the best path. There are two main versions:

1. **RIP v1:** (_Legacy_) Uses **classful routing**, meaning it does not support **VLSM (Variable Length Subnet Masking)** or **CIDR (Classless Inter-Domain Routing)**.
2. **RIP v2:** Uses **classless routing**, which supports **CIDR**, **VLSM**, and sends subnet masks with routing updates.

**IMPORTANT**:  RIP v1 Is **No Longer Used** because not support **subnet masks**, making it incompatible with modern networks, Does not support **route summarization**, Broadcast-based updates consume unnecessary bandwidth, and No support for **authentication**, making it vulnerable to attacks.

### ✅ When to Use RIP v2

- **Small networks** with a limited number of routers.
- **Lab environments** for learning basic dynamic routing.
- **Backup routing** when using another protocol as the primary.

### ❌ When Not to Use RIP v2

- **Large networks** due to its **15-hop limit**.
- Networks requiring **fast convergence**, as RIP has slow convergence.
- When using modern **link-state protocols** like **OSPF** or **EIGRP**, which provide better scalability and efficiency.

## 🔹 How RIP v2 Works in This Topology

With **five routers** connected in a ring topology, RIP v2 will:

1. Exchange routing updates every **30 seconds**.
2. Use **hop count** as the only metric (lower is better).
3. If multiple paths exist, the **path with the lowest hop count** will be preferred.
4. The **maximum number of hops is 15**—any route with **16 hops is considered unreachable**.
5. Subnet masks will be included in updates (unlike RIP v1), allowing for **CIDR and VLSM**.

## 🛠 **Configuring RIP v2 on Each Router**

RIPv2 operates by simply advertising its own networks. In this scenario, each router has three different networks—one for each interface (one LAN and two WAN). RIPv2 sends advertisements of these networks to its neighboring routers, which then propagate the updates to their neighbors, and so on, forming a continuous update process. This propagation continues until reaching the maximum limit of 15 hops, as any route with 16 hops is considered unreachable.

### IMPORTANT!!!: `No Auto-summarization`

Auto-summarization in RIP automatically summarizes routes to their classful network address when sending updates. This can cause issues in networks with discontiguous subnets, where networks of the same class are split across multiple locations without a direct connection.

**If no auto-summary is not used, RIP will:**

- **Summarize routes to their classful boundaries (e.g., 10.1.0.0/8 instead of 10.1.0.0/30).**
- Cause incorrect routing because **routers might not know the exact subnet mask**.
- Forward traffic incorrectly or drop packets due to missing subnet information.

**Example of the Problem** 

- Imagine R1 advertises 10.1.0.0/30 to R2, but R2 summarizes it as 10.0.0.0/8. If R3 has a different 10.x.x.x subnet, R2 may think it belongs to the same large network and misroute traffic.

The Best Practice is **Always use `no auto-summary`, This ensures RIP advertises specific subnet masks instead of summarizing to classful boundaries, preventing routing errors.:

### RIP v2 Config: `Router-1`

```py
! ## ROUTER 1 : RIPv2
!
enable
configure terminal
!
! ## Enable RIP and Set Version 2
router rip
version 2
!
! ## Advertise Networks
! ## WANs(Fa0/0,0/1) LAN(Fa1/1)
network 10.1.0.0
network 10.5.0.0
network 192.168.1.0
!
! ## Disable Auto-Summarization
!  # Since we are using >discontiguous networks<, disable auto-summary to avoid incorrect routing:
no auto-summary
!
! ## Save & Check Configs
end
write memory
!
show ip route rip
!

```


### RIP v2 Config: `Router-2`

```py
! ## ROUTER 2 : RIPv2
!
enable
configure terminal
!
! ## Enable RIP and Set Version 2
router rip
version 2
!
! ## Advertise Networks
! ## WANs(Fa0/0,0/1) LAN(Fa1/1)
network 10.2.0.0
network 10.1.0.0
network 192.168.2.0
!
! ## Disable Auto-Summarization
!  # Since we are using >discontiguous networks<, disable auto-summary to avoid incorrect routing:
no auto-summary
!
! ## Save & Check Configs
end
write memory
!
show ip route rip
!

```



### RIP v2 Config: `Router-3`

```py
! ## ROUTER 3 : RIPv2
!
enable
configure terminal
!
! ## Enable RIP and Set Version 2
router rip
version 2
!
! ## Advertise Networks
! ## WANs(Fa0/0,0/1) LAN(Fa1/1)
network 10.3.0.0
network 10.2.0.0
network 192.168.3.0
!
! ## Disable Auto-Summarization
!  # Since we are using >discontiguous networks<, disable auto-summary to avoid incorrect routing:
no auto-summary
!
! ## Save & Check Configs
end
write memory
!
show ip route rip
!

```



### RIP v2 Config: `Router-4`

```py
! ## ROUTER 4 : RIPv2
!
enable
configure terminal
!
! ## Enable RIP and Set Version 2
router rip
version 2
!
! ## Advertise Networks
! ## WANs(Fa0/0,0/1) LAN(Fa1/1)
network 10.4.0.0
network 10.3.0.0
network 192.168.4.0
!
! ## Disable Auto-Summarization
!  # Since we are using >discontiguous networks<, disable auto-summary to avoid incorrect routing:
no auto-summary
!
! ## Save & Check Configs
end
write memory
!
show ip route rip
!

```



### RIP v2 Config: `Router-5`

```py
! ## ROUTER 5 : RIPv2
!
enable
configure terminal
!
! ## Enable RIP and Set Version 2
router rip
version 2
!
! ## Advertise Networks
! ## WANs(Fa0/0,0/1) LAN(Fa1/1)
network 10.5.0.0
network 10.4.0.0
network 192.168.5.0
!
! ## Disable Auto-Summarization
!  # Since we are using >discontiguous networks<, disable auto-summary to avoid incorrect routing:
no auto-summary
!
! ## Save & Check Configs
end
write memory
!
show ip route rip
!

```




## RIP v2: Validation

```
# This should display learned routes from other routers.
show ip route rip

# To check RIP updates being sent:
debug ip rip
```

















# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=u6LA4fB-hDo

  
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



