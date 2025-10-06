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

- 

#  CMD, Powershell, Bash :: `Fz3r0 Troubleshooting Bible`






## Ping

### Windows CMD

````py
# Simple ping
ping google.com

# Continuous ping
ping -t 8.8.8.8

# Ping a specific number of times
ping -n 20 google.com

# Continuous ping with log output (Ctrl + C to stop)
ping -t google.com >> C:\Temp\ping_log.txt

# Continuous ping with timestamp (CMD trick using for loop)
for /f "tokens=*" %a in ('ping -t 8.8.8.8') do @echo %time% %a

````

### Windows Powershell

````py
# Simple ping 
ping google.com
ping 8.8.8.8



# Add date to ping command
ping -t google.com | ForEach-Object {"$(Get-Date -Format 'HH:mm:ss') $_"}

# Export log to file
ping -t google.com | ForEach-Object {"$(Get-Date -Format 'HH:mm:ss') $_"} >> C:\Temp\ping_shepherd.log

````


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

