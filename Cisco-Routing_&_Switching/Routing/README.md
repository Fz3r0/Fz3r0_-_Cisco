# 📌 Routing Protocols 

En redes de computadoras, los **Routing Protocols** son un conjunto de reglas utilizadas por los routers para intercambiar información sobre rutas dentro de una red.  

Su función principal **no es mover paquetes de datos**, sino actualizar y gestionar la **routing table**, que contiene información sobre las mejores rutas hacia diferentes destinos.  

Las rutas aprendidas a través de un **Routing Protocol** se denominan **dynamic routes**.  

El objetivo del **Dynamic Routing** es garantizar una comunicación eficiente entre routers, con beneficios como:  

- ✅ No es necesario configurar manualmente cada ruta, como en **Static Routing**.  
- ✅ Los routers pueden anunciar cambios en la red y reaccionar ante fallos en los enlaces.  

## 🔹 Tipos de Routing Protocols  

![image](https://github.com/user-attachments/assets/921edaf0-8c3c-4160-947b-32bd2f60e78e)

![image](https://github.com/user-attachments/assets/0256d59f-28cb-412b-a995-cc57927919d3)

The image illustrates the classification of routing protocols into two main categories: 

1. Static
2. Dynamic.

Dynamic routing protocols are further divided into:

- Interior Gateway Protocols (IGP)
- Exterior Gateway Protocols (EGP).

IGP is subdivided into: 

- Distance Vector
- Link State
- Hybrid protocols

EGP only includes:

- Path Vector protocols.

Key Routing Protocols Highlighted:

- Static Routing Protocols 🛠️: These are manually configured and provide a fixed path for data packets. They are simple but lack flexibility in adapting to network changes.
- Dynamic Routing Protocols:
 - IGP (Interior Gateway Protocols) 🛤️:
 - Distance Vector: Protocols like RIP and IGRP that determine the best path based on the distance to the destination.
 - Link State: Protocols like OSPF and ISIS that use the state of each link in the network to build a comprehensive map for routing decisions.
 - Hybrid: Protocols like EIGRP that combine elements of both Distance Vector and Link State protocols.
 - EGP (Exterior Gateway Protocols) 🌐:
 - Path Vector: Protocols like BGP that are used for routing between autonomous systems and maintain path information to ensure efficient data transfer.

Los **Routing Protocols** se pueden clasificar en tres categorías principales:  

1. **`Static Routing`** → Rutas **configuradas manualmente** por el administrador, sin cambios automáticos.  
2. **`Dynamic Routing`** → Los routers **aprenden y actualizan rutas automáticamente** según la topología de la red.  
3. **`Default Routing`** → Ruta predefinida usada cuando no hay una coincidencia específica en la routing table (eg 0.0.0.0 0.0.0.0 + Next Hop IP = cualquier destino no conocido). 

Dentro de **Dynamic Routing**, existen dos grandes tipos: `Interior Gateway Protocols (IGP)` and `External Gateway Protocols (EGP)`:  

### 🔄 IGP: Interior Gateway Protocols 

Los Interior Gateway Protocols (IGPs) intercambian información de enrutamiento **dentro de un único dominio de enrutamiento**.

1. **`Distance Vector Routing Protocols`** → Determinan la mejor ruta según la cantidad de **hops** hasta el destino y envían la routing table completa a los vecinos (Ej.: RIP, IGRP).  
2. **`Link-State Routing Protocols`** → Calculan la mejor ruta basándose en el **estado de los enlaces**, creando un mapa de la red y enviando solo actualizaciones específicas (Ej.: OSPF, IS-IS).  

### 🔄 EGP: External Gateway Protocols

Los **Exterior Gateway Protocols (EGP)** como **`BGP`**, se utilizan para el enrutamiento entre diferentes **Autonomous Systems (AS)** en Internet.  

Un **Autonomous System (AS)** es un grupo de redes IP bajo una misma administración y con una política de enrutamiento común. Cada **AS** tiene un número único llamado **ASN (Autonomous System Number)** asignado por **IANA (Internet Assigned Numbers Authority)** o los **RIR (Regional Internet Registries)**. (eg. **Google (AS15169)** Usa BGP para gestionar tráfico entre sus servidores y otros ISPs. ) 

## 🔹 Static Routing VS Dynamic Routing VS Default Routing



| **Tipo de Enrutamiento**     | **Descripción**                                                                                                                                       | **Ejemplos de Protocolo**                                      | **Ventajas**                                                                                                                          | **Desventajas**                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| **Static Routing**            | Rutas **configuradas manualmente** por el administrador, sin cambios automáticos.                                                                     | - No aplica (no hay protocolos específicos, ya que es manual) | ✔️ No consume CPU del router.<br>✔️ No genera tráfico innecesario en la red.<br>✔️ Solo el administrador puede modificar las rutas. | ❌ Requiere conocer manualmente la topología de la red.<br>❌ No es escalable en redes grandes.<br>❌ Si una ruta falla, el administrador debe actualizarla manualmente. |
| **Dynamic Routing**           | Los **routers aprenden y actualizan rutas automáticamente** según la topología de la red.                                                             | - **RIP (Routing Information Protocol)**<br>- **OSPF (Open Shortest Path First)**<br>- **BGP (Border Gateway Protocol)** | ✔️ Más fácil de configurar en redes grandes.<br>✔️ Ajusta rutas dinámicamente en caso de fallos.<br>✔️ Permite **load balancing** sobre múltiples enlaces. | ❌ Consume más CPU y RAM del router.<br>❌ Utiliza ancho de banda para intercambiar información. |
| **Default Routing**           | Ruta predefinida usada cuando no hay una coincidencia específica en la routing table (ej. 0.0.0.0 0.0.0.0 + GW = cualquier destino no conocido). | - No aplica (se configura manualmente, no es un protocolo específico) | ✔️ Simplifica la configuración.<br>✔️ Usada comúnmente en redes pequeñas o en el borde de una red. | ❌ No es flexible para redes grandes.<br>❌ Solo maneja rutas no específicas. |



## 🔹 Dynamic Routing: Distance Vector VS Link-State VS Path Vector


| **Protocolo de Enrutamiento**         | **Descripción**                                                                                                                                          | **Ejemplos**                                                                                     | **Ventajas**                                                                                                                                                          | **Desventajas**                                                                                                                       |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| **Distance Vector Routing Protocols** | Determinan la mejor ruta basándose en la **distancia** (cantidad de **hops** hasta el destino). Utilizan el **Bellman-Ford Algorithm** y envían periódicamente la **routing table** completa a los routers vecinos. | - **RIP (Routing Information Protocol)**<br>- **IGRP (Interior Gateway Routing Protocol)**<br>- **EIGRP (Enhanced Interior Gateway Routing Protocol)** (híbrido, pero basado en Distance Vector) | ✔️ Fácil de configurar y mantener.<br>✔️ Funciona bien en redes pequeñas y medianas.                                                                                                                                  | ❌ Convergencia más lenta que los protocolos de **Link-State**.                                                                            |
| **Link-State Routing Protocols**      | Determinan la mejor ruta basándose en el **estado de los enlaces** en lugar de la cantidad de **hops**. Utilizan el **Dijkstra Algorithm (Shortest Path First - SPF)** para calcular la mejor ruta y envían solo **actualizaciones específicas** en caso de cambios en la topología. | - **OSPF (Open Shortest Path First)**<br>- **IS-IS (Intermediate System to Intermediate System)** | ✔️ Convergencia rápida y eficiente en comparación con Distance Vector.<br>✔️ Uso optimizado del ancho de banda al no enviar la tabla completa.<br>✔️ Escalable y adecuado para redes grandes y complejas.<br>✔️ Menos propenso a **routing loops**. | ❌ Mayor consumo de **CPU y memoria** debido a la complejidad del cálculo SPF.<br>❌ Más difícil de configurar y administrar en comparación con Distance Vector. |
| **Path Vector Routing Protocols**     | Determinan la mejor ruta utilizando un **vector de caminos**, lo que significa que cada router mantiene una lista de rutas completas hacia los destinos, incluyendo información sobre los routers intermedios por los que pasan esos caminos. El protocolo más conocido en este grupo es **BGP (Border Gateway Protocol)**. | - **BGP (Border Gateway Protocol)** | ✔️ Escalable y adecuado para redes de gran tamaño, como Internet.<br>✔️ Permite políticas de enrutamiento avanzadas mediante la manipulación de atributos como **AS Path**, **Next Hop**, y **Local Preference**.<br>✔️ Redundancia y robustez debido a la capacidad de seleccionar múltiples rutas de acceso. | ❌ Convergencia más lenta que los protocolos **Link-State** debido a la cantidad de rutas que deben intercambiarse.<br>❌ Configuración y mantenimiento más complejos, especialmente en redes grandes con múltiples **AS**.<br>❌ Susceptible a **routing loops** si no se implementan correctamente las políticas de control de rutas. |


## 📌 Comparativa de Routing Protocols  

Esta tabla detalla las diferencias clave entre los distintos **Routing Protocols**, incluyendo sus ventajas, desventajas y aplicaciones en la vida real.  

| **Característica**          | **Static Routing**             | **RIP (v1/v2)**                       | **IGRP**                              | **EIGRP**                             | **OSPF**                              | **IS-IS**                             | **BGP**                               |
|----------------------------|--------------------------------|----------------------------------------|----------------------------------------|----------------------------------------|----------------------------------------|----------------------------------------|----------------------------------------|
| **Tipo**                   | Manual                         | Distance Vector                        | Distance Vector                        | Distance Vector (Híbrido)              | Link-State                            | Link-State                            | Path Vector                           |
| **Dinámico**               | ❌ No                          | ✅ Sí                                  | ✅ Sí                                  | ✅ Sí                                  | ✅ Sí                                  | ✅ Sí                                  | ✅ Sí                                  |
| **Escalabilidad**          | 🚫 Limitado                    | 🚫 Pequeñas redes                      | 🔸 Medianas redes                      | ✅ Medianas y grandes redes            | ✅ Grandes redes                       | ✅ Grandes redes                       | 🌍 Muy grandes redes (Internet)       |
| **Convergencia**           | ⚡ Inmediata (pero manual)      | 🐢 Lenta                               | 🐢 Lenta                               | ⚡ Rápida                              | ⚡ Rápida                              | ⚡ Rápida                              | 🐢 Lenta                              |
| **Consumo de CPU/RAM**     | 🔹 Bajo                        | 🔸 Bajo                                | 🔸 Medio                               | 🔸 Medio                               | 🔺 Alto                                | 🔺 Alto                                | 🔺 Alto                                |
| **Uso de Ancho de Banda**  | ❌ No usa                      | 🔸 Alto (envía toda la routing table)  | 🔸 Alto                                | 🔹 Bajo (solo cambios)                 | 🔹 Bajo (solo cambios)                 | 🔹 Bajo (solo cambios)                 | 🔺 Alto (grandes volúmenes de datos)  |
| **Soporta VLSM/Subnets**   | ✅ Sí                          | 🚫 No (RIP v1) / ✅ Sí (RIP v2)        | 🚫 No                                  | ✅ Sí                                  | ✅ Sí                                  | ✅ Sí                                  | ✅ Sí                                  |
| **Prevención de Loops**    | 🚫 No                          | 🚫 No (usa hop count)                 | 🔸 Parcial                             | ✅ Sí (DUAL Algorithm)                 | ✅ Sí (SPF Algorithm)                  | ✅ Sí (SPF Algorithm)                  | ✅ Sí (Path Vector)                    |
| **Métrica**                | -                              | Hop Count (máx. 15)                   | Bandwidth + Delay                      | Bandwidth + Delay + Load + Reliability | Cost (Bandwidth)                      | Cost (IS Reachability)                | Path Attributes (AS Path, Weight, etc.) |
| **Tamaño de Red Ideal**    | 🔸 Pequeña                     | 🔸 Pequeña (<15 hops)                  | 🔸 Mediana                             | ✅ Mediana/Grande                      | ✅ Grande                              | ✅ Grande                              | 🌍 Global (ISPs, Internet)            |
| **Casos de Uso**           | Pequeñas oficinas, redes fijas | Pequeñas empresas, redes simples      | Redes medianas (Cisco propietario)     | Empresas, redes corporativas          | ISP, grandes empresas, WANs           | ISPs, proveedores backbone            | Interconexión de ISPs (Internet)      |
| **No recomendado para**    | Redes dinámicas o grandes      | Redes grandes, entornos críticos      | Redes modernas (obsoleto)              | ISP o entornos masivos                | Pequeñas redes (configuración compleja) | Pequeñas redes (configuración compleja) | Redes locales o pequeñas empresas     |

- **Para redes pequeñas**, el **Static Routing** o **RIP** pueden ser suficientes.  
- **Para empresas medianas y grandes**, **EIGRP** (si usan Cisco) o **OSPF** son las mejores opciones.  
- **Para ISPs y redes de alto tráfico**, **IS-IS** o **BGP** son esenciales.  
- **BGP es el estándar en Internet**, pero **no es necesario** en redes privadas a menos que se manejen múltiples **Autonomous Systems**.


## CCNA Routing CheatSheet

![image](https://github.com/user-attachments/assets/05441ab4-5c08-4f39-a892-d9f45433abb4)



## Resources

- https://edukedar.com/routing-protocols/#gsc.tab=0
- https://www.baeldung.com/cs/routing-igp-egp-protocols
- https://en.wikipedia.org/wiki/Routing_protocol#Routed_protocols



