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

<span align="center"> <p align="center"><img height="400" alt="image" src="https://github.com/user-attachments/assets/25ad4c9f-c828-4327-87c3-e296c8db9826" /></p> </span>

## 🧭 `System 360`

La vista **System 360** es el punto central para validar la **salud del Catalyst Center**.
Desde esta pantalla se obtiene una visión completa del cluster y de los servicios que componen la plataforma.

En esta sección se visualiza:

* 🧩 Número de **Catalyst Center nodes** desplegados en el cluster
* 🌐 **Management IP address** asociada a cada nodo
* ✅ **Node status** y **services status** general

<span align="center"> <p align="center"><img width="2797" height="922" alt="image" src="https://github.com/user-attachments/assets/a575d89f-3b02-48a1-abfa-f05cbd73251d" /></p> </span>


Al acceder a la sección de **Services** desde la parte del **Host**, se presenta una lista detallada de todos los servicios internos en ejecución. Para cada servicio se muestra:

* 🏷️ Service name
* 📊 Current state (Up / Down)
* 🧬 Software version
* 📈 Acceso a **metrics** y **logs**

<span align="center"> <p align="center"><img width="3054" height="917" alt="image" src="https://github.com/user-attachments/assets/fb4d1da0-9fe1-4c36-af91-b656753aa39f" /></p> </span>

También se puede acceder desde la pestaña de **Service Explorer** y verlos uno por uno:

<span align="center"> <p align="center"><img width="2787" height="925" alt="image" src="https://github.com/user-attachments/assets/557ff097-f50a-4e45-91f2-83f279dc1902" /></p> </span>

Esta información resulta crítica durante actividades de **monitoring**, **performance validation** y **troubleshooting**, ya que permite identificar rápidamente servicios degradados o inconsistencias de versión dentro de la plataforma.


## 📦 `Software Management`

La sección **Software Management** permite administrar el **software lifecycle** de Catalyst Center.

Desde esta vista se valida:

* ☁️ Conectividad con el **Cisco software server**
* 🆕 Disponibilidad de nuevas **releases**
* ⏬ Estado de descargas en curso
* 🧱 Aplicaciones instaladas y disponibles

<span align="center"> <p align="center"><img width="3058" height="925" alt="image" src="https://github.com/user-attachments/assets/6a4c0a3f-4b8f-459c-bec8-65d3cff167cf" /></p> </span>

Cuando una nueva versión se encuentra disponible, el sistema presenta opciones para **download** e **install**.
En entornos de laboratorio, es común que se muestre una versión más reciente que la actualmente instalada. Sin embargo, durante ejercicios prácticos se recomienda **no ejecutar upgrades**, ya que el proceso puede tomar varias horas y afectar la estabilidad del entorno.

Adicionalmente, la sección **Currently Installed Applications** muestra las aplicaciones core asociadas a la versión en uso, incluyendo sus versiones individuales. Desde aquí es posible:

* 🔗 Revisar dependencias
* 🗑️ Desinstalar aplicaciones
* ➕ Instalar nuevas aplicaciones disponibles

<span align="center"> <p align="center"><img width="1218" height="1047" alt="image" src="https://github.com/user-attachments/assets/1208b3f6-e816-41b9-871a-22431c9d53df" /></p> </span>


## 🛠️ `Settings`

La sección **Settings** agrupa la configuración global del **Catalyst Center platform**.

Aquí se definen parámetros de carácter **system-wide**, entre los que se incluyen:

* 🌍 Global configurations
* 🎛️ Control parameters
* 🔐 Certificate management
* 🔌 Integration con external services
* ☁️ Smart Account configuration
* ⚙️ Otros parámetros fundamentales para la operación de la plataforma

<span align="center"> <p align="center"><img width="3046" height="928" alt="image" src="https://github.com/user-attachments/assets/e7b7fc69-a6ae-46ea-98f6-b0a77c6f312a" /></p> </span>

