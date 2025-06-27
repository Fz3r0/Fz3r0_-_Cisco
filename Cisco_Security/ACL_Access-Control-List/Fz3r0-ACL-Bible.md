# 🧠🏗️🌐 Cisco Networking: `ACL - Access Control List :: Fz3r0 Bible`

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

## Reglas a considerar para ACL



- Se deben declarar de lo más específico a lo más general (Por ejemplo, una IP en particular en caso de solo querer una PC, o una red unicamente si solo se quiere seleccionar una subred por ejemplo)
- Las declaraciones deben especificar primero lo que se permite (en una norma, aunque en ocasiones especiales si se puede llegar a poner primeor lo que se niega, estas son las nuevas practicas)
- Las declaraciones se revisan de arriba hacia abajo (es decir, en la lsita, literal se revisan de arriba para abajo, el orden importa!!! por ejemplo en nuetsro ejemplo , perimo revisara el puerto 80 http, y despues el SMPT y asi...por eorden si primero se niega la red 10.10.x.x y luego se permite, se va a terminar pemritiendo, porque seri ala sguenda linea la que lee al final de forma secuencial) 
- Cuando se encuentra una coincidencia se ejecuta y termina la revisión (como dije antes, prime se ejecuta la sentencia, y depsues la sque siguen, por eso el ejemplo de por eorden si primero se niega la red 10.10.x.x y luego se permite, se va a terminar pemritiendo, porque seri ala sguenda linea la que lee al final de forma secuencial)
- Por defecto bloquea todo el tráfico al final de la lista (por default el router al final podnra una linea que se llama "negar todo lo demas", asi que en caso que no se haya permitido o negado lo necesario, eso seria bloqueado)
- Las ACL se aplican por interfaz (Es decir, despues de crar una lsita, hay qu eentrar a la interfaz y aplicarla. En pocas ocasiones se pone en otras lineas como para VTY o NAT)
- Solo se puede aplicar una ACL por sentido en cada interfaz (E/S) (Solo un ACL por entrada o por salida)
- Solo se puede aplicar una ACL por protocolo de Capa 3 (IPv4, IPv6)

## Wildcard

Es una máscara de bits que indica qué partes de una dirección de IP debe coincidir para la ejecución de una determinada acción de la lista. Es deicr, que parte de la IP es la que tiene que coincideir para permitirla/negarl

- LA wildcard sencillamente es lo contrario a la mscara

Otra forma de decirlo es que el Wildcard es la representación de bits significativos

Los “0” en el wildcard indican los bits significativos (es decir lo bit q ue no pueden cambair, osea con los que estoy comparando)

Los “1” en el wildcard indican que ese bit es irrelevante (es decir que puede ser 1 o 0, no importa, estos no seran leidos)

Es por eso que se usa una wildcard!!! y no la mwsxcara, porque digamsoq ue es el proceso a la rewversa entre signifciativos y no. 

Concepto basico:

Red: 192.168.1.0/24

Mascara 255.255.255.0

Wild card 0.0.0.255 (inversa)

que esta pasando? en binaro el /24 significa los bits que estan activos, en hexadicimal ya vimos qu eeso:

Mascara /24 == 255.255.255.0 == 11111111.11111111.11111111.00000000

eentonces una vez comprneido eso, lo que debe hacer martch son lso bit significativos, los 1, osea 192.168.1 y el .0 seria el no signiciativo. osea la wil car dserira 0.0.0.255 == 00000000.0000000.0000000.11111111

Entocnes en las ACL por eso dse dcide usar la wilcard, si por ejemplo la wildcard fuera mas especifica, yo podria haacer coincidencia con solo la IP 192.168.1.1 con su binario

## ACL y and OUT

Es sencillo pero iportante comprende de que sentido debo onfigurar las ACL en una interfaz. ya se ainbpound o outbound

Poneindo un ejemplo sencillo, con un router enmedio y una PC1 del lazi quiziquiero y una PC 2 dell lado derecho, cada una en su red ejemplo 10.10.1.0 y 10.10.2.0 se va a confugurar los ACL enmedio en el router

- En este ejemplo en particular existira trafico entre PC1 (eth0/1) y PC2 (eth0/2) y dependiendo el caso el trafico seria inbound o outbound. 
- El requerimiento es "prihuibir la trasnferencia de arhcivos FTP/TFTP/SMB entre la PC 1 y PC 2

y la pregunta qui es: 

- Se debe configurar el ACL inbound o outbound?  todo depende de donde viene el trafico que se quiere bloquear e smuy faicl:

Ejemplo 1: 

Aqui la PC 1 es quien genera el flujo de trafico inicial, es quien genera la "conversaicion"

- PC1 envia trafico FTP a router a la Eth0/1 donde esta conectado
- Entonces, el trafico ENTRA (inbound) por la interfaz Eth 0/1 al router (el putbound esta en eth0/2 hacia la PC2)

Ejemplo 2: 

Aqui la PC 2 es quien genera el flujo de trafico inicial, es quien genera la "conversaicion"

- PC2 envia trafico FTP a router a la Eth0/2 donde esta conectado
- Entonces, el trafico ENTRA (inbound) por la interfaz Eth 0/2 al router (el putbound esta en eth0/1 hacia la PC1)


![image](https://github.com/user-attachments/assets/5cec8d19-71a6-4c78-a9eb-9e0111c54372)

Donde configurar la ACL para bloquear el trafico entonceS? inbound u outbiun?:

Recordar siempre la regla, hay que bliqeuar el inbound, es decir desde donde se esta generando la conversaicon inciial. porque sino, el trafico lograria entrar, y aunque no pueda salir, ya habra atreavezado el router por primera vez, eso no es necesario, se debe blqoeuar desd einciio, es decir, ibound! 






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
