# 🚀🌐🔐 Cisco Catalyst Center: `System`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0)

---

#### Keywords:

`Cisco Catalyst Center` `Cisco DNA Center` `Catalyst Center` `Cisco Software-Defined Access` `Cisco SDA` `Campus Fabric` `SDA Fabric` `Network Automation` `Network Assurance` `Intent-Based Networking` `IBN` `Fabric Edge` `Fabric Border` `Control Plane Node` `VXLAN` `Overlay Network` `Underlay Network` `LISP` `Anycast Gateway` `Virtual Networks` `VN` `Scalable Group Tag` `SGT` `TrustSec` `Policy Matrix` `Macro Segmentation` `Micro Segmentation` `Cisco ISE Integration` `Identity Services Engine` `802.1X` `Network Access Control` `NAC` `Fabric Wireless` `SDA Wireless` `Catalyst Center Assurance` `Telemetry` `Path Trace` `Client Health` `Device Health` `Troubleshooting` `Configuration Drift` `Day-0 Day-1 Day-2` `Campus Automation` `Enterprise Campus`

---

<br>

# 📄 `Index`



# ⚙️ Cisco Catalyst Center: `System`

El menú **System** en Cisco Catalyst Center concentra todas las funciones relacionadas con la **gestión de la plataforma**, independientemente de la arquitectura de red que se administre (SDA, traditional campus, wireless, etc.).
Desde esta sección se valida el estado operativo del sistema, se gestionan usuarios y roles, se controla el ciclo de vida del software y se configuran mecanismos de respaldo y recuperación.

<img width="578" height="651" alt="image" src="https://github.com/user-attachments/assets/25ad4c9f-c828-4327-87c3-e296c8db9826" />

## 🧭 `System 360`

La vista **System 360** es el punto central para validar la **salud del Catalyst Center**.
Desde esta pantalla se obtiene una visión completa del cluster y de los servicios que componen la plataforma.

En esta sección se visualiza:

* 🧩 Número de **Catalyst Center nodes** desplegados en el cluster
* 🌐 **Management IP address** asociada a cada nodo
* ✅ **Node status** y **services status** general

<img width="2797" height="922" alt="image" src="https://github.com/user-attachments/assets/a575d89f-3b02-48a1-abfa-f05cbd73251d" />


Al acceder a la sección de **Services**, se presenta una lista detallada de todos los servicios internos en ejecución. Para cada servicio se muestra:

* 🏷️ Service name
* 📊 Current state (Up / Down)
* 🧬 Software version
* 📈 Acceso a **metrics** y **logs**

<img width="3055" height="920" alt="image" src="https://github.com/user-attachments/assets/8acc4400-e0f6-4da7-b38b-d2a99cc2dd7a" />

<img width="2787" height="925" alt="image" src="https://github.com/user-attachments/assets/557ff097-f50a-4e45-91f2-83f279dc1902" />

Esta información resulta crítica durante actividades de **monitoring**, **performance validation** y **troubleshooting**, ya que permite identificar rápidamente servicios degradados o inconsistencias de versión dentro de la plataforma.


## 📦 Software Management

La sección **Software Management** permite administrar el **software lifecycle** de Catalyst Center.

Desde esta vista se valida:

* ☁️ Conectividad con el **Cisco software server**
* 🆕 Disponibilidad de nuevas **releases**
* ⏬ Estado de descargas en curso
* 🧱 Aplicaciones instaladas y disponibles

Cuando una nueva versión se encuentra disponible, el sistema presenta opciones para **download** e **install**.
En entornos de laboratorio, es común que se muestre una versión más reciente que la actualmente instalada. Sin embargo, durante ejercicios prácticos se recomienda **no ejecutar upgrades**, ya que el proceso puede tomar varias horas y afectar la estabilidad del entorno.

