# 🔥🧱🛡️ Cisco: `SD-WAN: Components`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `SD-WAN`

---

<br>

# Cisco SD WAN Components

- vManage : Management Plane
- vSmart : Control Plane
- vBond : Orchestrator
- vEdge : Data Plane

![image](https://github.com/user-attachments/assets/298ff229-8cdc-4b55-aabc-48216a4a9aff)

![image](https://github.com/user-attachments/assets/e8b3c4ac-01b3-4bec-bbdb-c1f1050a162d)

![image](https://github.com/user-attachments/assets/d7e32e13-88d0-4dd2-b034-c8932d857082)


## vManage : Management Plane

- Installation: Local Data Center, Remote Data Center (other vendor) or Cisco Cloud Based
- Fuction: NMS (Network Management System) eg, solarwinds, zabbix, graffana, etc. da un dashboard.
- Dashboard: Muesdtra la telemetría de la red que viene del Dataplane vEdge
- Monitor: Monitorea todo tipo de alertas
- Plantillas: Politicas de ingenieria del trafico del Data Plane
- Puede tener redundancia, pero no 2 activos al mismo tiempo. 

![image](https://github.com/user-attachments/assets/2b4ae49f-330c-4d87-a60f-88e4374d3e35)

Capacidades: 

![image](https://github.com/user-attachments/assets/a0f59c06-2617-47eb-8197-bce87d6c93fc)

## vSmart : Control Plane

- Installation: Local Data Center, Remote Data Center (other vendor) or Cisco Cloud Based
- Fuction: Cerebro de la arquitectura SD.-WAN (administra routing, politicas, QoS y elección de ruta)
- Anuncia ruta, politica de data plane, politicas de seguridad (mini-firewall). 
- Todo el Data Plane se empareja con el vSmart, y el vSMart lo enruta
- Los clusters deben estar conectados por cable uno al otro para lograr la latencia minima. 

![image](https://github.com/user-attachments/assets/315630f4-f88f-47bf-9923-444b5b7a9a20)

## vBond: Orchestrator

- Autentica: Valida que todos los elementos, sean elementos de nuestra red (vManage, vSmart, vEdge) y no sean falsos creados por un adversario/hunter

![image](https://github.com/user-attachments/assets/21a15b84-e086-4113-9891-3a9e92d3d9d9)

## vEdge: Data Plane

You don't typically need a separate "traditional router" when using SD-WAN, as an SD-WAN appliance essentially functions as a router itself, managing traffic routing and connections across multiple WAN links through a centralized controller, effectively replacing the need for individual routers at each branch location; however, depending on your network setup, you might still need a router at the edge of your network to connect to the SD-WAN appliance. (Some Cisco Routers can bu upgraded to Edge devices called "cEdge" devices)

- On-Site: Debe de estar localmente en la red LAN del cliente (puede ser virtual, pero local)
- Tunel entre vSmart: Conecta al vSmart con OMP, el vSmart le comparte las rutas
- Tunel entre sitios: IPsec o GRE entre sitios
- Como su nombre lo dice: está en el borde/edge, es como si fuera el router, conectado directo a la WAN (internet, mpls, 4g, etc)

![image](https://github.com/user-attachments/assets/10953c05-5a22-43fb-ad44-6ff76777220f)

![image](https://github.com/user-attachments/assets/94b99005-0a43-4e75-b135-27c247f59af0)

## cEdge: Data Plane

Similar al vEdge, pero usado en Routers normales Cisco que pueden ser upgeadeados con un software que los hace compatibles con SD-WAN, es decir, equipos edge a la SDWAN. 

![image](https://github.com/user-attachments/assets/214f311d-0497-4a9c-89f0-970cfa35fb9a)

## vEdge: Family Products and capabilities

- Depende del equipo sus capacidades, en particular cambia mucho la seguridad. 

![image](https://github.com/user-attachments/assets/a5492dd2-47ea-4afc-ab11-1553a9b3d234)

![image](https://github.com/user-attachments/assets/edde6664-b02d-4e1f-b7c7-1de44dfbe24b)

## vAnalytics: Analytics / Extra

- Se conecta entre vManage y vAnalytics
- Monitoring & Analytics
- Graph & Tasty

![image](https://github.com/user-attachments/assets/46b8502d-cc73-4180-9b46-068be3ec23bc)

![image](https://github.com/user-attachments/assets/9c51a525-0177-405b-879e-e48c1811b054)

## Licencing example

![image](https://github.com/user-attachments/assets/e4b5e07e-1875-404d-9e02-a60c77275204)


# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=voaycjsoips&list=PLwAU7bA502wHJkVCke8ypTxZUeeV9bVZL
- https://youtu.be/zs8fDA1ZlzE?si=k57gh4tALBGl-svn
- https://youtu.be/zs8fDA1ZlzE?si=4u82LXB3zi96xxyc
- https://youtu.be/8lW_FQYhOp0?si=b5ul89wREtJMlA-J


  
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






