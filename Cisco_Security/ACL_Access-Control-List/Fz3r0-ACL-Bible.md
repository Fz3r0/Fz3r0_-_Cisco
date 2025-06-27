# 🧠🏗️🌐 Cisco Nexus: `NX-OS - VXLAN/EVPN @ Tier 3 Fabric - PT1 : OSPF Underlay + Essentials`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `CCNA` `CCNP` `ACL` `Access Control List` `Standard ACL` `Extended ACL` `Numbered ACL` `Named ACL` `ip access-list` `access-list` `access-class` `VTY ACL` `Telnet ACL` `SSH ACL` `VTY Lines` `Firewall` `L3 Filter` `Packet Filtering` `Permit` `Deny` `Wildcard Mask` `host` `any` `eq` `range` `icmp` `tcp` `udp` `ACL Logging` `log` `show access-lists` `debug ip packet` `debug ip icmp` `undebug all` `ip access-group` `inbound` `outbound` `ip policy route-map` `route-map` `match ip address` `set ip next-hop` `Policy-Based Routing` `PBR` `QoS Classification` `Class-map` `Control Plane Policing` `CoPP` `NAT` `ip nat inside source` `PAT` `Overload` `Redistribution Filtering` `OSPF ACL` `BGP ACL` `route filtering` `deny ip any any` `permit ip any any` `Interface ACL` `Traffic Control` `Security` `Edge Filtering` `Core Filtering` `Router Security` `Switch Security` `Cisco IOS` `CCNA` `CCNP` `Cisco Firewall Basics` `Network Access Control` `Routing Security` `Access Filtering` `IPv4 ACL` `IPv6 ACL` `Cisco Labs` `Network Simulator` `Packet Tracer`

---

<br>

# 📄 `Index`



## ACL

2 propositos basicos: 

1. filtrado - (mas utilizadas) permitir, negar , resgtingir acceso a algo
2. clasificacion - la sque se usan en conjunto por eemplo con Qos dond elas ACL clasifican os eeleciconan redes, rangos, o IPs par aclasicicar, igual como para rutas en BGP OSPf

## Filtrado

- enunciados o sentiencias que segun su config pemrioten o niegan pquetes de atravesar un router
- Pueden tomart desiciones basadas en paqietes (osi layer 3 network = direcciones IP) o segmentos (osi layer 4 trasnporte = puertos) o tomart desicines em ambos 3 y 4

En caso de ser layer 3 IP, se pueden diltrar por:

- ip, rango, subred, red, o superred. 

En caso de ser layer 4, por

- puertos 1 al 65535, (TCP/UDP)

### ACL = la lista

como su nombre lo dice en una lista y lleva uin order por ejemplo

1. permitir puerto l4 80 (http)
2. permitir puerto l4 25 (SMPT)
3. permitir la IP X
4. negar acceso a la red 192.168.1.0
5. Permitir acceso a telnet solo a la IPx
6. etc
7. etc

Todas esas son siderentes sencencias que tienes diretepntes propositos, y todos son parte de una misma lista, o una misma ACL.,






## 🔍 Cisco ACL Types – Unified Overview

| 🔖 **ACL Type**            | 🧠 **Description**                                                                                                                                     | 🎯 **Match Criteria**                 | 🛠️ **Application Scope**                    | 💡 **Example**                               |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|---------------------------------------------|----------------------------------------------|
| `Standard ACL`            | Filters **only by source IP address**. Best used **closest to the destination**. Very common.                                                         | Source IP only                       | NAT, VTY access, basic filtering             | `access-list 1 permit 10.10.0.0 0.0.0.255`    |
| `Extended ACL`            | Matches **source, destination, protocol, and ports**. Should be placed **closest to the source**. Most flexible and widely used.                      | Src/Dst IP, Protocol, Port           | Traffic filtering, firewall, QoS, PBR        | `access-list 100 permit tcp any any eq 80`   |
| `Numbered ACL`            | Uses ACL number (`1–99` for standard, `100–199` for extended). **Legacy format**, still commonly used.                                                 | Same as standard/extended            | Quick configs or simple use cases            | `access-list 10 permit 192.168.1.0 0.0.0.255` |
| `Named ACL`               | Uses a **custom name** instead of a number. Supports **sequence numbers** and easier management. Recommended for modern configs.                        | Same as numbered                     | Modern, editable ACLs                        | `ip access-list extended BLOCK-HTTP`         |
| `VTY ACL`                 | Standard ACL applied to **VTY lines** (SSH/Telnet) to **restrict remote access** by source IP. Essential for device hardening.                         | Source IP                            | SSH/Telnet access control                    | `access-class 55 in`                          |
| `SSH/Telnet ACL`          | Synonym of VTY ACL, focused on **restricting CLI remote access**. Uses standard ACL format. Common practice.                                           | Source IP                            | Limit SSH/Telnet access                      | `access-list 55 permit 10.0.0.0 0.0.0.255`    |
| `Reflexive ACL`           | **Stateful ACL** that allows return traffic based on outbound sessions. Less common in modern setups.                                                  | Session-aware (evaluated)            | Basic stateful filtering                     | `evaluate OUTBOUND-TRAFFIC`                  |
| `Time-Based ACL`          | ACL that applies **only during defined time-ranges** (e.g., work hours). Very useful but not widely used.                                              | IP + Time-Range                      | Temporary access control                     | `time-range WORK-HOURS`                      |
| `IPv6 ACL`                | ACL designed to filter **IPv6 traffic**. Syntax is similar to IPv4 extended ACLs.                                                                      | IPv6 addresses & protocols           | IPv6 traffic control                         | `ipv6 access-list BLOCK-ICMPv6`              |
| `Dynamic ACL (Lock-and-Key)` | **Allows access after user authentication** via Telnet. Rarely used today.                                                                      | Source IP (after login)              | Telnet-based access control (legacy)         | `autocommand access-enable`                  |
| `Port ACL (PACL)`         | Applied **directly to switchports** (Layer 2). Filters traffic entering physical ports. Used mostly in Catalyst switches.                              | MAC or IP                            | Security on access ports                     | `ip access-group 101 in` on L2 port          |
| `Control Plane ACL (CoPP)`| Filters traffic **destined to the control plane** (e.g., OSPF, BGP, SSH). Essential in datacenter/ISP.                                                  | Src/Dst IP, Protocol                 | CPU protection (CoPP/CPPr)                   | `control-plane` with ACL                     |
| `MAC ACL`                 | Filters traffic **based on MAC addresses** instead of IP. Only supported on certain switch platforms. Rarely used today.                               | Source/Destination MAC               | L2 security filtering on trunks or ports     | `mac access-list extended MAC-FILTER`        |




# 🗃️ Resources

- https://www.youtube.com/watch?v=ob7rgwt9nuk&list=PLwAU7bA502wHx44LlUptxSG6UQ3ONx3hh
  
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
