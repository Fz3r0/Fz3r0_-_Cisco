# 📌 Routing Protocols 

En redes de computadoras, los **Routing Protocols** son un conjunto de reglas utilizadas por los routers para intercambiar información sobre rutas dentro de una red.  

Su función principal **no es mover paquetes de datos**, sino actualizar y gestionar la **routing table**, que contiene información sobre las mejores rutas hacia diferentes destinos.  

Las rutas aprendidas a través de un **Routing Protocol** se denominan **dynamic routes**.  

El objetivo del **Dynamic Routing** es garantizar una comunicación eficiente entre routers, con beneficios como:  

- ✅ No es necesario configurar manualmente cada ruta, como en **Static Routing**.  
- ✅ Los routers pueden anunciar cambios en la red y reaccionar ante fallos en los enlaces.  

## 🔹 Tipos de Routing Protocols  

![image](https://github.com/user-attachments/assets/921edaf0-8c3c-4160-947b-32bd2f60e78e)

Los **Routing Protocols** se pueden clasificar en tres categorías principales:  

1. **`Static Routing`** → Rutas **configuradas manualmente** por el administrador, sin cambios automáticos.  
2. **`Dynamic Routing`** → Los routers **aprenden y actualizan rutas automáticamente** según la topología de la red.  
3. **`Default Routing`** → Ruta predefinida usada cuando no hay una coincidencia específica en la routing table (eg 0.0.0.0 0.0.0.0 + GW = cualquier destino no conocido). 

Dentro de **Dynamic Routing**, existen dos grandes tipos: `Interior Gateway Protocols (IGP)` and `External Gateway Protocols (EGP)`:  

### IGP: Interior Gateway Protocols 

1. **`Distance Vector Routing Protocols`** → Determinan la mejor ruta según la cantidad de **hops** hasta el destino y envían la routing table completa a los vecinos (Ej.: RIP, IGRP).  
2. **`Link-State Routing Protocols`** → Calculan la mejor ruta basándose en el **estado de los enlaces**, creando un mapa de la red y enviando solo actualizaciones específicas (Ej.: OSPF, IS-IS).  

### EGP: External Gateway Protocols

Los **Exterior Gateway Protocols (EGP)** como **`BGP`**, se utilizan para el enrutamiento entre diferentes **Autonomous Systems (AS)** en Internet.  

Un **Autonomous System (AS)** es un grupo de redes IP bajo una misma administración y con una política de enrutamiento común. Cada **AS** tiene un número único llamado **ASN (Autonomous System Number)** asignado por **IANA (Internet Assigned Numbers Authority)** o los **RIR (Regional Internet Registries)**. (eg. **Google (AS15169)** Usa BGP para gestionar tráfico entre sus servidores y otros ISPs. ) 


## 🔹 Static Routing  


El **Static Routing** implica la configuración manual de rutas en cada router. Es más seguro, pero menos flexible.  

### ✅ Ventajas  

- ✔️ No consume CPU del router.  
- ✔️ No genera tráfico innecesario en la red.  
- ✔️ Solo el administrador puede modificar las rutas.  

### ❌ Desventajas  

- ❌ Requiere conocer manualmente la topología de la red.  
- ❌ No es escalable en redes grandes.  
- ❌ Si una ruta falla, el administrador debe actualizarla manualmente.  



## 🔹 Dynamic Routing  

Los **Dynamic Routing Protocols** permiten que los routers aprendan y actualicen rutas automáticamente en función de los cambios en la red.  

### ✅ Ventajas  

- ✔️ Más fácil de configurar en redes grandes.  
- ✔️ Ajusta rutas dinámicamente en caso de fallos.  
- ✔️ Permite **load balancing** sobre múltiples enlaces.  

### ❌ Desventajas  

- ❌ Consume más CPU y RAM del router.  
- ❌ Utiliza ancho de banda para intercambiar información.  



## 🔹 Distance Vector Routing Protocols  

Estos protocolos determinan la mejor ruta basándose en la **distancia** (cantidad de **hops** hasta el destino).  
Utilizan el **Bellman-Ford Algorithm** y envían periódicamente la **routing table** completa a los routers vecinos.  

📌 **Ejemplos de Distance Vector Routing Protocols:** 

- **RIP (Routing Information Protocol)**  
- **IGRP (Interior Gateway Routing Protocol)**  
- **EIGRP (Enhanced Interior Gateway Routing Protocol)** (híbrido, pero basado en Distance Vector)  

### ✅ Ventajas 

- ✔️ Fácil de configurar y mantener.  
- ✔️ Funciona bien en redes pequeñas y medianas.  

### ❌ Desventajas  

- ❌ Convergencia más lenta que los protocolos de **Link-State**




## 🔹 Link-State Routing Protocols  

Estos protocolos determinan la mejor ruta basándose en el **estado de los enlaces** en lugar de la cantidad de **hops**.  
Utilizan el **Dijkstra Algorithm (Shortest Path First - SPF)** para calcular la mejor ruta y envían solo **actualizaciones específicas** en caso de cambios en la topología.  

📌 **Ejemplos de Link-State Routing Protocols:**  

- **OSPF (Open Shortest Path First)**  
- **IS-IS (Intermediate System to Intermediate System)**  

### ✅ Ventajas  

- ✔️ Convergencia rápida y eficiente en comparación con Distance Vector.  
- ✔️ Uso optimizado del ancho de banda al no enviar la tabla completa.  
- ✔️ Escalable y adecuado para redes grandes y complejas.  
- ✔️ Menos propenso a **routing loops**.  

### ❌ Desventajas  

- ❌ Mayor consumo de **CPU y memoria** debido a la complejidad del cálculo SPF.  
- ❌ Más difícil de configurar y administrar en comparación con Distance Vector.


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





## Resources

- https://edukedar.com/routing-protocols/#gsc.tab=0