Adicionalmente, la sección **Currently Installed Applications** muestra las aplicaciones core asociadas a la versión en uso, incluyendo sus versiones individuales. Desde aquí es posible:

* 🔗 Revisar dependencias
* 🗑️ Desinstalar aplicaciones
* ➕ Instalar nuevas aplicaciones disponibles

---

## 🛠️ Settings

La sección **Settings** agrupa la configuración global del **Catalyst Center platform**.

Aquí se definen parámetros de carácter **system-wide**, entre los que se incluyen:

* 🌍 Global configurations
* 🎛️ Control parameters
* 🔐 Certificate management
* 🔌 Integration con external services
* ☁️ Smart Account configuration
* ⚙️ Otros parámetros fundamentales para la operación de la plataforma

Estos ajustes no están relacionados directamente con la topología de red, sino con el **comportamiento interno** del sistema y sus integraciones externas.

---

## 📊 Data Platform

El módulo **Data Platform** proporciona visibilidad sobre los servicios de **data processing** y **data storage** utilizados por Catalyst Center.

Desde esta sección se puede:

* 📡 Monitorear cómo se procesa la **telemetry data**
* 🧠 Validar el estado de los servicios que soportan **analytics** y **assurance**
* ⚙️ Confirmar que el flujo de datos opera correctamente en background

Este componente es clave para funciones de **Assurance**, **telemetry analysis** y **health monitoring**, aunque normalmente opera de forma transparente para el usuario.

---

## 👤 Users and Roles

La sección **Users and Roles** se utiliza para administrar el acceso a la plataforma mediante **Role-Based Access Control (RBAC)**.

Catalyst Center incluye roles predefinidos que cubren los escenarios más comunes, como:

* 👑 Super Admin
* 🛡️ Network Admin
* 👀 Observer

Adicionalmente, se pueden crear **custom roles**, asignando permisos específicos para ajustar el acceso a requerimientos operativos particulares.
Este modelo permite una segmentación clara de responsabilidades dentro del equipo de operación.

La plataforma también soporta integración con **AAA**, permitiendo centralizar la autenticación y autorización de usuarios.

---

## 💾 Backup and Restore

La sección **Backup and Restore** permite proteger la configuración del Catalyst Center mediante respaldos programados o manuales.

Desde este módulo se puede:

* 🗄️ Configurar backup servers
* ⏰ Programar backups recurrentes
* 🔐 Almacenar configuraciones de forma segura
* ♻️ Restaurar el sistema en caso de falla o migración

Este componente es esencial para estrategias de **disaster recovery** y para preservar la continuidad operativa de la plataforma.

---

## 🧠 Cierre del capítulo

El menú **System** proporciona las herramientas fundamentales para operar y mantener Cisco Catalyst Center como plataforma.
Desde la validación de servicios hasta la gestión de usuarios, software y respaldos, esta sección establece la base sobre la cual se construyen todas las funciones de **automation**, **assurance** y **policy**.

Antes de avanzar hacia cualquier arquitectura de red o flujo de trabajo, se debe validar claramente que el sistema se encuentre **healthy**, actualizado y correctamente configurado a nivel de plataforma.

---

Esto ya está **nivel libro técnico moderno**, Carlos.
Si quieres, en el siguiente capítulo podemos:

* mantener **la misma paleta de emojis**
* definir **iconografía fija** (ej. ⚙️ siempre system, 🧠 assurance, 📡 telemetry)
* o arrancar **Capítulo 2: Catalyst Center Architecture vs SDA Fabric**

Tú mandas. Vector sigue documentando 📘🔥


## Lab:

Topology:

<img width="1004" height="792" alt="image" src="https://github.com/user-attachments/assets/0fe3dd98-159a-4448-ae11-c460cd247e33" />

Routing:

<img width="930" height="1107" alt="image" src="https://github.com/user-attachments/assets/58afaff5-5321-45dc-9d3e-8634505882e6" />


# 🗃️ Resources

- 
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


