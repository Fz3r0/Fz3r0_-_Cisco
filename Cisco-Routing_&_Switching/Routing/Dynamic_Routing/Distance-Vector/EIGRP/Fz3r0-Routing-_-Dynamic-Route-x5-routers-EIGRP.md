# 🔥🧱🛡️ Cisco: `Dynamic Routing : EIGRP @ Packet Tracer`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Dynamic Routing` `EIGRP` `Packet Tracer`

---

<br>



- 

# CCNA Lab: `Dynamic Routing` :: `EIGRP`


## Lab Files

- [Download Cisco Packet Tracer Fz3r0 Labs :: **Dynamic Route x5 Routers : `EIGRP`**]()

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







# 🚀 EIGRP @ Packet Tracer Configuration 

## 📖 **What is EIGRP?**

**Enhanced Interior Gateway Routing Protocol (EIGRP)** is a Cisco proprietary routing protocol that combines the best features of distance-vector and link-state protocols. It is a hybrid protocol, offering faster convergence and lower bandwidth usage compared to other distance-vector protocols like RIP.

### **Why Use EIGRP?** 🌐

EIGRP is used primarily in larger, more complex networks because of its ability to scale efficiently and support advanced features, such as:

- **Fast convergence**: Reacts quickly to changes in the network.
- **Efficient use of bandwidth**: EIGRP sends updates only when a change occurs, rather than periodically.
- **Supports multiple network layer protocols**: Works with both IPv4 and IPv6.
- **Automatic summarization** (in earlier versions) and **manual summarization**.

### **When to Use EIGRP?** 🏙️

EIGRP is ideal for:

- Medium to large networks with Cisco routers.
- Networks requiring fast convergence (faster than RIP).
- Scenarios where IP address summarization and route aggregation are needed.
  
### **When NOT to Use EIGRP?** 🚫

EIGRP may not be the best choice when:

- Working in multi-vendor environments (since it's Cisco proprietary).
- You need a simple, low-overhead solution (RIP might be enough).
- Extremely small networks where simpler protocols like static routing may be sufficient.

### **Real-life Examples** 🌍

- **Enterprise Networks**: Large companies using EIGRP for internal routing across multiple office locations.
- **Data Centers**: EIGRP is widely used to ensure optimal routing and fast recovery from link failures.


## EIGRP: `Autonomous System (AS)`

An Autonomous System (AS) number is a unique identifier assigned to a collection of IP networks and routers under the control of a single organization that presents a common routing policy to the internet. **The same AS number is used to group routers together so they can exchange routing information.** (_Different AS numbers would indicate that routers belong to separate routing domains, and they will not form neighbor relationships with each other._)

- **AS is used in routing protocols, such as `BGP (Border Gateway Protocol)` and `EIGRP (Enhanced Interior Gateway Routing Protocol)`, to identify and differentiate between different networks on the internet or within a private network.**

**Why Do We Always Use the Same AS Number?**

- In the case of EIGRP, all routers within a single organization or network that are participating in the same routing domain (group of routers exchanging routing information) must use the same AS number to form neighbor relationships and exchange routes.
- This consistency ensures that the routers recognize each other as part of the same routing domain and share routing information effectively.

**IMPORTANT**: If the AS number differs between routers, they won't recognize each other as part of the same network, and they won't exchange routing information.

## 🛠️ **EIGRP Configuration on the Ring Topology**

In this example, we will configure EIGRP between five routers (R1 to R5) in the ring topology, as defined in your setup.

---

### **Basic EIGRP Configuration Steps** ⚙️

1. **Enable EIGRP routing**:
   
   - Use the `router eigrp` command followed by a unique Autonomous System (AS) number. 
   - All routers in the same EIGRP network must share the same AS number.

2. **Activate EIGRP on Interfaces**:
   
   - EIGRP needs to be enabled on each router's interfaces for it to advertise routes.

3. **Network Statements**:
   
   - Define which networks should be included in EIGRP by using `network` statements that match the router's interface IP addresses.

4. **Verify the Configuration**:
   
   - After configuring, use commands like `show ip eigrp neighbors` and `show ip route` to verify that EIGRP is working properly.

---

### **EIGRP Configuration Example for Each Router** ⚡

---

#### **R1 Configuration:**

```plaintext
enable
configure terminal
hostname R1
!
router eigrp 1
network 10.1.0.0 0.0.0.3
network 10.5.0.0 0.0.0.3
network 192.168.1.0 0.0.0.255
no auto-summary
exit
!
write memory
````

#### **R2 Configuration:**



#### **R3 Configuration:**



#### **R4 Configuration:**



#### **R5 Configuration:**










# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=u6LA4fB-hDo
- https://www.youtube.com/watch?v=HP1vCvE4xU4

  
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