<span align="center"> <p align="center"><img width="3053" height="927" alt="image" src="https://github.com/user-attachments/assets/eb2ad230-4148-4321-9150-1951111519b8" /></p> </span>

Estos ajustes no están relacionados directamente con la topología de red, sino con el **comportamiento interno** del sistema y sus integraciones externas.

## 📊 `Data Platform`

El módulo **Data Platform** proporciona visibilidad sobre los servicios de **data processing** y **data storage** utilizados por Catalyst Center.

Desde esta sección se puede:

* 📡 Monitorear cómo se procesa la **telemetry data**
* 🧠 Validar el estado de los servicios que soportan **analytics** y **assurance**
* ⚙️ Confirmar que el flujo de datos opera correctamente en background

<span align="center"> <p align="center"><img width="2256" height="577" alt="image" src="https://github.com/user-attachments/assets/603b6a95-efaf-447c-9369-a492d87d68e9" /></p> </span>

<span align="center"> <p align="center"><img width="2551" height="935" alt="image" src="https://github.com/user-attachments/assets/b2cb3401-0451-40f1-a983-4e0a13b1da7a" /></p> </span>

Este componente es clave para funciones de **Assurance**, **telemetry analysis** y **health monitoring**, aunque normalmente opera de forma transparente para el usuario.


## 👤 `Users and Roles`

La sección **Users and Roles** se utiliza para administrar el acceso a la plataforma mediante **Role-Based Access Control (RBAC)**.

Catalyst Center incluye roles predefinidos que cubren los escenarios más comunes, como:

* 👑 Super Admin
* 🛡️ Network Admin
* 👀 Observer

<span align="center"> <p align="center"><img width="2578" height="412" alt="image" src="https://github.com/user-attachments/assets/3e592b8d-fb9a-4ebe-ae62-1b415b89c60b" /></p> </span> <br><br>

Adicionalmente, se pueden crear **custom roles**, asignando permisos específicos para ajustar el acceso a requerimientos operativos particulares.
Este modelo permite una segmentación clara de responsabilidades dentro del equipo de operación.

<span align="center"> <p align="center"><img width="3065" height="461" alt="image" src="https://github.com/user-attachments/assets/0e2b54fe-971b-4e68-9e76-1bc373dc56e9" /></p> </span> <br><br>


La plataforma también soporta integración con **AAA**, permitiendo centralizar la autenticación y autorización de usuarios.

<span align="center"> <p align="center"><img width="3061" height="856" alt="image" src="https://github.com/user-attachments/assets/b3ceaadf-1a94-43e0-bd4f-ed155562876d" /></p> </span> <br><br>


## 💾 Backup and Restore

La sección **Backup and Restore** permite proteger la configuración del Catalyst Center mediante respaldos programados o manuales.

Desde este módulo se puede:

* 🗄️ Configurar backup servers
* ⏰ Programar backups recurrentes
* 🔐 Almacenar configuraciones de forma segura
* ♻️ Restaurar el sistema en caso de falla o migración

<img width="1929" height="571" alt="image" src="https://github.com/user-attachments/assets/d0ba626b-df88-416b-acdc-32726430df4b" />

<img width="1981" height="705" alt="image" src="https://github.com/user-attachments/assets/cbcf8258-491f-4978-a319-9e3075e07110" />

Este componente es esencial para estrategias de **disaster recovery** y para preservar la continuidad operativa de la plataforma.

## 🧠 Conclusiones

El menú **System** proporciona las herramientas fundamentales para operar y mantener Cisco Catalyst Center como plataforma.
Desde la validación de servicios hasta la gestión de usuarios, software y respaldos, esta sección establece la base sobre la cual se construyen todas las funciones de **automation**, **assurance** y **policy**.

**Antes de avanzar hacia cualquier arquitectura de red o flujo de trabajo, se debe validar claramente que el sistema se encuentre "healthy", actualizado y correctamente configurado a nivel de plataforma.**


# 🗃️ Resources

- WWT
  
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


