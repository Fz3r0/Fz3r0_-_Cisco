# 🔥🧱🛡️ Cisco: `SD-WAN: Introduction & Concept`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `Console Cable` `Serial`

---

# Connecting a Cisco Router or Switch to a PC via Serial Cable

## What is a Serial Cable and Why is it Used?

A serial cable is a communication cable used to establish a direct connection between networking equipment (routers and switches) and a computer. Unlike a standard RJ45 Ethernet cable, a serial cable allows direct access to a device's console for management and configuration.

### History and Composition

Serial communication was pioneered by IBM and widely adopted in computing and networking in the 1960s. The RS-232 standard, developed in 1960 by the Electronic Industries Association (EIA), became the foundation for serial communication. These cables are composed of multiple conductors, typically twisted pairs, enclosed in a protective shield to reduce electromagnetic interference (EMI). Older serial connectors used DB25 and DB9 connectors, while modern implementations have transitioned to RJ45 and USB interfaces.

![image](https://github.com/user-attachments/assets/0f11c505-e4ea-4fc5-a198-b561820eeed4)

### Why Not Use a Regular RJ45 Cable?

- A standard RJ45 cable is used for network communication, not direct console access.
- A serial connection provides out-of-band management, meaning you can configure the device even if networking services are down.
- Serial cables use RS-232 communication, while RJ45 cables use Ethernet protocols.

## Common Serial Connectors and Cable Types

| Connector Type / Cable Type       | Description                                                                                               | Common Devices                   | Year Introduced |
| --------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------- | --------------- |
| **DB9 (DE-9)**                    | 9-pin D-subminiature connector used in legacy PCs.                                                        | Older PCs, Cisco 1800, 2800      | Early 2000s     |
| **RJ45 to DB9 (Legacy Serial)**   | Used with older PCs that have a DB9 serial port. Requires an RJ45-to-DB9 adapter.                         | Cisco 1800, 2800, 3800           | Early 2000s     |
| **USB to Serial (DB9/RJ45)**      | Required for newer PCs without serial ports. Needs a USB-to-serial adapter or a direct USB-to-RJ45 cable. | Cisco 2900, 3900, Catalyst 3750X | 2010s           |
| **Micro-USB to USB**              | Newer Cisco devices have a built-in USB console port, allowing direct connection.                         | Cisco ISR 4000, Catalyst 9000    | 2015-Present    |

![image](https://github.com/user-attachments/assets/e0a421a1-849a-44ea-8393-f3db4ebeee84)

## How to Connect Each Type

### Legacy (RJ45 to DB9)

1. Connect the **RJ45** end to the console port of the router/switch.
2. Attach the **DB9** end to the PC’s serial port.
3. Use a **USB-to-serial adapter** if the PC lacks a DB9 port.

### USB to Serial Adapter

1. Plug the **USB-to-serial adapter** into the PC.
2. Connect an **RJ45-to-DB9 cable** between the adapter and the router/switch.
3. Install the necessary USB-to-serial drivers on the PC.

### Micro-USB to USB

1. Use a **USB-to-Micro-USB cable** to connect the PC to the router/switch.
2. Install Cisco USB console drivers if necessary.
3. Open a terminal emulator to access the console.

## Windows Configuration for Serial Console Access

- **IMPORTANT**: Physical connection must be established first (connection betweeen PC & Switch/Router), then follow these steps to configure the serial connection on Windows:

### Step 1: Identify the COM Port

1. Connect the PC to the router/switch using the appropriate serial cable for your PC/Router/Switch.
2. Open **Device Manager** (`Win + X` → Device Manager).
3. Expand **"Ports (COM & LPT)"**.
4. Locate the active COM ports (e.g., `COM3`, `COM4`, `COM5`).
   - If you see a **yellow warning icon (!)**, install the required driver.

<img width="793" height="511" alt="image" src="https://github.com/user-attachments/assets/5381d846-b224-4d4e-adaf-e8fbc5aa68c1" />

### Step 2: Connect via PuTTY or SecureCRT

1. Open **PuTTY** or **SecureCRT** (or another terminal emulator like Tera Term).
2. Select **"Serial"** as the connection type.
3. In the **"Serial line"** field, enter the COM port (e.g., `COM5`).
4. Set the **Speed (Baud Rate)**:
   - Cisco default: `9600`
   - Raritan Server (example): `115200`
5. Click **Open** to start the session.
6. Log in with **admin credentials**, and you're ready to configure the device!

<img width="531" height="491" alt="image" src="https://github.com/user-attachments/assets/6e56aeb1-f161-402a-9add-9dc851d0a0de" />or<img width="455" height="446" alt="image" src="https://github.com/user-attachments/assets/e7a86159-ca51-4ac9-ad0f-3e0f44a0ed28" />



<img width="536" height="487" alt="image" src="https://github.com/user-attachments/assets/8bce3046-da47-4da9-928a-841b5230226e" />


## Troubleshooting Console Connection:

If the console is unresponsive:

- Ensure you selected the correct COM port.
- Try different baud rates (`9600`, `115200`, `38400`).
- Restart PuTTY or the router/switch.




# 📚🗂️🎥 Resources

- https://www.youtube.com/shorts/PJLOZIAeZR8

  
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
