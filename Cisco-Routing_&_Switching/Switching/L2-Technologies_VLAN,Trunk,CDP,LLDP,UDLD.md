
## 🟦 VLAN (Virtual LAN)

Una VLAN es como dividir un switch físico en varios switches virtuales.

📦 Un solo switch
➡️ Muchas redes lógicas separadas

Sirve para:

* Seguridad 🔐
* Organización 🗂️
* Menos broadcast 💥
* Mejor rendimiento ⚡

Ejemplo:

| VLAN | Nombre  | Uso típico              |
| ---- | ------- | ----------------------- |
| 10   | USERS   | PCs de usuarios         |
| 20   | VOICE   | Teléfonos IP            |
| 30   | SERVERS | Servidores              |
| 99   | MGMT    | Administración switches |

Un puerto normal es:

```
Access Port → pertenece a UNA sola VLAN
```

---

## 🔗 TRUNK

Un trunk es un enlace que transporta MUCHAS VLANs al mismo tiempo entre switches, routers, firewalls, etc.

Imagina un cable como una autopista:

* Cada VLAN es un carril 🚗🚙🚕

Usa etiquetado 802.1Q:

```
[Frame Ethernet] + [Tag VLAN]
```

Ejemplo:

| Tipo de puerto | VLANs permitidas |
| -------------- | ---------------- |
| Access         | 1 sola VLAN      |
| Trunk          | Muchas VLANs     |

Comando típico:

```bash
switchport mode trunk
switchport trunk allowed vlan 10,20,30,99
```

---

## 📢 CDP (Cisco Discovery Protocol)

Protocolo propietario de Cisco.
Sirve para que los equipos Cisco se “presenten” entre ellos.

Dice cosas como:

* Quién soy 🧍
* Qué IP tengo 🌐
* Qué puerto uso 🔌
* Qué modelo soy 🧰

Funciona SOLO entre equipos Cisco.

Ejemplo:

| Campo      | Ejemplo    |
| ---------- | ---------- |
| Device ID  | SW-CORE-01 |
| IP Address | 10.1.1.1   |
| Port ID    | Gi1/0/24   |
| Platform   | C9300-48P  |

Verlo:

```bash
show cdp neighbors
show cdp neighbors detail
```

Emoji mental:
🗣️ “Hola, soy un switch Cisco y estoy conectado aquí.”

---

## 🌍 LLDP (Link Layer Discovery Protocol)

Es lo mismo que CDP, pero estándar (no propietario).

Funciona con:

* Cisco
* HP
* Aruba
* Juniper
* Mikrotik
* etc.

Es el idioma universal de los switches.

Comparación:

| Protocolo | Fabricante |
| --------- | ---------- |
| CDP       | Solo Cisco |
| LLDP      | Todos      |

Comandos:

```bash
show lldp neighbors
show lldp neighbors detail
```

Emoji mental:
🌎 “Hola, hablo el idioma estándar de redes.”

---

## 🔁 UDLD (Unidirectional Link Detection)

Este es de nivel más bajo y más crítico.

Sirve para detectar enlaces rotos de forma peligrosa:

Cuando:

* El cable transmite solo en una dirección ❌
* Pero el switch cree que está bien 😵
* Se crean loops, blackholes, STP roto, etc.

UDLD salva vidas de red 🛟

Modos:

| Modo       | Comportamiento     |
| ---------- | ------------------ |
| Normal     | Solo avisa ⚠️      |
| Aggressive | Apaga el puerto 🔥 |

Ejemplo real:
Fibra dañada donde TX funciona pero RX no.

Comando:

```bash
udld aggressive
```

Emoji mental:
👀 “No me basta con ver link up, quiero confirmar que ambos lados hablan.”

---

## 🧩 Todo junto en una tabla final

| Concepto | Para qué sirve                          | Nivel |
| -------- | --------------------------------------- | ----- |
| VLAN     | Separar redes lógicas                   | L2    |
| Trunk    | Transportar múltiples VLANs             | L2    |
| CDP      | Descubrir vecinos Cisco                 | L2    |
| LLDP     | Descubrir vecinos estándar              | L2    |
| UDLD     | Detectar enlaces rotos unidireccionales | L1/L2 |

---

## 🧠 Visual mental completo

```
[PC] -- VLAN 10 --\
                  \
                [SW1]==== Trunk ==== [SW2] ---- VLAN 20 ---- [Phone]
                   |||
                 CDP/LLDP
                   |||
                Descubrimiento

                 UDLD
         Protección de enlaces rotos
```

---

## 💥 Resumen

* VLAN = separación lógica 🧱
* Trunk = autopista de VLANs 🛣️
* CDP = presentación Cisco 🤝
* LLDP = presentación universal 🌐
* UDLD = detector de cables traicioneros 🕵️


