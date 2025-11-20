# 🚀🌐🔐 Cisco SD-WAN (Viptela): `Overlay, Underlay and Components`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0)

---

#### Keywords:

`SD-WAN` `Cisco` `Viptela` `vBond` `vSmart` `vManage` `WAN Edge Device` `OMP` `TLOC` `Color` `Control Plane` `Data Plane` `Zero Touch Provisioning (ZTP)` `Device Template` `Feature Template` `SDN` `SASE` `SSE` `Cloud-Hosting Provider` `Broadband Internet` `MPLS` `Hybrid WAN` `Path Selection` `Packet Loss` `Latency` `Jitter` `Configuration Drift` `Automation` `WAN Optimization` `Private Data Center` `Overlay` `Underlay`  

---

<br>

# 📄 `Index`


# 🚀🌐 Cisco SD-WAN (Viptela): Core Principles

Cisco Viptela SD-WAN es una solución **software-defined wide-area network** diseñada para ofrecer a las empresas una forma más flexible, segura y eficiente de administrar sus conexiones WAN.  

Permite utilizar cualquier combinación de **private networks**, **public infrastructures** y **broadband internet** a través de data centers, branches y cloud-hosting providers, todo bajo un modelo **centralizado y fácil de operar**.

La clave del SD-WAN es que **separa el control plane del data plane**, virtualiza gran parte del routing tradicional y crea un **overlay network** encima de los circuitos físicos existentes.  

Esta separación permite que cada plano se especialice:

- 🧠 **Control Plane** → define políticas, rutas y reglas del overlay.  
- 🚚 **Data Plane** → mueve el tráfico real entre dispositivos.  

El resultado es un **fabric WAN flexible y robusto**, capaz de adaptarse a diversos transports (MPLS, broadband, LTE) con seguridad end-to-end y visibilidad completa.

Para entender bien cómo funciona SD-WAN hay que familiarizarnos un poco con el término underlay y overlay


# 🧬 Underlay vs Overlay – Aplicada a Cisco SD-WAN (Viptela)

En una arquitectura SD-WAN, existen dos capas fundamentales que trabajan juntas pero tienen funciones totalmente diferentes:

- 🟦 **Underlay** → La red física real (lo tradicional).  
- 🟪 **Overlay** → La red virtual inteligente (lo SD-WAN).

Esta separación es la base de cómo funciona Cisco Viptela SD-WAN.

<img width="680" height="350" alt="image" src="https://github.com/user-attachments/assets/c5e7592e-6d20-48ee-b161-905b28368549" />

## 🟦 Underlay – La Red Física

El **underlay** es la red real, tangible, la que ya existe antes de SD-WAN:

- Enlaces **MPLS**
- **Broadband Internet**
- **LTE / 5G**
- Fibra óptica de ISP
- Rutas del carrier
- Hops físicos y routers tradicionales
- IPs públicas y privadas
- Infraestructura física

### Características del Underlay

- ❌ No entiende SD-WAN  
- ❌ No conoce OMP  
- ❌ No sabe de IPSec overlay tunnels  
- ❌ No aplica políticas SD-WAN  

Su trabajo es **dar reachability IP básica**, nada más.

Es como la autopista hecha de concreto: existe, funciona, pero no es inteligente.

## 🟪 Overlay – La Red Virtual SD-WAN

El **overlay** es una red lógica creada *encima* del underlay mediante túneles entre WAN Edge devices:

- IPSec  
- GRE  
- (y en otras tecnologías también VXLAN)

Este overlay es donde vive **toda la inteligencia** de SD-WAN.

Incluye:

- vManage  
- vSmart  
- vBond  
- OMP (Overlay Management Protocol)  
- VPN segmentation  
- Application policies  
- Encryption end-to-end  
- Topologías virtuales (full mesh, hub-spoke, etc.)

### Qué aporta el Overlay

- 🔐 Encripción end-to-end entre WAN Edges  
- 🧠 Path selection inteligente basado en latency, jitter, packet loss  
- 📦 Application-aware routing  
- 🌐 Topologías virtuales dinámicas  
- 🛡️ Security policies centralizadas  
- 🏷️ VPN segmentation  
- 📈 Visibilidad total del comportamiento del tráfico  
- 🚀 Túneles IPSec automáticos entre sites  

El overlay es la “autopista elevada, moderna e inteligente”.

## 🎯 Diferencias Principales

| Concepto | Underlay | Overlay |
|---------|----------|---------|
| Tipo de red | Física | Virtual |
| Función | IP reachability | Inteligencia, seguridad, políticas |
| Protocolos | BGP/OSPF del ISP | OMP, IPSec |
| Topología | Fija y manual | Dinámica y centralizada |
| Seguridad | Depende del ISP | Full IPSec end-to-end |
| Control | Local en cada router | Centralizado por vSmart/vManage |

## 🔧 Cómo se relacionan Underlay y Overlay

- El **underlay** sólo provee IP reachability (los routers pueden hablar).  
- El **overlay** aprovecha esa reachability para construir túneles seguros.  

Los controladores SD-WAN viven en el overlay.  
Los paquetes viajan cifrados por el underlay.

El overlay **no reemplaza** al underlay, sino que lo **utiliza** y lo mejora.

