# 🔥🧱🛡️ Cisco: `SD-WAN: Introduction & Concept`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `Console Cable` `Serial`

---

# Configure SPAN and RSPAN

Tu escenario pide RSPAN (capturar en SW2 pero recibir la copia en SW1). 

## 3 Pasos de config

- Fuente (source): SW2, interfaz Gi1/0/1 (access en VLAN 10). _Esto ya está listo ya que es el puerto a moinitorear_

- Destino (destination): SW1, interfaz Gi1/0/11 (donde conectas Wireshark). Este será el core switch, tiene varios access switch conectados. 

- Transporte: una RSPAN VLAN (ej. 999) que viaje por los trunks entre SW2 ↔︎ SW1. LA VLAN DE RSPAN DEBE SER UNA NUEVA CONFIGURADA EN TODOS LOS SWITCHES, La VLAN RSPAN no se usa para usuarios u otro tráfico o subnet (dedícala solo a RSPAN).

## Configuración:

1. Crear la VLAN RSPAN en ambos switches:

````
enable
conf t
!
vlan 888
   name RSPAN-888
   remote-span
end

````

2. Permitir la VLAN RSPAN en los trunks (en todos los saltos) - Revisar que el comando sea ADD para no borrar otras VLANs!!!

````
enable
conf t
!
interface Range GigabitEthernet1/0/19-24
   switchport trunk allowed vlan ADD 999
end

````

3. Vericiar configuraciones

````
!
show interfaces trunk | include 999

!
show vlan remote-span
````

4. Definir la sesión RSPAN en el switch fuente (SW2) - _Donde está el AP o el aparato a monitorear_

Notas:

- `both` copia RX y TX. Puedes usar `tx` o `rx` si quieres solo una dirección.
- Si en vez de un puerto quisieras toda la VLAN 10 (VSPAN), usa: `monitor session 10 source vlan 10`

````
enable
conf t
!

! # Sesion 10: fuente = Gi1/0/1 (ambas direcciones)
monitor session 10 source interface gi1/0/1 both

! # Destino = VLAN remota 999 (RSPAN)
monitor session 10 destination remote vlan 999

end

! # Verifica
show monitor session 10

````

4. Terminar la sesión en el switch destino (SW1) _Donde conectaremos el wireshark para monitorear_

Ahora recibes ese “túnel” en la VLAN 999 y lo sacas por tu puerto de análisis Gi1/0/11:

````
enable
conf t
!

! # Sesión 10: fuente = VLAN remota 999
monitor session 10 source remote vlan 999

! # Destino = tu puerto de captura (conecta aquí el laptop con Wireshark)
monitor session 10 destination interface gi1/0/11

end

! # Verifica
show monitor session 10
show interface status | include Gi1/0/11


````

Importante:

- El puerto destino entra en modo monitor (no envía tráfico de usuario, no puede ser trunk, ni pertenecer a un port-channel, ni tener port-security).
- Conecta tu PC con Wireshark a Gi1/0/11 y listo!!!

5. Validaciones

````
show vlan remote-span
show monitor session all
show interfaces trunk | include 999
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
conf t
!

no monitor session 10
end

!
!

````

7. Opcional: conservar la `VLAN 999` para futuras capturas o borrarla si ya no se usará.






## Ejemplo:


### SWITCH1 - WIRESHARK CONNECTED

````
SW1-Fz3r0#

SW1-Fz3r0#enable
SW1-Fz3r0#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW1-Fz3r0(config)#!
SW1-Fz3r0(config)#vlan 888
SW1-Fz3r0(config-vlan)#   name RSPAN-888
SW1-Fz3r0(config-vlan)#   remote-span
SW1-Fz3r0(config-vlan)#end
SW1-Fz3r0#
SW1-Fz3r0#
SW1-Fz3r0#config t
Enter configuration commands, one per line.  End with CNTL/Z.
SW1-Fz3r0(config)#interface Range GigabitEthernet1/0/19-24
SW1-Fz3r0(config-if-range)#   switchport trunk allowed vlan ADD 999
SW1-Fz3r0(config-if-range)#end
SW1-Fz3r0#
SW1-Fz3r0#
SW1-Fz3r0#!
SW1-Fz3r0#show interfaces trunk | include 999
Gi1/0/23    2-100,999
SW1-Fz3r0#
SW1-Fz3r0#!
SW1-Fz3r0#show vlan remote-span

