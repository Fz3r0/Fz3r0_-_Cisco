# 🚀🌐🔐 Cisco SD-WAN (Viptela): `WAN Edge Onboarding`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0)

---

#### Keywords:

`SD-WAN` `Cisco` `Viptela` `vBond` `vSmart` `vManage` `WAN Edge Device` `OMP` `TLOC` `Color` `Control Plane` `Data Plane` `Zero Touch Provisioning (ZTP)` `Device Template` `Feature Template` `SDN` `SASE` `SSE` `Cloud-Hosting Provider` `Broadband Internet` `MPLS` `Hybrid WAN` `Path Selection` `Packet Loss` `Latency` `Jitter` `Configuration Drift` `Automation` `WAN Optimization` `Private Data Center` `Overlay` `Underlay`  

---

<br>

# 📄 `Index`


# 🚀🌐 Cisco SD-WAN (Viptela): WAN Edge Onboarding

- Cisco SD-WAN Troubleshooting tips: https://www.cisco.com/c/en/us/support/docs/routers/sd-wan/214509-troubleshoot-control-connections.html

En esta sección se revisa cómo expandir una **SD-WAN fabric** agregando nuevos **WAN Edge devices** a la red. En **Cisco SD-WAN (Viptela)**, este proceso puede realizarse de distintas formas y normalmente se clasifica en dos grandes categorías: **automated provisioning** y **manual provisioning** (aunque también existe el método **bootstrap**).

Antes de ejecutar un onboarding en un entorno real, resulta fundamental comprender el flujo exacto mediante el cual un **WAN Edge** se integra a la **control plane** y queda operativo dentro de la fabric, así como el rol que desempeña cada controller involucrado en este proceso.

🎯 El propósito de este documento es:

- 🧠 Analizar el rol de cada **controller** dentro del onboarding
- 🔢 Identificar el **orden lógico del proceso**
- 🔐 Comprender los requisitos necesarios para que un **WAN Edge** se integre correctamente al **SD-WAN fabric**
- 📘 Utilizar terminología técnica en inglés, tal como aparece en la documentación oficial y en el examen

## 🛣️🔐 WAN Edge Onboarding

Para que un **WAN Edge device** pueda unirse a la **SD-WAN fabric**, el dispositivo debe establecer conectividad inicial con el **VBond**, el cual actúa como **gatekeeper** y punto de entrada al control plane.

El **VBond** es responsable de la autenticación inicial del device y de la redirección hacia los demás controllers, pero no mantiene control connections permanentes con los WAN Edge devices.

---

## 🛠️📦 Métodos de Provisioning

La información necesaria para que el **WAN Edge** pueda contactar al **VBond** puede configurarse utilizando distintos métodos de provisioning:

- 🧑‍💻 **manual provisioning**  
  Configuración directa vía **CLI**, donde se define explícitamente la información del VBond y otros parámetros iniciales.

- 🧩 **bootstrap provisioning**  
  Uso de una **bootstrap configuration** (por ejemplo, bootstrap file o ZTP-style bootstrap) que permite inicializar el device con la información mínima requerida.

- 🤖 **automated provisioning**  
  Proceso automatizado gestionado desde **vManage**, donde el device obtiene su configuración inicial mediante workflows y mecanismos de automatización.

---

## 🔁📡 Flujo de Onboarding del WAN Edge

### 1️⃣ 🧭 VBond Information

El **WAN Edge** es configurado con la información del **VBond**, ya sea de forma **manual** via CLI, mediante **bootstrap** provisioning o a través de **automated** provisioning.

<img width="460" height="394" alt="image" src="https://github.com/user-attachments/assets/4a0a0fd4-a245-40c9-b36f-069c003da5f8" />

- 🧾 Incluye IP address o FQDN del VBond
- 🧭 Define el punto inicial de contacto con la SD-WAN fabric


### 2️⃣ 🔑📡 Autenticación con VBond

Una vez configurado, el **WAN Edge** inicia comunicación con el **VBond** para realizar el proceso de autenticación.

<img width="459" height="397" alt="image" src="https://github.com/user-attachments/assets/1ce5eda2-062b-4156-bf74-74a80b374b70" />

- 🔐 La autenticación se basa en **system information**
- 🆔 Incluye identidad del device, certificados y serial information

---

### 3️⃣ 🔄📣 Notificación y Redirección a Controllers

Después de que el WAN Edge es autenticado correctamente:

- 📣 El **VBond** notifica a **vManage** y **vSmart** sobre una conexión entrante
- 🧭 El **VBond** redirige al WAN Edge hacia **vManage** y **vSmart**, proporcionando la información de IP necesaria

<img width="485" height="393" alt="image" src="https://github.com/user-attachments/assets/ae9439cf-f7c9-444a-93e6-9b4b28d0cdaa" />

⚠️ Punto clave:

- 🚪 El **VBond** no mantiene control connections permanentes
- 🧭 Su función se limita a autenticación inicial y redirección

<img width="447" height="352" alt="image" src="https://github.com/user-attachments/assets/df6ee74e-baec-47a9-8d8f-050077f06f4c" />

### 4️⃣ 🔗🖥️ Conexiones Permanentes del Control Plane

Con la información recibida, el **WAN Edge** establece conexiones directas y permanentes con:

- 🖥️ **vManage**
- 🧠 **vSmart**

Estas conexiones forman parte del **control plane** y requieren procesos adicionales de autenticación.

---

### 5️⃣ ⚙️🧠 Configuración y Programación del Control Plane

Una vez aceptado dentro de la fabric:

- 🖥️ **vManage**:
  - ⚙️ Aplica la configuración del device
  - 📐 Distribuye device templates y feature templates

- 🧠 **vSmart**:
  - 📡 Programa el control plane
  - 🛣️ Distribuye forwarding information
  - 📜 Aplica control policies, routing policies y data policies

---

### 6️⃣ 🟢🚀 WAN Edge Operativo

Al finalizar el proceso:

- 🟢 El **WAN Edge** queda online dentro de la SD-WAN fabric
- 🔗 El control plane se encuentra completamente establecido
- 🚦 El forwarding funciona conforme a las policies definidas
- 🌐 El device queda integrado y listo para transportar traffic de producción







# 🗃️ Resources

- https://www.cisco.com/c/en/us/support/docs/routers/sd-wan/214509-troubleshoot-control-connections.html
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


