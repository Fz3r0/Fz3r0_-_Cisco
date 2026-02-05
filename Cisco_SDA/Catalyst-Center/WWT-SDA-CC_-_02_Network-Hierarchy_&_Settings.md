# 🚀🌐🔐 Cisco Catalyst Center & Cisco SDA: `Network Hierarchy & Settings`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0)

---

#### Keywords:

`Cisco Catalyst Center` `Cisco DNA Center` `Catalyst Center` `Cisco Software-Defined Access` `Cisco SDA` `Campus Fabric` `SDA Fabric` `Network Automation` `Network Assurance` `Intent-Based Networking` `IBN` `Fabric Edge` `Fabric Border` `Control Plane Node` `VXLAN` `Overlay Network` `Underlay Network` `LISP` `Anycast Gateway` `Virtual Networks` `VN` `Scalable Group Tag` `SGT` `TrustSec` `Policy Matrix` `Macro Segmentation` `Micro Segmentation` `Cisco ISE Integration` `Identity Services Engine` `802.1X` `Network Access Control` `NAC` `Fabric Wireless` `SDA Wireless` `Catalyst Center Assurance` `Telemetry` `Path Trace` `Client Health` `Device Health` `Troubleshooting` `Configuration Drift` `Day-0 Day-1 Day-2` `Campus Automation` `Enterprise Campus`

---

<br>

# 📄 `Index`



# 🌐 Cisco Catalyst Center & Cisco SDA: `Network Hierarchy & Settings`

Los módulos **Network Hierarchy** y **Settings** definen los cimientos sobre los cuales opera **Cisco Catalyst Center** como plataforma de **intent-based networking**. Antes de avanzar hacia funcionalidades como automation, policy o provisioning, resulta indispensable establecer una estructura lógica que represente de forma fiel tanto la organización como el entorno físico de la red.

<img height="380" alt="image" src="https://github.com/user-attachments/assets/7f8f06f4-6820-4bf5-8474-6cdc5d167c45" />

En este capítulo se introduce el concepto de **network hierarchy**, el cual permite modelar la red dentro de Catalyst Center mediante una estructura jerárquica basada en **sites**, organizados en niveles de **Area**, **Building** y **Floor**. Esta jerarquía no cumple únicamente un propósito organizativo, sino que actúa como el punto de referencia para la aplicación de configuraciones globales y específicas por ubicación.

<img width="592" height="422" alt="image" src="https://github.com/user-attachments/assets/217661e8-f350-4a53-8e87-c1106d910bee" /> <br><br>

Sobre esta jerarquía se apoyan los **network settings**, que constituyen los parámetros fundamentales para la operación de la plataforma. Entre ellos se incluyen:

* 🔑 **Device credentials**, utilizadas para automation y device onboarding
* 🌍 **Infrastructure services** como DNS y NTP
* 🌐 **IP address pools**
* 📦 **IP pool reservations**

Estos elementos son consumidos de forma transversal por los flujos de **automation** y **device provisioning**, permitiendo que Catalyst Center aplique configuraciones de manera consistente, repetible y alineada con la intención definida.

Un aspecto clave de la **network hierarchy** es el modelo de **inheritance**. Las configuraciones aplicadas en los niveles superiores de la jerarquía se heredan automáticamente hacia los niveles inferiores. A medida que se desciende en la jerarquía, las configuraciones pueden volverse más granulares, permitiendo excepciones controladas sin perder coherencia global.

Por ejemplo, es posible definir una jerarquía que represente regiones completas, ciudades dentro de esas regiones y edificios dentro de cada ciudad. Sobre esta estructura, pueden aplicarse diferentes network settings, como servidores DNS o NTP, en función de cada región o sitio específico.

Dentro de este modelo jerárquico:

* 🗺️ **Areas** (o *Sites*) representan el nivel más alto y no están asociadas a una dirección física. Funcionan como contenedores lógicos que pueden incluir subareas y buildings.
* 🏢 **Buildings** representan ubicaciones físicas reales, requieren una dirección y coordenadas geográficas, y pueden contener floors. Los buildings permiten aplicar configuraciones específicas a un sitio concreto.
* 🧱 **Floors** existen únicamente dentro de buildings y representan niveles físicos como pisos, oficinas, wiring closets y espacios de trabajo. En este laboratorio, no se definirán floors como parte del ejercicio.

El objetivo de este capítulo es proporcionar una base sólida para:

* 🧭 Construir jerarquías de red alineadas con la realidad operativa
* ⚙️ Centralizar configuraciones comunes y permitir excepciones controladas por sitio
* 📊 Administrar direcciones IP de forma estructurada mediante pools y reservas

Una correcta definición de la **network hierarchy** y de los **network settings** es un requisito fundamental tanto en entornos de laboratorio como en despliegues de producción, ya que impacta directamente en la eficiencia operativa, la escalabilidad y la coherencia de la configuración a lo largo del Campus.

---

Si quieres, el siguiente paso natural sería:

* **Site Hierarchy en el lab (Area y Building)**
* **Global vs Site-Specific Network Settings**
* o ir directo a **IP Address Pools & Reservations**

Esto ya quedó **nivel documentación de ingeniería**, no training notes.
Seguimos cuando tú digas 🧠📘









---

net design, se necesitara en un futuro 

<img width="2450" height="1240" alt="image" src="https://github.com/user-attachments/assets/11006e48-7724-4604-8daa-f71b3360e6b5" />


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


