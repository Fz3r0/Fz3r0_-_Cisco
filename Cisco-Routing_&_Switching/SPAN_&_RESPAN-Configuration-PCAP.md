# 🔥🧱🛡️ Cisco: `SD-WAN: Introduction & Concept`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `Console Cable` `Serial`

---





# 🛰️ Configure SPAN and RSPAN


Mirror traffic from three AP access ports to a single Wireshark capture laptop using a single RSPAN VLAN (888), covering:

1. Local sources on Switch-1
2. A remote source on Switch-2 — all delivered to the same destination port on Switch-1

## 🧭 Scenarios

1. Scenario 1 (all on Switch-1)

* Source interfaces (mirrors): Gi 1/0/11 and Gi 1/0/17 (APs)
* Destination (analyzer): Gi 1/0/5 (Wireshark laptop)
* Use three distinct monitor sessions on the same switch, all leveraging the same RSPAN VLAN 888:

  * Session 10: destination session (Wireshark)
  * Session 11: source Gi 1/0/11
  * Session 12: source Gi 1/0/17

2. Scenario 2 (remote source on Switch-2)

* From Switch-1, still capture with the same analyzer port Gi 1/0/5
* Add one source session on Switch-2 for AP on Gi 1/0/17, also using VLAN 888:

  * Session 13 on Switch-2: source Gi 1/0/11 (per your example text) or Gi 1/0/17 (per the description) — keep it consistent with the actual AP port you intend to mirror

Result: one RSPAN VLAN (888) fans in traffic from all three APs to the single analyzer port on Switch-1.

> Tip: Always use unique session numbers per switch and per source to stay organized and avoid overlap.

Session plan:

1. Switch-1 :: Session 10 — Wireshark destination
2. Switch-1 :: Session 11 — AP-1 source (Gi 1/0/11)
3. Switch-1 :: Session 12 — AP-2 source (Gi 1/0/17)
4. Switch-2 :: Session 13 — AP-3 source (Gi 1/0/11 or Gi 1/0/17, as applicable)

---

## 🛠️ RSPAN Configuration (3 steps)

### ⭕ 1) `Configure the RSPAN VLAN and trunks`

- Create the dedicated remote-span VLAN on both switches (VLAN 888):

```
enable
configure terminal
!
vlan 888
   name RSPAN-888
   remote-span
end

!
```

- Allow the VLAN 888 across all relevant 802.1Q trunks that interconnect the path between Switch-2 and Switch-1 (or any other switches).
- **IMPORTANT**: Use `ADD` so you don’t wipe the existing allowed VLAN list!!!

```
enable
configure terminal
!
interface GigabitEthernet 1/0/23
   switchport trunk allowed vlan ADD 888
end

!
```

Validate:

```
show interfaces trunk | include 888
show vlan remote-span
```

---

### ⭕ 2) `Configure the Source sessions` (mirror the AP ports)

- On the switch where the source interface physically resides (e.g. Access Points), define a monitor session with the access port as the source and the RSPAN VLAN as the destination.

Notes:

- `both` mirrors ingress and egress (RX and TX). You may use `rx` or `tx` if you only need one direction.
- To mirror an entire VLAN instead of a single port (VSPAN), use `monitor session <id> source vlan <vlan-id>`.

#### `AP-1 on Switch-1` :: `Session 11`:

```
enable
configure terminal
!
! Session 11: source = Gi 1/0/11 (both directions)
monitor session 11 source interface Gi 1/0/11 both
! Destination = remote RSPAN VLAN 888
monitor session 11 destination remote vlan 888
end

! Verify
show monitor session 11
show monitor session all
```

AP-2 on Switch-1 — Session 12:

```
enable
configure terminal
!
! Session 12: source = Gi 1/0/17 (both directions)
monitor session 12 source interface Gi 1/0/17 both
! Destination = remote RSPAN VLAN 888
monitor session 12 destination remote vlan 888
end

! Verify
show monitor session 12
show monitor session all
```

AP-3 on Switch-2 — Session 13 (same pattern on the remote switch):

```
enable
configure terminal
!
! Session 13: source = Gi 1/0/11 (both directions)
monitor session 13 source interface Gi 1/0/11 both
! Destination = remote RSPAN VLAN 888
monitor session 13 destination remote vlan 888
end

! Verify
show monitor session 13
show monitor session all
```