##  🧠 OMP (Overlay Management Protocol) – El Protocolo del Overlay

**OMP (Overlay Management Protocol)** es el protocolo de routing del overlay.  
Es similar a BGP pero diseñado específicamente para SD-WAN.

OMP distribuye:

- Rutas del overlay  
- TLOCs  
- Políticas  
- Atributos de tráfico  
- Información de seguridad  
- Service-chains  

Mientras tanto:

- El underlay usa BGP/OSPF/estático del carrier  
- El overlay usa OMP para el control plane SD-WAN  

OMP vive dentro de los túneles DTLS/IPSec del overlay.

## 🧬 Analogía 

### 🛣️ Underlay = La calle de la ciudad  

Hecha de concreto, tráfico, semáforos viejos, ruta fija.

### 🛫 Overlay = Una autopista elevada inteligente  

Con sensores, reglas dinámicas, direcciones optimizadas y seguridad integrada.

- **El overlay depende del underlay, pero el underlay nunca sabe que el overlay existe.**

# 🧱 Componentes del Cisco SD-WAN Fabric

La arquitectura se compone de **cuatro elementos principales**, cada uno viviendo dentro de su propio plano lógico:

1. **vManage** – *Management Plane*  
2. **vBond** – *Orchestration Plane*  
3. **vSmart** – *Control Plane*  
4. **WAN Edge** – *Data Plane*

A continuación se explica cada uno.

---

## 🖥️ 1. vManage (Management Plane)

vManage es el **Network Management System (NMS)** del SD-WAN — el punto central de control para administrar toda la red.

### 🔧 Funciones principales

- 📡 Configuración centralizada de todos los WAN Edge devices.  
- 🛠️ Gestión de templates, policies, certificates y feature profiles.  
- 👁️ Monitoreo completo del fabric (alarms, logs, statistics, performance).  
- 🧪 Herramientas integradas de troubleshooting.  
- 💾 Repositorio central de software images para upgrades coordinados.  
- 🌐 Interfaz web para administradores: *single point of management*.  

vManage es literalmente **el cerebro operativo** desde donde se gestiona absolutamente todo.

---

## 🔐 2. vBond (Orchestration Plane)

vBond es el **gatekeeper** — el encargado de la autenticación inicial y orquestación.  
Sin vBond, ningún dispositivo puede ingresar al overlay.

### 🔧 Funciones principales

- 🛃 Autentica y valida cada dispositivo que intenta unirse al SD-WAN fabric.  
- 🔑 Verifica certificates e identidad.  
- 🛰️ Proporciona a los Edge la información necesaria para conectarse a vSmart y vManage.  
- 🔗 Establece **DTLS tunnels** entre Edge ↔ Controllers.  
- ⚙️ Facilita **ZTP (Zero Touch Provisioning)**.  
- 🔁 Puede existir redundancia con múltiples vBond en producción.

vBond es el primer controlador con el que habla cualquier WAN Edge nuevo.

---

## 🧠 3. vSmart (Control Plane)

vSmart es **el cerebro lógico** del overlay network.  
Controla las rutas, las políticas y la seguridad del fabric.

### 🔧 Funciones principales

- 🔗 Mantiene control plane adjacencies con todos los Edge mediante **OMP (Overlay Management Protocol)**.  
- 🔐 Maneja authentication, key-rotation y rekeying.  
- 🧠 Procesa y aplica:
  - Control policies  
  - Data policies  
  - VPN policies  
  - Traffic engineering  
- 🌍 Comparte rutas y metadatos con todos los WAN Edge.  
- 📚 Utiliza una tabla de rutas centralizada para autenticar nuevos edges.  
- ♻️ Soporta múltiples controladores vSmart por región para alta disponibilidad.

vSmart recibe políticas desde vManage y las distribuye por OMP hacia todos los Edge.

---

## 🚦 4. WAN Edge (Data Plane)

El WAN Edge es el router **físico o virtual** desplegado en branches, data centers o clouds.  
Es el elemento que realmente mueve los paquetes.

Ejemplos:

- 🟦 Cisco vEdge  
- 🟩 Cisco IOS-XE SD-WAN routers  

### 🔧 Funciones principales

- 🚚 Forwarding de tráfico entre sitios (data plane).  
- 🌐 Conectar múltiples tipos de WAN links: broadband, MPLS, LTE.  
- 🔐 Establecer **IPSec tunnels** entre Edge routers.  
- 🔄 Ejecutar routing tradicional: **OSPF, BGP, VRRP**.  
- ⚖️ Aplicar QoS, ACLs, shaping y políticas locales.  
- 📡 Mantener conexiones DTLS/OMP con vSmart y certificados gestionados por vManage.  
- 🧬 Formar el **overlay data plane fabric** sobre los circuitos físicos existentes.

Los WAN Edge son donde ocurre el tráfico real del usuario — el transporte del overlay.

---

# 🧩 Resumen visual de los planos

- Management Plane → vManage
- Orchestration Plane → vBond
- Control Plane → vSmart
- Data Plane → WAN Edge

Cada uno cumple un rol único y todos juntos forman el **Cisco SD-WAN Fabric**.












# 🗃️ Resources

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

