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





## 🌐 Distance Vector VS Link-State

When discussing routing protocols, two major categories come up: **Distance Vector** and **Link-State**. Both have distinct methods of determining the best path through a network, and understanding the differences is crucial.

### 🔄 What is Link-State?

**OSPF** is considered a **Link-State** protocol

- Unlike Distance Vector protocols (such as RIP), Link-State focuses on the **state of the links** rather than the distance.
- The key factor it considers when determining the best path is **`Bandwidth (BW)`** value. (_EIGRP can use up to 5 values, including bandwidth, while OSPF only uses bandwidth. That's why EIGRP has a lower Administrative Distance (AD) than OSPF._)  
- This means that if there are two possible paths, it will prefer the one with the highest total bandwidth, even if it has more physical hops.

![image](https://github.com/user-attachments/assets/876ec9eb-db13-4eb9-8174-097fb1b22e06)

OSPF is considered a **Link State / Dynamic Routing** protocol, it means that **each router in a network keeps an updated map of the entire network in a database, including each router and each route of the network**. 

- When a change occurs, like a new route or a failure, routers share this updated information with all others, so every router has the same view of the network.
- This helps routers calculate the best paths efficiently and react quickly to changes.

### 🛠️ Key Characteristics of Link-State Protocols:

| Feature                  | Link-State                                 |
|--------------------------|--------------------------------------------|
| **Routing Updates**       | Immediate updates sent to all routers in the network |
| **Network Knowledge**     | Full (routers have a complete map of the network) |
| **Convergence Time**      | Faster than Distance Vector                |
| **Loop Prevention**       | Uses SPF (Shortest Path First) algorithm to calculate best paths |
| **Example Protocols**     | OSPF, IS-IS                               |

**NOTE:** EIGRP is considered a **Distance Vector** protocol because it relies on routing-by-rumor, meaning routers exchange information only with their directly connected neighbors rather than having a complete network topology like OSPF. However, it's called **Hybrid** because it incorporates **Link-State-like** features, for example: Supporting advanced metrics (weights K1-K5) for better path selection.

### 🔄 What is Distance Vector?

- Unlike Link-State protocols (such as OSPF), Distance Vector focuses on **hop count or cumulative distance** rather than the state of links.  
- The key factor it considers when determining the best path is **the total metric** (e.g., hop count in RIP or composite metric in EIGRP).  
- Distance Vector protocols rely on **"routing-by-rumor"**, meaning routers exchange information only with their directly connected neighbors rather than maintaining a full network topology.  

![image](https://github.com/user-attachments/assets/1026df31-adaf-45b8-a9b7-cb6aec0eb24f)

RIP is a classic **Distance Vector / Dynamic Routing** protocol, which means:  

- Each router only knows the **next-hop and cost** to reach a destination but does not have a complete network view.  
- Routers periodically send updates to their **direct neighbors**, which propagate throughout the network.  
- **Slower convergence** and **prone to routing loops**, mitigated by features like **split horizon, route poisoning, and hold-down timers**.  

### 🛠️ Key Characteristics of Distance Vector Protocols:  

| Feature | Distance Vector |
|---------|----------------|
| **Routing Updates** | Periodic updates to neighbors only |
| **Network Knowledge** | Limited (routers only know next-hop info) |
| **Convergence Time** | Slower than Link-State |
| **Loop Prevention** | Split horizon, route poisoning, hold-down timers |
| **Example Protocols** | RIP, EIGRP (Hybrid) |

**NOTE:** EIGRP is technically a **Distance Vector protocol**, but it includes **Link-State-like optimizations**, such as advanced metric calculations (**K-values**) and topology tables, which is why it's called **Hybrid**.






















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






## Administrative Distance

![image](https://github.com/user-attachments/assets/da08d869-af14-481a-829a-815b5e5a89f6)

https://www.networkurge.com/2020/04/administrative-distance-of-ip-routing.html

to add: administrative distance: https://www.internetworks.in/2023/06/what-is-administrative-distance-how-to.html

![image](https://github.com/user-attachments/assets/03c78e70-994d-4cfa-b6e4-89e5c2c63a08)


## Resources

- https://edukedar.com/routing-protocols/#gsc.tab=0
- https://www.baeldung.com/cs/routing-igp-egp-protocols
- https://en.wikipedia.org/wiki/Routing_protocol#Routed_protocols
- https://www.networkurge.com/2020/04/administrative-distance-of-ip-routing.html



