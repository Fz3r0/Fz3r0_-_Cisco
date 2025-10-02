# 🧠🏗️🌐 Cisco IOS: `Fz3r0 Troubleshooting Bible`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` 

---

<br>

# 📄 `Index`

### Device Location

- [🔎 Locate Device in LAN (by IP or MAC)]()

#  Cisco IOS :: `Fz3r0 Troubleshooting Bible`




# 🔎 Locate Device in LAN (by IP or MAC)

To locate a device in the LAN, just follow these 4 steps:

### ⭕ 1. Ping the Device 

- Ping the Device If you have the IP, if you only have the MAC, skip to step 3

````py  
ping 10.10.0.100
````

### ⭕ 2. Check ARP Table 

- This will show the MAC and the VLAN

````py 
show ip arp 10.10.0.100
````

### ⭕ 3. Search by MAC

- Preferably search from the core switch or the closest switch from the device you are searching

````py 
show mac address-table address f0f0.f0f0.f0f0
````

### ⭕ 4. Trace the Path

- 🎯 If step 3 shows you an **access port** e.g. `Gi 1/0/12`, that’s the device location!!!
- 🕵️‍♂️ If it shows a **trunk or port-channel**, go to that switch and repeat step 3 until you reach the exact switch and access port 




# 🗃️ Resources

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

