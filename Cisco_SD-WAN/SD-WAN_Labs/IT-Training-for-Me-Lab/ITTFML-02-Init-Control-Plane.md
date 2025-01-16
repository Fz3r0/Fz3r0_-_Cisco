# 🔥🧱🛡️ Cisco: `SD-WAN: Eve-NG install`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Eve-NG install`

---

<br>

# 📝❓📄 `Index`

- 

# Initalizing Control Plane

## Diagram

![image](https://github.com/user-attachments/assets/6f984862-c1e5-4275-90a4-4e6a9fc001da)

## Credenciales

- admin
- admin

## vManage

### 1. Hacer el Storage

![image](https://github.com/user-attachments/assets/e9a47f49-5e45-4a38-a500-9f6d886da919)

---

### 2. Config Basica:

- System IP Address 50.3.0.2
- System ID 503
- Organization Name "sd-wan-lab"
- vBond = 200.1.1.1

#### I - System IP, Site ID, Organization Name & vBond

![image](https://github.com/user-attachments/assets/b3a92d3f-016c-4deb-ba86-34d4ce4e4467)

````java
configuration mode terminal
system

system-ip 50.3.0.2
site-id 503
organization-name "sd-wan-lab"
vbond 200.1.1.1
exit
````

#### II - VPNs

- VPN0 = vManage
- VPN512 = (no necesaria)

![image](https://github.com/user-attachments/assets/9fe12c70-ae4a-4477-b4d7-e165062b556a)

````java
vpn0
interface eth0
ip address 200.1.1.2/24
no shutdown
exit

commit and-quit
````

#### III - Validar

````java
show running config
````

![image](https://github.com/user-attachments/assets/718e0d19-f8ba-4c79-b3d8-26bf13cd70ad)

![image](https://github.com/user-attachments/assets/282d8d8f-6741-4d27-b1d6-10a92eba8e99)

---

### 3. Probar Conectividad

Ejemplo: 

![image](https://github.com/user-attachments/assets/8ccb2c49-9607-44a1-a9f5-ff3240c9b662)

Abrir CMD desde PC:

![image](https://github.com/user-attachments/assets/4500fab6-1767-4aed-8481-0dfe07363a8f)

Verificar Acceso Web:

![image](https://github.com/user-attachments/assets/6e3a587a-1abb-4671-a5a7-5a2be2f20c89)

![image](https://github.com/user-attachments/assets/3465a553-0a15-41bd-b582-4fa71f80daa2)

---

### 4. Cambiar User y Password de la db

Default:

- neo4j
- password

Nuevo:

- vmanage
- password

````java
request nms configuration-db update-admin-user

neo4j
password

vmanage
cisco123

````

#### Validar:

![image](https://github.com/user-attachments/assets/4914d0b3-266f-4762-9477-ab97c44c8ac9)









# 📚🗂️🎥 Resources

- https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file
- https://www.youtube.com/watch?v=gYWr5pNYkuw
- https://htluo.blogspot.com/2020/05/cisco-sd-wan-with-gns3.html GNS3!!!



  
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

