# 🔥🧱🛡️ Cisco: `SD-WAN: Transport Locators (TLOCS)`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `SD-WAN` `OMP` `Overlay Manangemt Protocol (OMP)`

---

<br>

# 📝❓📄 `Index`

- 

# Transport Locators (TLOCS)

- **Simple. Los TLOCs son lo que identifica a un vEdge en su sitio y configuraciones**

# 3 Rutas OMP

Es importante siempre recordar los 3 tipos de ruta OMP: 

1. Rutas OMP o vRoutes: prefijos que pertenecen al sitio local es decir la LAN de la branc h de un cliente. (la LAN puede tener switches, routers, etc. donde hay redes lan internas como 192.168.1.0/24, esa es una vRoute)
2. Service Route: Todas las redes que se publican, pero son para servicios exclusivos para todos los vEdge
3. TLOCs: 

![image](https://github.com/user-attachments/assets/bc400377-4566-4fd4-bac8-855db6743119)


## Rutas OMP o vRoutes:

![image](https://github.com/user-attachments/assets/348fad88-76d5-4e0d-be57-654b7c1ade77)

## Service Route

![image](https://github.com/user-attachments/assets/80d29754-7104-4058-9c1e-0f8439837d7b)

## TLOC

Un TLOC (Transport Locator) en Cisco SD-WAN es un identificador que representa una ruta o enlace de transporte en la red. El TLOC se utiliza para identificar los diferentes transportes (como MPLS, Internet, 4G, etc.) a través de los cuales los dispositivos en la red SD-WAN pueden comunicarse.

En términos simples, el TLOC se compone de los siguientes elementos:

- IP de Transporte: La dirección IP del dispositivo en la red que está proporcionando el transporte.
- Color de TLOC: Es una etiqueta que define la calidad o tipo del enlace de transporte (por ejemplo, "MPLS", "Internet", "4G", etc.). Este color ayuda a priorizar y seleccionar el transporte más adecuado según las políticas definidas.

El TLOC se utiliza en el contexto de la control plane en Cisco SD-WAN, especialmente en el protocolo Overlay Management Protocol (OMP), que facilita la comunicación entre los dispositivos de la red. Es una pieza fundamental para que el controlador SD-WAN (vBond) y los dispositivos de borde (vEdge o CPE) puedan intercambiar información sobre las rutas y los enlaces disponibles para el enrutamiento dinámico.

Función principal:

- Encapsulamiento de tráfico: Los TLOCs permiten que el tráfico de usuario sea encapsulado y enviado a través del transporte más adecuado.
- Selección de rutas: Ayudan en la toma de decisiones sobre cuál es el mejor transporte a utilizar en función de las políticas definidas en la red SD-WAN.

![image](https://github.com/user-attachments/assets/8d2415b1-81be-4fe4-988c-0ef74e2a1ccb)

### TLOC: Atributos

NOTA: IPsec es mejor que GRE  por sgeuridad. 

![image](https://github.com/user-attachments/assets/918e5a38-049d-488a-ab9e-b1dcc685181d)

### TLOV: Valores

![image](https://github.com/user-attachments/assets/e34e1905-8858-469c-8b72-b256ea2a4805)


# 📚🗂️🎥 Resources

- https://youtu.be/9wpHWhDFVpo?si=ydFINrvyD37ckba-



  
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