> Keep the session number unique per source. If your third AP is actually on Gi 1/0/17 of Switch-2, use that exact interface in Session 13.

---

### 3) Configure the Destination session (Wireshark analyzer on Switch-1)

Terminate the RSPAN VLAN at Switch-1 and forward it out your analyzer port.

```
enable
configure terminal
!
! Session 10: source = remote RSPAN VLAN 888
monitor session 10 source remote vlan 888
! Destination = analyzer interface (plug your Wireshark laptop here)
monitor session 10 destination interface gi 1/0/5
end

! Verify
show monitor session 10
show monitor session all
show interface status | include Gi1/0/5
```

---

## ✅ Expected Result and Validations

All mirrored traffic from:

* Switch-1 Gi 1/0/11
* Switch-1 Gi 1/0/17
* Switch-2 Gi 1/0/11 (or Gi 1/0/17, whichever AP you selected)

…is delivered out of Switch-1 Gi 1/0/5 to your Wireshark host.

Useful checks:

```
show vlan remote-span
show monitor session all
show interfaces trunk | include 888
```

### ⚠️ Important behavior of the destination port

* The destination port becomes a monitor (analyzer) port while the session is active. It does not forward user traffic, cannot be a trunk, cannot be part of an EtherChannel, and ignores features like voice VLAN or port-security. It simply emits the mirrored frames toward your analyzer.
* Once you remove the SPAN/RSPAN session, the destination port automatically returns to its configured switchport state as defined in the running-config. The source port always keeps operating normally while being mirrored.

---

## 🧹 Cleanup — remove SPAN when done

Do this on any switch where a session is configured.

```
enable
configure terminal
!
no monitor session 10
end
```

If you created multiple sessions, remove each explicitly:

```
no monitor session 10
no monitor session 11
no monitor session 12
no monitor session 13
```

---

## 📋 Quick Operations Playbook

Analyzer side (Switch-1):

```
! Wireshark switch side
monitor session 10 source remote vlan 888
monitor session 10 destination interface gi1/0/5

show monitor session 10
show interface status | include Gi1/0/5
```

AP mirrors (example on Switch-1):

```
! AP-1
monitor session 11 source interface Gi 1/0/11 both
monitor session 11 destination remote vlan 888

! AP-2
monitor session 12 source interface Gi 1/0/17 both
monitor session 12 destination remote vlan 888

show monitor session 11
show monitor session 12
show monitor session all
```

Shutdown when finished:

```
no monitor session 10
no monitor session 11
no monitor session 12
no monitor session 13
```

---

## 🧩 Platform Notes (good to know)

* Session limits vary by platform and software. Some Catalyst families only support a small number of concurrent SPAN/RSPAN sessions. If a command is rejected, you may have hit a platform limit.
* Interface naming can be flexible in the CLI parser, but on many systems it is typically written without a space (Gi1/0/5, GigabitEthernet1/0/23). Your current syntax may still work; be consistent with what your device accepts.
* Ensure VLAN 888 is allowed across every trunk hop between the source and destination switches, otherwise the RSPAN frames will not reach the destination session.

---

## 🧪 Troubleshooting Tips

* If you see no packets on Wireshark:

  * Verify `show vlan remote-span` on both ends.
  * Confirm `show interfaces trunk | include 888` along the full path.
  * Make sure the analyzer interface is actually the destination in Session 10 and is up/up.
  * Check that your source sessions are set to `both` (or the correct direction) and that the correct interfaces are referenced.
* If a command is not recognized:

  * You might be on a platform or software release with slightly different SPAN syntax or feature limits.

All set — this version should be drop-in, exam-grade, and field-ready. 🚀







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

1. Configurar RSPAN-VLAN y Trunks
2. Configurar Source (Interfaces Mirrors)
3. Configurar Destination (Interfaz Wireshark)

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

### 2. `Configurar Source` :: _Mirrors / APs a monitorear_

1. Definir la sesión RSPAN en el switch **SOURCE (MIRROR)**  _Donde está el AP o el X aparato a monitorear_

Notas:

- `both` copia RX y TX.
- Puedes usar `tx` o `rx` si quieres solo una dirección.
- Si en vez de un puerto quisieras toda la VLAN 10 (VSPAN), usa: `monitor session 11 source vlan 10`

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
show monitor session 11
show monitor session all

