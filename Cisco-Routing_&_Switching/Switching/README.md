

### WEBINAR: VLANS, TRUNKS, STP y ETHERCHANNEL:

https://www.youtube.com/watch?v=NJkFlRIyGcI

# 🧠 What are **Switches** in Networking?

Switches are the **backbone of modern local networks (LANs)**.  
They connect devices like computers, printers, and servers within a building or campus so they can communicate efficiently.

> Think of a switch like a super-smart mailman 📬 — it knows exactly where to send each message on your network.

---

## 🕰️ A Brief History: From Hubs to Switches

| Era        | Device      | Description |
|------------|-------------|-------------|
| 1980s–90s  | **Hubs** 🪝 | Sent data to *all* ports. Dumb and chatty. |
| Late 90s   | **Bridges** 🌉 | Smarter than hubs, filtered traffic by MAC. |
| 2000s+     | **Switches** ⚡ | Fast, efficient, MAC-learning devices that forward frames only to the right port. |

---

## 🧱 Switch Layers Explained: L1 vs L2 vs L3

| Layer | Name        | Function | Example | Used For |
|-------|-------------|----------|---------|----------|
| L1    | Physical    | Transmits bits (no logic) | Ethernet hub (obsolete) | 🪐 Bare signal relay |
| L2    | Data Link   | Forwards frames based on **MAC addresses** | Most enterprise switches | 🖧 VLANs, segmentation |
| L3    | Network     | Forwards packets based on **IP addresses** | Multilayer switches | 🌐 Inter-VLAN routing, static/dynamic routing |

> 🔎 L2 Switch = “smart power strip”  
> 🧠 L3 Switch = “router built into a switch”

---

## 🔌 Modern Switch Features (2020s and beyond)

✅ PoE (Power over Ethernet)  
✅ 802.1X NAC integration  
✅ VLAN tagging / trunking  
✅ Stacking (single control plane for multiple switches)  
✅ NetFlow / SNMP / Telemetry  
✅ SDN Support (Software-defined networking)

> 🧪 Some enterprise switches even support containerized services (like Docker)!

---

## 🔄 Cisco vs Other Vendors: Key Differences

| Feature                  | Cisco 🇺🇸               | Aruba 🇳🇱              | Juniper 🇺🇸             | TP-Link / Ubiquiti 🌍         |
|--------------------------|-------------------------|------------------------|-------------------------|-------------------------------|
| CLI / OS                 | IOS / NX-OS / IOS-XE    | AOS-CX / ArubaOS       | Junos OS                | Web GUI + CLI                 |
| Licensing                | Feature-based licenses 💸 | Included in hardware   | Subscription or fixed   | Mostly license-free           |
| Automation Support       | Cisco DNA, Netconf/YANG  | REST APIs, Ansible     | SaltStack, PyEZ         | Limited / Basic REST          |
| Focus                    | Enterprise / Data Center | Campus / Wireless-first| Telcos / Service Core   | Small/medium business         |
| Price                    | $$$                     | $$                     | $$$                     | $                             |

> 💡 Cisco = robust and complex  
> 💡 Aruba = simple and wireless-integrated  
> 💡 Juniper = strong in ISPs & automation  
> 💡 Ubiquiti = budget-friendly but limited

---

## 🛠️ Real-World Example

You're setting up a new office:

- Use **L2 switches** on each floor for device access
- Use an **L3 core switch** for routing between VLANs
- Deploy **PoE switches** for IP phones and access points
- Centralize with **Cisco DNA Center** or **Aruba Central**

---

## 🎓 TL;DR

| Concept      | Meaning |
|--------------|---------|
| Switch       | Connects devices in a network (smart forwarding) |
| L2 Switch    | Works with MAC addresses (VLANs, segmentation) |
| L3 Switch    | Routes IP packets (inter-VLAN, static/dynamic routes) |
| Vendors      | Cisco = enterprise, Aruba = wireless/campus, Juniper = ISPs, Ubiquiti = SMB |

---

## 🧩 Pro Tip

🧠 Want to become a switch ninja?

- Master **VLANs**, **STP**, and **LACP**
- Practice **configuring SVI interfaces** for L3
- Use **Wireshark** to sniff switch port traffic
- Learn **zero-touch provisioning** for modern networks




### Images

![image](https://github.com/user-attachments/assets/c42dc899-f6a6-4319-a34e-d6acc4ab172d)
