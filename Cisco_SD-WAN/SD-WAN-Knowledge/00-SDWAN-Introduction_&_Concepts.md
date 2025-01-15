# 🔥🧱🛡️ Cisco: `SD-WAN: Introduction & Concept`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `SD-WAN`

---

<br>

# Traditional Network VS SD-WAN

- Traditional networks rely on static connections, manual management, and fixed routes, often leading to latency, congestion, and high costs. 
- As businesses moved towards cloud computing, SD-WAN emerged to address these limitations by offering dynamic, scalable, and cloud-friendly solutions.
- Introduced to enhance flexibility, security, and performance, SD-WAN began gaining traction in the 2010s, providing better traffic management, optimized performance, and lower operational costs.

The following table compares both architectures, highlighting why SD-WAN is increasingly preferred in modern networking:

| **Aspect**                         | **Traditional Network**                                       | **SD-WAN**                                                  |
|-------------------------------------|---------------------------------------------------------------|------------------------------------------------------------|
| **Concept**                         | Conventional networks relying on traditional routing and QoS methods. | Wide Area Network defined by software with cloud orchestration. |
| **Connection and Link Types**       | Relies on fixed links and physical connections (MPLS, Internet, etc.).  | Supports multiple connection types such as MPLS, Internet, LTE, etc. |
| **Latency**                         | May experience latency due to congestion and bandwidth limitations. | Optimizes latency through dynamic path selection. |
| **Congestion**                       | Prone to congestion when too many users share the same link. | Manages congestion with dynamic path selection and QoS policies. |
| **QoS (Quality of Service)**        | QoS implemented manually, prioritizing critical traffic like voice and video. | Supports QoS to prioritize traffic and allocate bandwidth for critical applications. |
| **Traffic Management**              | Requires manual intervention to ensure traffic paths and performance. | Performs dynamic path selection for more efficient traffic management. |
| **Route Planning**                  | Static planning and manual control of traffic routes. | Dynamic route selection with policy-based control and cloud orchestration. |
| **Scalability**                     | Limited scalability due to infrastructure costs and complexity. | High scalability through cloud-based services and centralized orchestration. |
| **MPLS-TE (Multiprotocol Label Switching Traffic Engineering)** | Used in large networks for traffic management, but requires high cost and advanced configuration. | Does not require MPLS-TE; uses orchestration and policies for dynamic path management. |
| **Infrastructure and Costs**        | Higher cost due to physical infrastructure and network management complexity. | Cost reduction using cloud services and virtualized infrastructure. |
| **Security**                        | Requires manual security solutions and VPNs, such as firewalls. | Supports integrated VPNs, enhanced security, and third-party services like firewalls. |
| **Ease of Use**                     | Requires manual management and in-depth knowledge of infrastructure. | Simple interface, easy to use with defined policies and cloud orchestration. |
| **Orchestration and Control**       | Centralized control, but with limited flexibility and dynamism. | Cloud-based orchestration that adapts dynamically to network changes. |
| **VPN Implementation**              | Manual VPN implementation with static configurations. | Supports VPNs with policy control and traffic orchestration. |
| **Mission-Critical Application Management** | Mission-critical applications may be affected by congestion without precise planning. | Ensures mission-critical applications are delivered with specific routing and QoS policies. |
| **Cloud Adaptability**              | Limited ability to adapt to cloud and virtualized services. | Designed to fully integrate with the cloud, offering efficient cloud service management. |
| **Multi-Network Traffic Management** | Requires manual management of multiple links and routes for critical and user traffic. | Dynamically and optimally manages traffic across multiple links and networks. |

### SD-WAN Gartner Magic Cuadrant

![image](https://github.com/user-attachments/assets/b84350f0-56f8-4f3f-bf38-498732f7f26c)


## Cisco iWAN (legacy)

Cisco's previous SD-WAN-like solution, iWAN, provided traffic control, security, QoS, WAN optimization, and VPN tunneling without the high costs of MPLS VPNs. However, iWAN had challenges in deployment and management, especially in complex or existing environments. As internet services improved and MPLS became less attractive due to lower costs and better SLAs, Cisco transitioned to SD-WAN to offer a more user-friendly solution with enhanced flexibility, scalability, and integration with cloud services.

Cisco's iWAN, which used technologies like DMVPN, IPSec, and PfRv3 for dynamic routing and application optimization, was complex and costly to deploy in mature networks. Despite its successes, the product was eventually overshadowed by SD-WAN solutions, like Viptela, which promised simplified management and better integration with cloud-based infrastructures.

## Viptela (legacy)

Viptela, founded in 2012 by former Cisco directors Amir Khan and Khalid Raza, rapidly emerged as a leading provider of SD-WAN solutions. With financial backing from Sequoia Capital, Viptela gained rapid traction in the network industry, garnering praise and attracting customers like Verizon, Singtel, and The Gap. Its SD-WAN solution offered virtualization of the WAN and carrier-agnostic capabilities, which allowed organizations to securely use any transport medium, such as broadband or 4G/LTE.

One of Viptela's main advantages was its ability to replace expensive MPLS lines with cheaper public internet, while still ensuring encrypted traffic. By 2017, Viptela boasted over 16,000 branch office deployments, offering 50% lower costs, 10x more bandwidth, and 5x better cloud performance. Its simple interface and integration with cloud services like AWS, Azure, and Office 365 helped it stand out from competitors like Cisco’s iWAN, ultimately leading to Cisco's acquisition of Viptela for $610 million in 2017.

## iWAN VS Viptela VD Cisco SD-WAN

| **Technology**   | **Year Introduced** | **Current Status** | **Legacy or Active** | **Key Features**                                      |
|------------------|---------------------|--------------------|----------------------|------------------------------------------------------|
| **Cisco SD-WAN** | 2017                | Active             | Active               | Integration with Cisco DNA, cloud-first architecture, simplified management, automation, security, and analytics. |
| **Viptela**      | 2012                | Acquired by Cisco  | Legacy (now part of Cisco SD-WAN) | WAN virtualization, carrier agnostic, cost-effective compared to MPLS, cloud service integration (AWS, Azure, Office 365), simple interface. |
| **iWAN**         | 2013                | Retired            | Legacy               | Integrated into Cisco branch routers, used DMVPN, IPSec, and PfRv3 for WAN optimization, security, and QoS, but difficult to deploy and manage compared to SD-WAN. |

# Cisco SD-WAN


## Cisco SD WAN

![image](https://github.com/user-attachments/assets/d80aaaaa-f66e-48b4-9ddc-0372346e14f3)

![image](https://github.com/user-attachments/assets/74a9b988-d881-456a-9f95-c8a5bd049890)

![image](https://github.com/user-attachments/assets/3269ac75-30d1-41a3-ac25-0319206d2a42)

![image](https://github.com/user-attachments/assets/9c38633d-a7e2-4548-807d-d15e00a5d86e)

![image](https://github.com/user-attachments/assets/9bc58514-9ea3-45a0-a048-76a2b709965f)

![image](https://github.com/user-attachments/assets/085b472d-4f93-43cd-b203-96d0e27bb64d)

## vManage Dashboard Overview

- https://www.youtube.com/watch?v=KqZcDJYQGvg

# 📚🗂️🎥 Resources

- [Learning SD-WAN with Cisco Transform Your Existing WAN Into a Cost-effective Network](https://drive.google.com/file/d/1SIYXYgIZ3txEDh4OTS9ZNzX_i4jbbPBX/view)

  
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







