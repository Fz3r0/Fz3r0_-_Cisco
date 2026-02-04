# 🚀🌐🔐 Cisco Catalyst Center & Cisco SDA: `Lab Environment`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0)

---

#### Keywords:

`Cisco Catalyst Center` `Cisco DNA Center` `Catalyst Center` `Cisco Software-Defined Access` `Cisco SDA` `Campus Fabric` `SDA Fabric` `Network Automation` `Network Assurance` `Intent-Based Networking` `IBN` `Fabric Edge` `Fabric Border` `Control Plane Node` `VXLAN` `Overlay Network` `Underlay Network` `LISP` `Anycast Gateway` `Virtual Networks` `VN` `Scalable Group Tag` `SGT` `TrustSec` `Policy Matrix` `Macro Segmentation` `Micro Segmentation` `Cisco ISE Integration` `Identity Services Engine` `802.1X` `Network Access Control` `NAC` `Fabric Wireless` `SDA Wireless` `Catalyst Center Assurance` `Telemetry` `Path Trace` `Client Health` `Device Health` `Troubleshooting` `Configuration Drift` `Day-0 Day-1 Day-2` `Campus Automation` `Enterprise Campus`

---

<br>

# 📄 `Index`



# 🧪 Cisco Catalyst Center & Cisco SDA: `Lab Environment`

Este laboratorio representa un **small Campus deployment** diseñado para practicar y validar conceptos de **Cisco Catalyst Center** y **Cisco Software-Defined Access (SDA)**. El entorno incluye componentes de routing, switching, identity y management, todos interconectados dentro de una plataforma de virtualización.

El objetivo principal del laboratorio es proporcionar una base funcional mínima para construir un **fabric network**, validar integración con **Cisco ISE** y operar Catalyst Center desde un estado inicial limpio.

## 🏫 Topología del Campus

La topología inicial del laboratorio incluye:

* 🌐 x2 **border routers** con conectividad a Internet utilizando **Cisco 8000v routers**
* 🔀 x4 **Cisco Catalyst 9000v virtual switches**
* 🖥️ x2 **Windows client workstations**
* 🧠 x1 deployment de **Cisco Catalyst Center**
* 🔐 x1 dispositivo **Cisco Identity Services Engine (ISE)**
* 🌍 x1 **Windows Server** que provee **DNS services** a los dispositivos del Campus

<img width="1004" height="797" alt="image" src="https://github.com/user-attachments/assets/ca8ea621-949f-4ac3-a950-5264835cf8a8" />

El routing del laboratorio ha sido configurado con:

* 🔁 **BGP** entre los Campus routers y el Internet
* 🔁 **BGP** entre los routers y los switches adyacentes (Cat9K-A y Cat9K-B)
* 🧭 **IS-IS** entre los Campus switches

<img width="946" height="1129" alt="image" src="https://github.com/user-attachments/assets/d67b21ff-ae92-4bc9-9f3f-bd1c71e801b0" />

Esta configuración representa el **mínimo routed underlay** requerido para construir un **SDA fabric** dentro del laboratorio.

## 🔀 Cisco Catalyst 9000v

El **Cisco Catalyst 9000v** es un virtual switch basado en **IOS-XE**, que opera como dispositivo **Layer 2/3** y proporciona una emulación de **software-based data plane**.

Características importantes del Catalyst 9000v en este laboratorio:

* 🧬 Imagen **IOS-XE**
* 🧪 Distribuido en modalidad **BETA**, sin soporte oficial para producción
* 💻 Requiere **x4 vCPUs** y **18 GB de RAM** por nodo
* 🚧 **Data plane throughput muy limitado**

  * La mayoría de versiones soportan menos de **1 Mbps**
  * Algunas versiones pueden bajar hasta **300 Kbps**

<img width="1493" height="689" alt="image" src="https://github.com/user-attachments/assets/54d7d4a4-dc21-441c-9deb-888a7e863d14" />

Estos switches se ejecutan dentro de una VM de **EVE-NG**, utilizada como plataforma de emulación para simular topologías complejas de red.



## 🧠 Plataforma EVE-NG

El laboratorio se ejecuta dentro de **EVE-NG (Emulated Virtual Environment Next Generation)**, una plataforma de emulación de red que permite desplegar y conectar dispositivos virtuales como routers, switches y firewalls.

Desde EVE-NG se puede:

* ▶️ Iniciar dispositivos
* ⏹️ Detener dispositivos
* 🛠️ Acceder a consolas para troubleshooting
* 🔄 Recuperar nodos en caso de crash

La VM de EVE-NG tiene conectividad externa hacia:

* Routers
* Catalyst Center
* Cisco ISE
* Windows client workstations



## 🖥️ Acceso a Cisco Catalyst Center

Catalyst Center se accede mediante:

* 🌐 Abrir **Chrome Browser**
* 🔖 Seleccionar el bookmark **Cisco Catalyst Center**
* 🔐 Autenticarse con las credenciales configuradas en el laboratorio

<img width="518" height="145" alt="image" src="https://github.com/user-attachments/assets/7515aa7e-7f07-48e2-b03d-6970103e5e6a" />

## 🔐 Acceso a Cisco Identity Services Engine (ISE)

Cisco ISE se accede mediante:

* 🌐 Abrir **Chrome Browser**
* 🔖 Seleccionar el bookmark **Identity Service Engine**
* 🔐 Autenticarse con credenciales administrativas

<img width="518" height="145" alt="image" src="https://github.com/user-attachments/assets/68b9d566-80dc-422a-9a62-e277362c130e" />

## 💻 Acceso CLI a Routers y Switches

El acceso CLI a routers y switches se realiza mediante **mRemoteNG**, el cual ya se encuentra configurado en la workstation del laboratorio desde el **Escritorio**.

<img width="366" height="291" alt="image" src="https://github.com/user-attachments/assets/1c70667a-87f8-44fa-90fd-613df58a0886" />

Características:

* 🧰 Las conexiones ya tienen credenciales preconfiguradas
* 🖱️ Acceso por doble clic sobre el dispositivo deseado
* 🔧 Uso principal para troubleshooting

## 🧠 Acceso a EVE-NG GUI

La interfaz gráfica de **EVE-NG** se accede mediante:

* 🌐 Abrir **Chrome Browser**
* 🔖 Seleccionar el bookmark **EVE-NG**
* 🖥️ Seleccionar **HTML console** para acceso directo a los nodos

<img width="518" height="145" alt="image" src="https://github.com/user-attachments/assets/7515aa7e-7f07-48e2-b03d-6970103e5e6a" />

Dentro de EVE-NG es posible iniciar o detener dispositivos haciendo clic derecho sobre cada nodo, lo cual resulta útil durante troubleshooting o recuperación de imágenes.


## 🖥️ Acceso a Client Workstations

El laboratorio incluye **x2 Windows client workstations**, conectadas a los switches del Campus.

El acceso CLI a routers y switches se realiza mediante **mRemoteNG**, el cual ya se encuentra configurado en la workstation del laboratorio desde el **Escritorio**.

* 🖱️ Remote Desktop Tool disponible en la taskbar o desktop
* 📂 Selección del client requerido

<img width="366" height="291" alt="image" src="https://github.com/user-attachments/assets/1c70667a-87f8-44fa-90fd-613df58a0886" />



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


