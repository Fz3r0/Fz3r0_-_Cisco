# 🔥🧱🛡️ Cisco: `SD-WAN: Considerations and Implementation Scenarios`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `(BFD)`

---

<br>

# 📝❓📄 `Index`

- 

# Considerations and Implementation Scenarios

Puntos a considerar cuando migraremos nuestros enlaces para implementar SD-WAN

Exsten 3 metodos diferentes para implementar SDWAN:

1. Entreprise IT (Private) :: Tener en sitio todos los elementos que componen el control plane (y managament)
2. Manage Service Provider (MSP) :: Contratamos servicios de SD-WAN a un proveedor de sevricio que ya tiene implementado todo (como un ISP), como cliente solo se hará uin deploy de vEdge
3. Cisco Cloud :: Cisco nos vende TODO: Administración, Implementación, Producto, etc. 

![image](https://github.com/user-attachments/assets/979189ac-55d1-4fb3-98f2-12dc81a2c4e2)

## Remote: vEdge remote site designs

1 licencia por cada enlace (MPLS, WAN, 4g, etc), en el ejhemplo del diagrama sería algo así:

- Mesh:  4 licencias = 4 enlaces (2 cada vEdge)
- Mesh switch: (ahorra licencias)
- TLOC extension: 2 salidas pero unicamente 1 licencia por cada vEdge grciuas a un link entre 2 vEdges conectados por el SW L2
- TLOC extension over L3: similar al anterior, pero con un link L3

![image](https://github.com/user-attachments/assets/e90903bc-5ab5-4297-aab0-ceb5b755f39c)


# 📚🗂️🎥 Resources

- https://youtu.be/JM5LD8doDPg?si=9L-REEqY6wuAlKbL



  
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