Remote SPAN VLANs
------------------------------------------------------------------------------
888
SW1-Fz3r0#
SW1-Fz3r0#
SW1-Fz3r0#enable
SW1-Fz3r0#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW1-Fz3r0(config)#!
SW1-Fz3r0(config)#
SW1-Fz3r0(config)#! # SesiC3n 10: fuente = VLAN remota 999
SW1-Fz3r0(config)#monitor session 10 source remote vlan 999
SW1-Fz3r0(config)#
SW1-Fz3r0(config)#$to de captura (conecta aquC- el laptop con Wireshark)     
SW1-Fz3r0(config)#monitor session 10 destination interface gi1/0/11
SW1-Fz3r0(config)#
SW1-Fz3r0(config)#end
SW1-Fz3r0#
SW1-Fz3r0#! # Verifica
SW1-Fz3r0#show monitor session 10
Session 10
----------
Type                   : Remote Destination Session
Source RSPAN VLAN      : 999
Destination Ports      : Gi1/0/11
    Encapsulation      : Native
          Ingress      : Disabled


SW1-Fz3r0#show interface status | include Gi1/0/11
Gi1/0/11                     notconnect   1            auto   auto 10/100/1000BaseTX
SW1-Fz3r0#
SW1-Fz3r0#
````

### SWITCH2 - AP CONNECTED


````
SW2-Fz3r0#

SW2-Fz3r0#enable
SW2-Fz3r0#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW2-Fz3r0(config)#!
SW2-Fz3r0(config)#vlan 888
SW2-Fz3r0(config-vlan)#   name RSPAN-888
SW2-Fz3r0(config-vlan)#   remote-span
SW2-Fz3r0(config-vlan)#end
SW2-Fz3r0#
SW2-Fz3r0#
SW2-Fz3r0#config t
Enter configuration commands, one per line.  End with CNTL/Z.
SW2-Fz3r0(config)#interface Range GigabitEthernet1/0/19-24
SW2-Fz3r0(config-if-range)#   switchport trunk allowed vlan ADD 999
SW2-Fz3r0(config-if-range)#end
SW2-Fz3r0#
SW2-Fz3r0#
SW2-Fz3r0#!
SW2-Fz3r0#show interfaces trunk | include 999
Gi1/0/23    2-100,999
SW2-Fz3r0#
SW2-Fz3r0#!
SW2-Fz3r0#show vlan remote-span

Remote SPAN VLANs
------------------------------------------------------------------------------
888
SW2-Fz3r0#
SW2-Fz3r0#
SW2-Fz3r0#enable
SW2-Fz3r0#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW2-Fz3r0(config)#!
SW2-Fz3r0(config)#
SW2-Fz3r0(config)#! # Sesion 10: fuente = Gi1/0/1 (ambas direcciones)
SW2-Fz3r0(config)#monitor session 10 source interface gi1/0/1 both
SW2-Fz3r0(config)#
SW2-Fz3r0(config)#! # Destino = VLAN remota 999 (RSPAN)
SW2-Fz3r0(config)#monitor session 10 destination remote vlan 999
SW2-Fz3r0(config)#
SW2-Fz3r0(config)#end
SW2-Fz3r0#
SW2-Fz3r0#! # Verifica
SW2-Fz3r0#show monitor session 10
Session 10
----------
Type                   : Remote Source Session
Source Ports           : 
    Both               : Gi1/0/1
Dest RSPAN VLAN        : 999


SW2-Fz3r0#
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
