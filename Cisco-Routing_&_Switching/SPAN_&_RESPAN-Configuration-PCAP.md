# 🔥🧱🛡️ Cisco: `SD-WAN: Introduction & Concept`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `Console Cable` `Serial`

---

# Configure SPAN and RSPAN

Para esta solución cubriremos 2 escenarios al mismo tiempo 

1. Escenario 1
    - En el mismo Switch-1 tendremos tanto la interfaz para capturar Wireshark (Gi 1/0/5), como 2 APs (Gi 1/0/11 y Gi 1/0/17) de los cuales queremos hacer el mirror de puerto para ver su tráfico.
    - Es decir, en el mismo Switch-1, configurar **3 sesiones diferentes de RSPAN** utilizando la **misma VLAN 888**.
    - 1 que "escucha", 2 que son el "espejo"

2. Escenario 2
    - Desde elo mismo Switch-1 y el mismo puerto de captura para wireshark (Gi 1/0/5), capturar 1 AP que está en Switch-2 Gi 1/0/17.
    - Es decir, desde Switch-1, configurar **1 sesiones de RSPAN** (escucha wireshark) y en Switch-1 configurar otra sesion de RSPAN, ambos utilizando la **misma VLAN 888**.
    - 1 que "escucha" (Switch-1), 1 que son el "espejo" (Switch-2)
      
Al final, con una sola VLAN 888 podremos escuchar todos los 3 APs al mismo tiempo, tanto los que están en el mismo switch de wireshark, tanto el remoto. 

Para llevar un orden limpio y no tener errores y no morir en el intento, aunque usemos switch remoto o sea el mismo SIEMPRE usaremos un número de sesion diferente de RSPAN, es decir tendremos: 

1. Switch-1 :: `RSPAN Sesion 10` - Wireshark Monitor (Destination)
2. Switch-1 :: `RSPAN Sesion 11` - Switch-1/AP-1 Mirror (Source)
3. Switch-1 :: `RSPAN Sesion 12` - Switch-1/AP-2 Mirror (Source)
4. Switch-2 :: `RSPAN Sesion 13` - Switch-2/AP-3 Mirror (Source)

## Configuración de RSPAN

Solo hay que seguir 3 sencillos pasos: 

1. Configurar VLAN y Trunks
2. Configurar Source
3. Configurar Destination

### 1. `Configurar VLAN y Trunks`

1. Crear la VLAN RSPAN en ambos switches, esta se convertirá un una VLAN dedicada para RSPAN y debe ser única. 

````
enable
configure terminal
!
vlan 888
   name RSPAN-888
   remote-span
end

````

2. Permitir la VLAN RSPAN en los trunks (en todos los uplinks que sean trunks a otros switches remotos que quieran ser monitoreados)

- `IMPORTANTE`: Revisar que el comando sea **ADD** para no borrar otras VLANs!!!

````
enable
configure terminal
!
interface GigabitEthernet 1/0/23 
   switchport trunk allowed vlan ADD 888
end

````

3. Vericiar configuraciones

````
!
show interfaces trunk | include 888

!
show vlan remote-span
````

4. Definir la sesión RSPAN en el switch **SOURCE (MIRROR)**  _Donde está el AP o el aparato a monitorear_

Notas:

- `both` copia RX y TX. Puedes usar `tx` o `rx` si quieres solo una dirección.
- Si en vez de un puerto quisieras toda la VLAN 10 (VSPAN), usa: `monitor session 10 source vlan 10`

````
enable
configure terminal
!

! # Sesion 11: fuente = Gi 1/0/11 (ambas direcciones)
monitor session 11 source interface Gi 1/0/11 both

! # Destino = VLAN remota 888 (RSPAN)
monitor session 11 destination remote vlan 888

end

! # Verifica
show monitor session 10

````

4. Terminar la sesión en el switch destino (SW1) _Donde conectaremos el wireshark para monitorear_

Ahora recibes ese “túnel” en la VLAN 888 y lo sacas por tu puerto de análisis Gi1/0/11:

````
enable
configure terminal
!

! # Sesión 10: fuente = VLAN remota 888
monitor session 10 source remote vlan 888

! # Destino = tu puerto de captura (conecta aquí el laptop con Wireshark)
monitor session 10 destination interface gi1/0/11

end

! # Verifica
show monitor session 10
show interface status | include Gi1/0/11


````

## Resultado

<img width="1062" height="209" alt="image" src="https://github.com/user-attachments/assets/e33ce204-2c5a-4cb2-8522-da7166394270" />

## Importante:

- El puerto destino entra en modo monitor (no envía tráfico de usuario, no puede ser trunk, ni pertenecer a un port-channel, ni tener port-security).
- Conecta tu PC con Wireshark a Gi1/0/11 y listo!!!

5. Validaciones

````
show vlan remote-span
show monitor session all
show interfaces trunk | include 888
````

6. Limpieza / quitar SPAN al terminal (Es importante para que el puerto deje de monitorear)

**IMPORTNAT!!!:**

**When configuring a SPAN/RSPAN session, neither the source port nor the destination port permanently lose their previous configuration.** 

- The source port continues to operate normally (e.g., access VLAN, security policies, QoS) while its traffic is simply mirrored.
- The destination port, on the other hand, temporarily ignores its normal switchport configuration (VLAN assignment, voice VLAN, STP, etc.) and becomes a dedicated monitor port that only outputs the mirrored traffic.
- **Once the SPAN session is removed, the destination port automatically returns to its original configuration as defined in the running-config, without requiring manual reconfiguration.**

En ambos switches:

````
enable
configure terminal
!

no monitor session 10
end

!
!

````

7. Opcional: conservar la `VLAN 888` para futuras capturas o borrarla si ya no se usará.


---

## Monitoreo final

Una vez teniendo todo configurado simplemente hay que prender y apagar interfaces a palcer a monitorear, por ejemplo:

- En donde vas a monitorear basciamente todo ese tiempo estará prendido como monitor con: 

````
!# Wireshark Switch Side:

! # Sesión 10: fuente = VLAN remota 888
monitor session 10 source remote vlan 888
! # Destino = tu puerto de captura (conecta aquí el laptop con Wireshark)
monitor session 10 destination interface gi1/0/11

! # Verifica
show monitor session 10
show interface status | include Gi1/0/11
````

- Donde vamos a monitorear, por ejemplo 3 APs, en 2 switches distinos prender ese monitoreo solo hay que hacer:

````
!# Client SIDE

! # Sesion 10: fuente = gi1/0/14 (ambas direcciones)
monitor session 10 source interface gi1/0/14 both
! # Destino = VLAN remota 888 (RSPAN)
monitor session 10 destination remote vlan 888

! # Verifica
show monitor session 10
````

Y ya que terminemos, en todos los swiutches usados solo correr: 

````
no monitor session 10
````


# 📚🗂️🎥 Resources

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