````

Hacer lo mismo con el segundo puerto (o la cantidad de puertos que sea necesario), solo NO REPETIR EL NÚMERO DE SESIÓN, ES SESIÓN POR PUERTO. 

````
enable
configure terminal
!

! # Sesion 12: fuente = Gi 1/0/17 (ambas direcciones)
monitor session 12 source interface Gi 1/0/17 both

! # Destino = VLAN remota 888 (RSPAN)
monitor session 12 destination remote vlan 888

end

! # Verifica
show monitor session 12
show monitor session all

````

Hacer lo mismo con cualquier otro switch que trasnporte la VLAN 888, en nuestro ejemplo con el switch 2, es tal cual la misma configuración: 

````
enable
configure terminal
!

! # Sesion 13: fuente = Gi 1/0/11 (ambas direcciones)
monitor session 13 source interface Gi 1/0/11 both

! # Destino = VLAN remota 888 (RSPAN)
monitor session 13 destination remote vlan 888

end

! # Verifica
show monitor session 13
show monitor session all

````

### 3. `Configurar Destination` :: _Monitor / Wireshark_

4. Todas las sesiones a monitorear van a terminar ena la sesión en el switch destino (SW1) _Donde conectaremos el wireshark para monitorear_

Ahora recibes ese “túnel” en la VLAN 888 y lo sacas por tu puerto de análisis Gi 1/0/5:

````
enable
configure terminal
!

! # Sesión 10: fuente = VLAN remota 888
monitor session 10 source remote vlan 888

! # Destino = tu puerto de captura (conecta aquí el laptop con Wireshark)
monitor session 10 destination interface gi 1/0/5

end

! # Verifica
show monitor session 10
show monitor session all
!
show interface status | include Gi1/0/5


````

## Resultado y Validaciones

Asu

<img width="1062" height="209" alt="image" src="https://github.com/user-attachments/assets/e33ce204-2c5a-4cb2-8522-da7166394270" />

- Importante:

- El puerto destino entra en modo monitor (no envía tráfico de usuario, no puede ser trunk, ni pertenecer a un port-channel, ni tener port-security), no es nada, simplemente un puerto monitor, por eso es importante regresarlo a la normalidad al final a menos que vaya a ser un SPAN dedicado para siempre. 


Validaciones

````
show vlan remote-span
show monitor session all
show interfaces trunk | include 888
````





## Limpieza / quitar SPAN al terminal (Es importante para que el puerto deje de monitorear)

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






## Monitoreo final

Una vez teniendo todo configurado simplemente hay que prender y apagar interfaces a palcer a monitorear, por ejemplo:

- En donde vas a monitorear basciamente todo ese tiempo estará prendido como monitor con: 

````
!# Wireshark Switch Side:

! # Sesión 10: fuente = VLAN remota 888
monitor session 10 source remote vlan 888
! # Destino = tu puerto de captura (conecta aquí el laptop con Wireshark)
monitor session 10 destination interface gi1/0/5

! # Verifica
show monitor session 10
show interface status | include Gi1/0/5
````

- Donde vamos a hacer el espejo del tráfioco, por ejemplo 2 APs, en un mismo switch solo hay que hacer

````
!# AP-1

! # Sesion 11: fuente = Gi 1/0/11 (ambas direcciones)
monitor session 11 source interface Gi 1/0/11 both
! # Destino = VLAN remota 888 (RSPAN)
monitor session 11 destination remote vlan 888

!# AP-2

! # Sesion 12: fuente = Gi 1/0/17 (ambas direcciones)
monitor session 12 source interface Gi 1/0/17 both
! # Destino = VLAN remota 888 (RSPAN)
monitor session 12 destination remote vlan 888

! # Verifica
show monitor session 11
show monitor session 12
show monitor session all


````

Al final apagar las sesiones que usamos: 

````
no monitor session 10
no monitor session 11
no monitor session 12
no monitor session 13
````



















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

1. Configurar RSPAN-VLAN y Trunks
2. Configurar Source (Interfaces Mirrors)
3. Configurar Destination (Interfaz Wireshark)

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

### 2. `Configurar Source` :: _Mirrors / APs a monitorear_

