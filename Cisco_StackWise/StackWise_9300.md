# 🔥🧱🛡️ Cisco: `StackWise`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `StackWise` `Cisco` `Catalyst`

---

<br>

# 📝❓📄 `Index`

- 

# **Cisco StackWise: Technical Overview**

## **What is Cisco StackWise?**

Cisco **StackWise** is a switch stacking technology that allows multiple physical switches to operate as a single logical unit. It is primarily used in Cisco **Catalyst** series switches, providing high availability, simplified management, and increased scalability for enterprise networks.

- **Switch stacking is to connect two or more stackable switches, usually up to nine, through a stack cable and uses Cisco propriety StackWise protocol.**
- **Stacked switches can be regarded as one unit, and use the same IP address.**
- **The stack master which contains the saved and running configuration files for the switch stack controls the whole stack units and can be replaced if it fails.**
- **Stacking connects the backplane of the switches thus having full backplane speed connectivity between the switches.**

![image](https://github.com/user-attachments/assets/4f0716fe-9b0c-4aaa-b057-91c9815c1ed2)


![image](https://github.com/user-attachments/assets/00920e6c-0246-4a73-834d-d17f95e9e854)

![image](https://github.com/user-attachments/assets/93650678-7cd5-4e82-a249-23de4bcbf3fb)



## Cisco StackWise: **Key Features**

| **Feature**               | **Description** |
|---------------------------|----------------|
| **Single Control Plane**  | All switches in the stack function as a single entity, with one switch acting as the **Master** and others as **Members**. |
| **High Availability**     | If the Master switch fails, another switch in the stack takes over without interrupting network operations. |
| **High-Speed Backplane**  | Switches are connected using Cisco’s proprietary stacking cables, with speeds up to **480 Gbps (StackWise-480)**. |
| **Unified Configuration** | Managed as a single switch with a single IP address, simplifying network administration. |
| **Scalability**           | Allows adding or removing switches without network downtime. |
| **Redundancy Support**    | Load is distributed across multiple switches, preventing single points of failure. |


## Cisco StackWise: **Supported Switch Models and Technologies**

| **Brand & Model**             | **Stacking Technology**           | **Backplane Speed** | **Year** | **Compatible IOS**            |
|-------------------------------|-----------------------------------|---------------------|----------|------------------------------|
| **Cisco Catalyst 2960-X**      | FlexStack-Plus                    | 40 Gbps             | 2013     | IOS LAN Base, LAN Lite        |
| **Cisco Catalyst 2960-XR**     | FlexStack-Plus, FlexStack-Extended | 40 Gbps             | 2015     | IOS IP Lite, IP Base          |
| **Cisco Catalyst 3650**        | StackWise-160                     | 160 Gbps            | 2014     | IOS XE                        |
| **Cisco Catalyst 3850**        | StackWise-480                     | 480 Gbps            | 2014     | IOS XE                        |
| **Cisco Catalyst 9300**        | StackWise-480, StackWise Virtual  | 480 Gbps            | 2017     | IOS XE                        |
| **Cisco Catalyst 9400**        | StackWise Virtual                 | N/A                 | 2017     | IOS XE                        |
| **Cisco Catalyst 9500**        | StackWise Virtual                 | N/A                 | 2017     | IOS XE                        |


> **Note:**
> - **StackWise** requires proprietary stacking cables for physical stacking.
> - **StackWise Virtual** enables logical stacking via fiber links without dedicated stacking cables.

- _`IOS (Internetwork Operating System)` is the operating system developed by Cisco for its networking devices, such as switches and routers. There are different versions of IOS, each designed to meet specific needs. IOS LAN Base is a basic version, ideal for small to medium-sized networks, with limited features. IOS IP Lite is also a lighter version, but with specific capabilities for routing IP._
- _`IOS XE` is the most recent and advanced version, based on Linux, offering greater modularity, flexibility, and support for new technologies, making it ideal for more complex and large-scale environments._

## **Stacking Technologies: Differences & Capabilities**

| **Technology**         | **Brand & Model**                | **Year**  | **Connection Type**        | **Max Bandwidth** | **Stacking Method**              | **Status**         | **Real-Life Example**                                                                 |
|------------------------|----------------------------------|----------|----------------------------|------------------|----------------------------------|--------------------|--------------------------------------------------------------------------------------|
| **FlexStack-Plus**     | Cisco Catalyst 2960-X, 2960-XR  | 2013     | Dedicated stacking cables   | 40 Gbps          | Physical Stacking               | In Use             | Used in enterprise networks for small-to-medium scale setups, like office campuses. |
| **FlexStack-Extended** | Cisco Catalyst 2960-XR           | 2015     | Stacking over fiber         | 40 Gbps          | Physical Stacking (Extended Range) | In Use             | Deployed in larger campuses or multi-building installations for extended reach.      |
| **StackWise-160**      | Cisco Catalyst 3650              | 2014     | Proprietary stacking cables | 160 Gbps         | Physical Stacking               | In Use             | Common in medium to large enterprises, data centers requiring higher bandwidth.      |
| **StackWise-480**      | Cisco Catalyst 3850              | 2014     | Proprietary stacking cables | 480 Gbps         | Physical Stacking               | In Use             | Used in high-performance enterprise networks and campus environments.                |
| **StackWise Virtual**  | Cisco Catalyst 9300, 9400        | 2017     | Fiber (No stacking cables)  | N/A              | Logical Stacking                | In Use             | Deployed in large-scale data centers and enterprises requiring flexibility.          |


## **StackWise vs. Switches Connected via Trunk**

Switching stacking and trunking are two different concepts, even though both of them can simplify network management. Switch stacking vs trunking, choosing which one depends on users requirements. If high density port and high speed are the first parameter to consider, stacking and stackable switch are a better choice. If not, then trunking can be taken into consideration.

| **Comparison Factor**    | **Cisco StackWise** | **Switches Connected via Trunk** |
|-------------------------|---------------------|---------------------------------|
| **Management**          | Managed as a single switch with one IP. | Each switch requires individual configuration. |
| **Redundancy**          | Automatic failover; if Master fails, another switch takes over. | Requires Spanning Tree Protocol (STP) or dynamic routing for redundancy. |
| **Performance**         | Up to **480 Gbps** dedicated stacking bandwidth. | Limited by trunk bandwidth (**1G, 10G, 40G, or 100G**). |
| **Failover Speed**      | Fast failover with no reconvergence delay. | STP may introduce delays during reconvergence. |
| **Configuration Complexity** | Simple, single-device configuration. | Requires VLAN configuration, inter-switch routing, and STP tuning. |
| **Cost**                | Higher due to proprietary stacking hardware. | More cost-effective using standard Ethernet or fiber links. |
| **Scalability**         | Easily scalable with plug-and-play stacking. | Adding switches requires manual trunk and VLAN configuration. |

![image](https://github.com/user-attachments/assets/9a31a9b4-3ec9-47b2-a83c-9b918415b7f5)

![image](https://github.com/user-attachments/assets/3cd4814a-7c5d-43f4-be7f-fb9a222515b6)


## **Pros and Cons of StackWise**

### ✅ **Advantages**

✔ **Simplified Management**: Managed as a single entity with one IP address.
✔ **High Performance**: Dedicated stacking bandwidth up to 480 Gbps.
✔ **Failover Redundancy**: Automatic Master switch reassignment in case of failure.
✔ **Scalability**: Add/remove switches dynamically without downtime.
✔ **Unified Configuration**: No need for STP tuning or VLAN trunking between stacked switches.

### ❌ **Disadvantages**

❌ **Higher Cost**: Requires proprietary Cisco stacking hardware and cables.
❌ **Hardware Dependency**: Only works with compatible Cisco Catalyst models.
❌ **Physical Limitations**: Stacked switches must be in close proximity due to stacking cable length constraints.


## **Use Cases**

### ✅ **When to Use StackWise** 

- Large enterprise networks requiring **high availability and redundancy**.
- Environments where **centralized management** and **scalability** are critical.
- Data centers or network closets with multiple **Cisco Catalyst switches**.

### ❌ **When to Use Trunk Links Instead**

- When connecting **switches from different vendors**.
- If switches are **geographically dispersed** and cannot be stacked physically.
- When looking for a **more cost-effective** solution using standard Ethernet links.




# Cisco StackWise: `Configuration`

This guide will walk you through the process of configuring two Cisco Catalyst 3750 switches with StackWise technology. We will outline the materials required, the step-by-step configuration, and troubleshooting commands.

## Materials Needed

- Cisco Catalyst 3750 Series Switches (2 switches minimum)
- Stacking Cables (Cisco proprietary cables for StackWise, make sure you have enough for the desired stack configuration)
- Power cables for each switch
- Ethernet cables for initial connectivity
- Console cables for configuration via serial port
- Terminal emulation software (e.g., PuTTY, Tera Term)
- Access to Cisco IOS on both switches

## Physical Setup

### 2 Switches: 

- Connect the StackWise cables to the StackWise ports on both Cisco 3750 switches in "X" pattern. _These ports are typically located at the back of the switch._
- Power on both switches.
- Ensure both switches are powered and stacked through the StackWise cables.
- Connect your console cable to one of the switches for configuration.

![image](https://github.com/user-attachments/assets/39de40f0-36d0-414a-9144-4938bba5aa01)

### 3 or more Switches

If you have 3 switches or more, you will need to connect the switches in a daisy-chain fashion.

- Switch 1 to Switch 2: Connect Switch 1’s Port 1 to Switch 2’s Port 2.
- Switch 2 to Switch 3: Connect Switch 2’s Port 1 to Switch 3’s Port 2.

If you have more than 3 switches, continue chaining them using Port 1 of the higher-numbered switch to Port 2 of the next switch.

![image](https://github.com/user-attachments/assets/2adfb895-9ee3-4b7e-9b31-8fb0a7d1810e)


## Enter CLI via Console Port

1. Connect the console cable to the console port of the top switch, which we will call `Core 1`:

![image](https://github.com/user-attachments/assets/3e9450a8-7c87-4d56-a044-f63f633b4d3f)

2. Once inside the CLI, first check the version with the show version command. Here you can see:

````
show version
````

- The OS version: <br> <br> ![image](https://github.com/user-attachments/assets/658e8c26-58ea-4b73-add4-25c7831bf732) <br>
- And the status of the switches: <br> <br> ![image](https://github.com/user-attachments/assets/d3f130f6-f939-4064-8353-648a254c2158) <br>

3. Check the stats with the show switch command to verify the StackWise status:

````
show switch
````

- **You should see both switches listed, with one designated as `Active` and the other as `Member` **

![image](https://github.com/user-attachments/assets/5384a919-01f7-48b0-9a87-2969faec94d2)

The order of `Active` and `Member` can be changed with just a simple configuration:. 

## Configuration

To configure the `Priority` value between `1` and `15` (**higher values make the switch to become the master**). In my case, I want the Active switch to be the top one, for which I just need to configure the StackWise.: 

There are also other configurable values like the switch number and role: 

![image](https://github.com/user-attachments/assets/6ab13005-ec3d-4848-88ea-64cb82bee772)

- ⚠️ **`IMPORTANT`: Every configuration change in Switch Number, Role or Priority need the devices to `Reboot/Reload` to take new changes.**

### 1. Change Priority of `Switch 1` (Top Switch)

Add the next command (priotioty 15 = highest priority):

````
switch 1 priority 15
````

### 2. Change Priority of `Switch 2` (Bottom Switch)

Add the next command (priotioty 10 = lower priority):

- NOTE: If more switches are added to the stack, you can configure lower values such as 9, 8, 7, 6... for the other switches. If only one switch has the highest priority and the others have the same priority, then if the active switch fails, the new active switch may be chosen randomly using other factors.

````
switch 2 priority 10
````

### 3. Save Configuration & Reload (Reboot)

Every configuration change in Switch Number, Role or Priority need the devices to `Reboot/Reload` to take new changes. But first we need to save configuration to running config. 

1. Save Config:

````
write memory
````

2. Reload:

````
reload
````




# StackWise: Useful Commands

````java
## Validation

show switch
show interface description
show version

## Configuration


````



# 📚🗂️🎥 Resources

- https://www.youtube.com/watch?v=oBGQ8bCjA9w
- https://www.youtube.com/watch?v=OXQSVJwwHjo
- https://www.fiberopticshare.com/switch-stacking-vs-trunking-whats-difference.html



  
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



