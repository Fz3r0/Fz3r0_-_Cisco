# 🔥🧱🛡️ Cisco Nexus: 

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)


### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Datacenter` `Cisco Nexus` `NX-OS`

---

<br>

# 📝❓📄 `Index`

- 

# 


## Classic VLAN architecture on a LAN

Para entender VXLAN primero veamos como se hace tradicionalmente una LAN a traves de VLAN para comunicar PC-A y PC-B a traves de switches capa 2 usando VLAN, los enalces de nemedio son los clasicos trunk y hacia las PC los clasicos access, ambas PC tienen la VLAN 10, y si tuvieran diferentes vlan y subnet, deberian ser routeados entre si con algo capa 3, ya sea un mismo switch L3, un router, firewall etc, lo clasico! simplemente la PC a envia trafico con su MAC, y los dispositivos L2 van aprendiendo tu MAC y la guardan en sus tablas, es decirt se genera un broadcast para buscar la MAC de destino via L2 (ARP). 

![image](https://github.com/user-attachments/assets/6ca80d48-648c-4084-947b-5479b96325a5)

el chiste de VXLAN es elminiar la conuicacion L2, vlans tags, trunks, stp... asi es!!! no usar enalces capa 3 y dejar de usar el concepto tradicional, la idea de VXLAN es comuinicar PC con PC b ahora usando conexiones L3 en lugar de L2, es decir, sinusar VLANs, STP, etc.

Esto surve por tencolofgias tipo SDN, como Cisco SD-WAN y solo est adisponible par anuevos rquipos cisoc XE o Nexus que es la nueva gama d eequipos de cisco, como la serie 9000 de cisco o la nexus.   

![image](https://github.com/user-attachments/assets/3b5dab0a-1ea5-4d30-b990-82a35931aedd)

![image](https://github.com/user-attachments/assets/75f830c2-1e1f-47c5-9b6d-42ceb4077e31)

Todos tienen capacidad de L3 Router/Switch es decir hacer rutas estaticas, OSPF, DHCP Pool, aunque limitadas por temas de licencias. Por ejemplo la licencia advantage es la que realmente te habilita L3 y ser controlado por una controladora externa, como el Cisco DNA-Center. 

Que es el Ciusoc DNA center

Un sistema iperativo que automatizara la red y todos los quipos cisco se registran en el DNA center y esta controladora los puede monitorear y congfigurar remotamente, dales roels como acceso, borde, ser ocntorl plane, comunicar VRFs, etc. es decir, adminsitrar toda la red desde ahi. 

Y la parte de capa 2 en realidad se contruye con VXLAN. 

VXLAN

Lo que se hara en una VXLAN a diferencia de la L2 normal con VLANs y lo tradicional o legacy, es primero utilizar equipos capaces de VXLAN como serie 9000. 
Que ninguna conexion sea capa 2, sino que ahora todas las conexxiones sea capa 3. si todo. Es decir cada interfaz tendra una direccion IP y necesitaria configurar un enrutamiento, por ejemplo un routing dinamico como OSPF, ISIS, etc.  Por ejemplo ahi la reduncacia tendir aque se OSPF, entonces ya no necesito STP y lidiar que un puerto se va a bloeuaqr o esas cosas ya consideradas del pasado, chavo! 

![image](https://github.com/user-attachments/assets/f2a90c1a-cea4-43e5-8bbe-89ac96b06b88)


Ojo, obviamente las conexiones que van de un switch a la PC si será layer 2, de acceso normalito, la PC ni se dara cuenta. para esto alfgo que se llama dominio de puente o dominio de capa 2 "bridge domain" o "VXLAN"!!! . 

Es decir, una VXLAN es un segmento de capa 2 que esta en el borde hacia el usuario, que en relaidafd es una base de datos o tabla de direcciones mac o "bridge domain". En L2 sabemos que una VLAN lleva tambien una direccion MAC asociada cuando se aprende el switch las direcicones, por ejemplo F0:f0:f0 percecene a la vlan 10 y puede comunicarse con todas las VLAN 10 pero no con otras VLAN (a menos que sean routeadas en capa 3), asi que vxlan cumnple con al funcion de la VLAN. 

Por ejempo, en un switch conextado a la PC A yo peudo crear la VXLAN 700, asi como en el switch pegado en la PC-B, esto se llama en realidad VNI. Enoncers el puerot lo poondr een la VXLAN/VNI 700 de ambos lados, y asi cuando la PC haga un comun y corriente y tradicional ARP y se comunique por primera vez, el switch se la aprendera en ese segmento de capa 2 VXLAN/VNI 700. 





# 📚🗂️🎥 Resources

- https://www.youtube.com/live/uW--KhRzdEk?si=ujMneJdPkihxBtT4
- [Cisco DNA Center](https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2023/pdf/BRKOPS-2077.pdf)



  
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