1. Definir la sesión RSPAN en el switch **SOURCE (MIRROR)**  _Donde está el AP o el X aparato a monitorear_

Notas:

- `both` copia RX y TX.
- Puedes usar `tx` o `rx` si quieres solo una dirección.
- Si en vez de un puerto quisieras toda la VLAN 10 (VSPAN), usa: `monitor session 11 source vlan 10`

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
show monitor session 11
show monitor session all

````

Hacer lo mismo con el segundo puerto (o la cantidad de puertos que sea necesario), solo NO REPETIR EL NÚMERO DE SESIÓN, ES SESIÓN POR PUERTO. 

````
enable
configure terminal
!

! # Sesion 12: fuente = Gi 1/0/17 (ambas direcciones)
monitor session 12 source interface Gi 1/0/17 both

! # Destino = VLAN remota 888 (RSPAN)
monitor session 12 destination remote vlan 888

end

! # Verifica
show monitor session 12
show monitor session all

````

Hacer lo mismo con cualquier otro switch que trasnporte la VLAN 888, en nuestro ejemplo con el switch 2, es tal cual la misma configuración: 

````
enable
configure terminal
!

! # Sesion 13: fuente = Gi 1/0/11 (ambas direcciones)
monitor session 13 source interface Gi 1/0/11 both

! # Destino = VLAN remota 888 (RSPAN)
monitor session 13 destination remote vlan 888

end

! # Verifica
show monitor session 13
show monitor session all

````

### 3. `Configurar Destination` :: _Monitor / Wireshark_

4. Todas las sesiones a monitorear van a terminar ena la sesión en el switch destino (SW1) _Donde conectaremos el wireshark para monitorear_

Ahora recibes ese “túnel” en la VLAN 888 y lo sacas por tu puerto de análisis Gi 1/0/5:

````
enable
configure terminal
!

! # Sesión 10: fuente = VLAN remota 888
monitor session 10 source remote vlan 888

! # Destino = tu puerto de captura (conecta aquí el laptop con Wireshark)
monitor session 10 destination interface gi 1/0/5

end

! # Verifica
show monitor session 10
show monitor session all
!
show interface status | include Gi1/0/5


````

## Resultado y Validaciones

Asu

<img width="1062" height="209" alt="image" src="https://github.com/user-attachments/assets/e33ce204-2c5a-4cb2-8522-da7166394270" />

- Importante:

- El puerto destino entra en modo monitor (no envía tráfico de usuario, no puede ser trunk, ni pertenecer a un port-channel, ni tener port-security), no es nada, simplemente un puerto monitor, por eso es importante regresarlo a la normalidad al final a menos que vaya a ser un SPAN dedicado para siempre. 


Validaciones

````
show vlan remote-span
show monitor session all
show interfaces trunk | include 888
````





## Limpieza / quitar SPAN al terminal (Es importante para que el puerto deje de monitorear)

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






## Monitoreo final

Una vez teniendo todo configurado simplemente hay que prender y apagar interfaces a palcer a monitorear, por ejemplo:

- En donde vas a monitorear basciamente todo ese tiempo estará prendido como monitor con: 

````
!# Wireshark Switch Side:

! # Sesión 10: fuente = VLAN remota 888
monitor session 10 source remote vlan 888
! # Destino = tu puerto de captura (conecta aquí el laptop con Wireshark)
monitor session 10 destination interface gi1/0/5

! # Verifica
show monitor session 10
show interface status | include Gi1/0/5
````

- Donde vamos a hacer el espejo del tráfioco, por ejemplo 2 APs, en un mismo switch solo hay que hacer

````
!# AP-1

! # Sesion 11: fuente = Gi 1/0/11 (ambas direcciones)
monitor session 11 source interface Gi 1/0/11 both
! # Destino = VLAN remota 888 (RSPAN)
monitor session 11 destination remote vlan 888

!# AP-2

! # Sesion 12: fuente = Gi 1/0/17 (ambas direcciones)
monitor session 12 source interface Gi 1/0/17 both
! # Destino = VLAN remota 888 (RSPAN)
monitor session 12 destination remote vlan 888

! # Verifica
show monitor session 11
show monitor session 12
show monitor session all


````

Al final apagar las sesiones que usamos: 

````
no monitor session 10
no monitor session 11
no monitor session 12
no monitor session 13
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
