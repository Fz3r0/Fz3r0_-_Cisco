# 🧠🏗️🌐 Cisco Networking: `ACL - Access Control List :: Fz3r0 Bible`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### ✖️ X (Twitter)  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `CCNA` `CCNP` `ACL` `Access Control List` `Standard ACL` `Extended ACL` `access-list` `access-class` `VTY ACL` `Protocol ACL` `L3 Filter` `Packet Filtering` `Permit` `Deny` `Wildcard Mask` `host` `any` `eq` `inbound (in)` `outbound (out)` `Policy-Based Routing (PBR)` `QoS Classification` `Class-map` `Control Plane Policing` `NAT / PAT` `ip nat inside source` `Overload` `Redistribution Filtering` `route filtering` `deny ip any any` `permit ip any any` `Interface ACL` `Access Filtering` `IPv4 ACL` `IPv6 ACL` `Security`

---

<br>

# 📄 `Index`




# 🚦👮🛑 `ACL - Access Control List :: Fz3r0 Bible`

An **Access Control List (ACL)** is a mechanism used in network devices _(mainly routers)_ to make **decisions about traffic** _(e.g. **permit** or **deny** traffic)_ based on specific criteria such as **IP address**, **protocol**, and **port number**. 

- ACLs are the **core tool** in Cisco IOS for **controlling**, **filtering**, and **classifying** traffic.
- ACLs control what traffic is **allowed** or **denied** between two points **based on rules** you define.
- They act like bouncers at a nightclub: they decide who gets in, who gets kicked out, and who gets redirected to the VIP area.

ACLs have existed in the computing world since the early days of **file system permissions** in operating systems (like Unix in the 1970s). But in networking, they became standard tools with the rise of **access-layer routers** and **filtering firewalls** in the 1990s.

- ACLs are vendor-specific features (Cisco, Juniper, etc.) and do not follow any formal RFC or IEEE standard, because they are considered a device-level implementation, not a network protocol. Their syntax, capabilities, and behavior can vary depending on the vendor

## 🧱 `ACL` vs `Firewall`

While both ACLs and firewalls are used to control traffic, there's a key difference:

`Firewall` = **Stateful** :: They track connections, sessions, and application behavior.

- Think of ACLs as basic traffic filters: They look at the packet, match it to a rule, and allow or deny it. 

`ACL` = **Stateless** :: They simply check each packet individually against a rule, without remembering anything about previous traffic.

- Firewalls go further than ACLs, by understanding ongoing sessions (e.g., “this TCP connection is already established, allow return traffic”).

**That’s why ACLs are often used for basic `filtering` or `classification`, while firewalls handle more advanced security tasks, like `detecting attacks`, `traffic shaping`, `very granular blocking rules` or `deep packet inspection behavior`.**

## 🎯 Core Purposes of ACLs

While ACLs can do many things, they serve **two main purposes** across most network environments:

| Purpose       | Role of ACL     | Common Examples                                          |
|---------------|------------------|-----------------------------------------------------------|
| 🔍 **`Filtering`**   | Allow or block   | `VLAN segmentation`, `firewall rules (block/allow traffic between networks/devices/etc)`, `VTY restrictions (ssh/telnet)`      |
| 🗃️ **`Classification`** | Identify/select | `QoS tagging`, `NAT selection`, `route filtering (OSPF, BGP, etc)`, `PBR`         |

## 🔍 ACL Purpose: `Filtering` _(most common use)_

ACLs are most often used to **control access** by:

- **`Permitting`** or **`denying`** traffic between devices or networks (**`inbound`** or **`outbound`** traffic)
- Enforcing segmentation between VLANs or security zones
- Blocking specific protocols or ports (e.g. deny telnet, allow SSH)
- Acting as **stateless firewalls** on interfaces, meaning they evaluate each packet individually, without keeping track of connection states like a stateful firewall would (This means, an ACL doesn't "remember" that a TCP session was established).

> ✅ This is the typical usage in most CCNA/CCNP setups and enterprise networks.

### 📦 Filtering by Packet Content

ACLs make decisions based on the contents of a packet, using criteria from:

- **Layer 3 (Network Layer)** = `IP Addresses`  
- **Layer 4 (Transport Layer)** = `TCP/UDP Ports`  
- Or a **`combination of both`**

This is what makes **`extended ACLs`** so powerful, they can evaluate **source/destination IP** *and* **source/destination port** at the same time.

### 🌐 Layer 3 (IP-based Filtering)

ACLs can match packets based on:

- A single **host IP**  
  → `192.168.10.1 0.0.0.0` or `host 192.168.1.1`
- An entire **subnet**  
  → `192.168.10.0 0.0.0.255`
- A **range of IPs** using wildcard masks  
  → `10.0.0.0 0.255.255.255`
- A **supernet** (summarized networks)  
  → `172.16.0.0 0.15.255.255`
- **The keyword `any` for “match all IPs”**

This type of filtering applies to both **source** and **destination** IP addresses.

### 🎯 Layer 4 (Port-based Filtering)

ACLs can also make decisions based on **TCP/UDP ports** for protocols like HTTP, SSH, Telnet, etc. You can:

- Match **specific ports**
  → `eq 80`, `eq 22`, `eq 443`
- Match **ranges of ports**
  → `range 1000 2000`
- Use **well-known port names**
  → `eq www`, `eq ftp`, `eq telnet`

Ports range from **1 to 65535**, and ACLs can filter **both directions**:
- Source Port (rare)
- Destination Port (most common)

⚠️ **IMPORTANT:**  Port-based filtering is only available in **extended ACLs**.

---

### 📌 Example ACL Entry (L3 + L4)

```PY
!# Step 1 – Create an extended ACL that blocks HTTP from subnet 10.10.10.0/24 to a specific server
ip access-list extended Fz3r0-ACL-HTTP-BLOCK
   deny tcp 10.10.10.0 0.0.0.255 host 192.168.1.100 eq 80
   permit ip any any
exit

!# Step 2 – Apply the ACL to the INBOUND (in) interface where the subnet is located
interface GigabitEthernet0/1
   ip access-group Fz3r0-ACL-HTTP-BLOCK in
exit
````

## 🗃️ ACL Purpose:  `Classification`

ACLs can also be used to **identify and classify** traffic for other processes. For example:

- **`QoS`**: ACLs are used to match VoIP, video, or critical traffic, so the switch or router can give it special treatment (priority queuing, shaping, etc).
- **`NAT`**: ACLs select which IPs will be translated and which won’t.
- **`Route filtering`**: Used in **BGP**, **OSPF**, or **EIGRP** to permit or deny specific networks from being advertised.
- **`Policy-Based Routing (PBR)`**: ACLs match traffic and route it through a specific next-hop instead of default routing.

> 🎯 In this use case, ACLs **do not filter traffic**, they just **mark or select** it for another process.

### 📌 Example ACL for NAT

```PY
!# Step 1 – Define a STANDARD ACL that selects which IPs to NAT
access-list Fz3r0-ACL-1 permit 10.10.0.0 0.0.0.255
access-list Fz3r0-ACL-1 permit 10.30.0.0 0.0.0.255
access-list Fz3r0-ACL-1 permit 10.50.0.0 0.0.0.255

!# Step 2 – Define the NAT rule using that ACL
ip nat inside source list Fz3r0-ACL-1 interface GigabitEthernet0/0 overload

!# Step 3 – Mark interfaces
interface GigabitEthernet0/1
 ip address 10.10.0.1 255.255.255.0
 ip nat inside

interface GigabitEthernet0/0
 ip address 123.1.1.2 255.255.255.252
 ip nat outside
```

## 📋 ACL: `The List (And Why Order Matters)`

ACLs are called *lists* for a reason: they’re literally read **line by line, from top to bottom**, like a checklist. And once a rule matches, **that’s it**, the rest of the list is ignored.

Some access control lists are comprised of multiple statements. The ordering of statements is key to ACL processing. The router starts from the top (first) and cycles through all statements until a matching statement is found. The packet is dropped when no match exists. Order all ACL statements from most specific to least specific. Assigning least specific statements first will sometimes cause a false match to occur. As a result the match on the intended ACL statement never occurs.

-  🧠 **Rule of ACLs:** First match wins. No second chances :: These rules are always evaluated **top-down**, and the **first matching condition** decides the fate of the packet. Nothing below that match is ever checked. For example:

````py
!# 1. 
permit tcp any any eq 80            ← allow HTTP
!# 2.
permit tcp any any eq 25            ← allow SMTP
!# 3.
permit ip host 10.30.0.5 any        ← allow specific IP
!# 4.
deny ip 192.168.1.0 0.0.0.255 any   ← deny access from a subnet
!# 5.
permit tcp host 10.30.0.5 eq 23     ← allow Telnet only for that IP
!# etc ...more rules...

````

### 🔢 ACL: `Order Matters`

Imagine you do something like this:

```bash
access-list 100 permit ip 10.30.0.0 0.0.0.255 any
access-list 100 deny ip host 10.30.0.5 any
````

- Did you just denied `10.30.0.5.`???... even though you already allowed the whole network `10.30.0.0` one line before??? No! because:
    - **That first line already matched the IP `10.30.0.5`!!!, so the second rule never gets evaluated.**
    - This means, If you allow a whole subnet before denying a specific IP from it, the deny will never apply, the packet was already allowed. **That’s why **rule number one of ACLs** is: Always go from specific to general**

✅ **"Always go from specific to general"**: To block a specific IP but allow the rest of the subnet, place the deny for that IP first, followed by a permit for the broader subnet:

```bash
!# 1. Specific: Single Host (block)
access-list 100 deny ip host 10.30.0.5 any
!# 2. General: Whole Subnet (permit)
access-list 100 permit ip 10.30.0.0 0.0.0.255 any
````

### 🚧 ACL: `invisible "deny" last line`

In Cisco IOS, all ACLs end with an **invisible** rule:

````py
deny ip any any
````

This implicit hidden command at the end of the ACLs, by default deny all last statement clause added to the end of any extended ACL. 

- You must include `permit ip any any` as a last statement to all extended ACLs. That effectively permits all packets that do not match any previous clause within an ACL.

Some ACLs are comprised of all deny statements as well, so without the last permit statement, all packets would be dropped. So, if a packet doesn’t match any line of the ACL, it will be dropped, even if it’s valid traffic. 

#### ❌ Example (Bad): 

All other traffic (HTTP, ICMP, DNS, etc.) will be blocked by default:

````py
access-list 110 deny tcp 10.10.0.0 0.0.255.255 any eq 21
````

#### ✅ Example (Correct): 

Always add permit ip any any at the end **unless you want to block everything else on purpose.**

````py
access-list 110 deny tcp 10.10.0.0 0.0.255.255 any eq 21
access-list 110 permit ip any any
````


## 👀 How to Read an ACL Entry _(Left to Right)_

When reading an ACL line, always parse it **from left to right**, like a sentence. Here's an example from our lab with PC1 and PC2:

- Allow TCP traffic from source IP `10.10.0.10` (PC1) to destination IP `10.20.0.20` (PC2) on port `80` (HTTP).

````py
!# Allow TCP traffic from source IP `10.10.0.10` (PC1) to destination IP `10.20.0.20` (PC2) on port `80` (HTTP).
access-list 100 permit tcp host 10.10.0.10 host 10.20.0.20 eq 80
````

**Structure Breakdown:**

- **`[action]` `[protocol]` from `[source IP]` to `[destination IP]` `[optional port match]`**

| Field            | Example Value     | Meaning                              |
|------------------|-------------------|--------------------------------------|
| `access-list 100` | Named or numbered | ACL identifier (100 = extended)      |
| `permit` or `deny` | `permit`         | Action: allow or block the traffic   |
| `tcp` / `udp` / `ip` | `tcp`           | Protocol to match                    |
| `source`         | `host 10.10.0.10` | Who is initiating the traffic (PC1)  |
| `destination`    | `host 10.20.0.20` | Who receives the traffic (PC2)       |
| `port`           | `eq 80`           | Optional: match specific port (HTTP) |

🧭 **Tip:** This format helps you mentally follow the packet flow:  

- **`From` → `To` → `What` → `Action`**

Also remember:

- `tcp` is layer 4 and is used for applications like **HTTP, SSH, Telnet, FTP**
- `udp` is layer 4 and is used for apps like **DNS, SNMP, NTP**
- `ip` matches **any protocol** at Layer 3 (more general)

## ✅ ACL Rules You Should Always Follow

1. **`Write from specific to general`**  
   Put specific matches (like a single IP or host) before general ones (like a whole subnet). If you do it backwards, the specific rule might never get hit.

2. **`Permit first, deny later`**  
   Normally, you start by allowing what’s needed. Deny rules usually come after. But if your goal is to block something specific, deny can go first — just be intentional.

3. **`ACLs are read top to bottom`**  
   The router checks the list line by line. First match wins. Anything below a match is ignored.

4. **`Once a match is found, the rest is skipped`**  
   As soon as a rule matches a packet, the router stops reading the ACL. Later lines don’t matter for that packet.

5. **`There’s an invisible "deny all" at the end`**  
   If no rule matches, the traffic is dropped. The router won’t warn you — it just silently blocks it. Always keep this in mind.

6. **`ACLs must be applied to an interface`**  
   Creating an ACL does nothing unless you apply it to an interface (`in` or `out`), or use it in a feature like NAT, VTY, or route-maps.

7. **`Only one ACL per direction per interface`**  
   You can’t apply two ACLs inbound or two outbound on the same interface. One per direction, that’s it.

8. **`Only one ACL per protocol (IPv4 or IPv6)`**  
   You can’t mix IPv4 and IPv6 ACLs on the same direction of the same interface. Use the right type depending on the traffic.

## Cisco Best Practices

Cisco best practices for creating and applying ACLs:

1. **📍 `Apply extended ACLs close to the source`**  
   Since extended ACLs allow filtering by **source/destination IP, protocol, and port**, placing them near the **source** prevents unnecessary traffic from entering the network in the first place.

2. **🎯 `Apply standard ACLs close to the destination`**  
   Standard ACLs only check the **source IP**, which makes them less precise. Placing them near the **destination** helps avoid accidentally blocking traffic that should’ve been allowed.

3. **📐 `Order ACL entries from most specific to least specific`**  
   Always list **specific rules first** (like individual hosts), followed by broader ones (like subnets or `any`). ACLs are read **top-down**, and the **first match wins**.

4. **🔀 `Only one ACL per direction per Layer 3 protocol per interface`**  
   You can apply **one ACL inbound** and **one ACL outbound** per interface, per Layer 3 protocol (IPv4 or IPv6). You can’t stack multiple ACLs in the same direction.

5. **➕ `Maximum of two ACLs per interface`**  
   Because of the previous rule, you can apply a **maximum of 2 ACLs** per interface:  
   - One **inbound ACL**  
   - One **outbound ACL**  
   And each must be for a single protocol (IPv4 or IPv6).


## Wildcard & ACLs

A **wildcard** is a bitmask that tells the router which parts of an IP address **must match** and which parts **can vary** when evaluating an ACL rule.

- `0` means "this bit **must match exactly**" _(bits **significativos**)_  
- `1` means "this bit can be **anything**" _(bits irrelevantes)_

> ✅ So, wildcards are basically the **reverse** of a subnet mask.

### 🔁 Comparing Mask vs Wildcard:

| Type           | Decimal             | Binary                              |
|----------------|----------------------|--------------------------------------|
| `Subnet Mask`    | 255.255.255.**0**        | 11111111.11111111.11111111.**00000000** |
| `Wildcard Mask`  | **0.0.0**.255            | **00000000.00000000.00000000**.11111111 |

As you can see, the wildcard highlights **which bits are ignored (0's)**, while the subnet mask highlights **which bits are fixed (0's)**.

- That's why ACLs don’t use subnet masks: they don’t need to define networks, they just need to match patterns (using the significant bits (0's))

---

### 📌 Wildcard & ACL example

#### **Whole Network**

```py
!# In a /24 (255.255.255.0) network, you want to make relevant the first portion:
!# 00000000.00000000.00000000.11111111
!# 0.0.0.255
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
```

#### **Single Host**

````py
!# You want to match ONLY the exact IP 192.168.1.10
!# In binary, all bits must match:
!# 00000000.00000000.00000000.00000000
!# So the wildcard is: 0.0.0.0 (no bits are ignored)
access-list 100 permit ip 192.168.1.10 0.0.0.0 any
````

Or you can use simplified version::

````py
access-list 100 permit ip host 192.168.1.10 any
````




## 🔁 Inbound vs Outbound – Where Should You Apply the ACL?

Understanding **inbound** and **outbound** is simple but important when applying ACLs.

When you apply an ACL **to an interface**, you must specify the **direction**:

- `inbound` :: The ACL is applied **as packets enter** the interface = **`in`**
- `outbound` :: The ACL is applied **as packets exit** the interface = **`out`**

### 🔄 Understanding traffic flow (inbound/outbound)

Imagine a router with two PCs on each side:

![image](https://github.com/user-attachments/assets/a9665d77-2d31-4402-af4e-8b0ed2ac3ebb)

- PC1 on network `10.10.0.0/16`, connected to `Gig0/1`
- PC2 on network `10.20.0.0/16`, connected to `Gig0/2`

You want to **block file transfers (FTP/TFTP/SMB)** between these two devices:

#### ✅ Example 1: PC1 initiates FTP connection to PC2

- Packet goes **into Gig0/1 (inbound)**
- Then **out of Gig0/2 (outbound)** toward PC2

So if PC1 is the one initiating traffic → the ACL should be applied **inbound on Gig0/1** :: `Gi0/1` = `in`

#### ✅ Example 2: PC2 initiates FTP connection to PC1

- Packet goes **into Gig0/2 (inbound)**
- Then **out of Gig0/1 (outbound)** toward PC1

If PC2 is initiating traffic → the ACL should be applied **inbound on Gig0/2**  :: `Gi0/2` = `in`

### 🧭 Best practice: Block traffic at the source (inbound)

You **should apply the ACL on the inbound interface**, where the conversation starts. Why?

- Blocking at **inbound** prevents the traffic from even entering the router
- Blocking at **outbound** allows it to enter, consume CPU, and be processed — only to be dropped later
- **Inbound = more efficient and clean**


---

### ⚙️ Example Config

In this example PC1 is initiating the conversation: PC1 -> PC2

```bash
!# Step 1 – Deny FTP (TCP port 21) from any host in 10.10.0.0/16 to anywhere
ip access-list extended BLOCK-FTP
 deny tcp 10.10.0.0 0.0.255.255 any eq 21
 permit ip any any
exit

!# Step 2 – Apply ACL to interface facing source of the traffic = >> INBOUND TRAFFIC (in) <<
interface GigabitEthernet0/1
 ip access-group BLOCK-FTP in
```


## Standard vs Extended ACLs

Cisco ACLs come in two main types **Standard** and **Extended**. Each one serves a different purpose and offers different levels of control:

- **`Standard ACLs`**: Filter traffic **only by source IP address**; _(simple but limited)._ <br>
   :: **Block a subnet (10.10.0.0/16) or host (10.10.0.167)**

````py
!# Step 1 – Create the ACL
access-list 10 deny 10.10.0.0 0.0.255.255
access-list 10 permit any

!# Step 2 – Apply to an interface (inbound or outbound)
interface GigabitEthernet0/1
   ip access-group 10 in
exit
````
  
- **`Extended ACLs`**: Allow filtering **by source/destination IP**; **protocol**, and **port number**. _(Offering precise, application-aware control)._ <br>
   :: **Block HTTP (TCP 80) from a specific subnet (10.10.0.0/16) to a specific server (10.67.0.10)**

````py
!# Step 1 – Create the ACL
ip access-list extended BLOCK-HTTP
   deny tcp 10.10.0.0 0.0.255.255 host 10.67.0.10 eq 80
   permit ip any any
exit

!# Step 2 – Apply to an interface (inbound or outbound)
interface GigabitEthernet0/1
   ip access-group BLOCK-HTTP in
exit
````

| Feature                  | Standard ACL             | Extended ACL                            |
|--------------------------|--------------------------|------------------------------------------|
| Matches                  | Source IP only           | Source IP, Destination IP, Protocol, Port |
| Protocol awareness       | ❌ No                    | ✅ Yes                                   |
| Port filtering           | ❌ No                    | ✅ Yes (HTTP, FTP, etc.)                 |
| Common use               | NAT, VTY lines, Basic traffic filtering           | Traffic filtering, firewalls, QoS        |
| Configuration complexity | Simple                   | More detailed and powerful               |
| ACL Number Range         | 1–99 / 1300–1999 <br> _Or named (string)_         | 100–199 / 2000–2699 <br> _Or named (string)_                    |

### Standard VS Extended

Standard ACLs are an older type and very general. 

- As a result they can inadvertently filter traffic incorrectly.
-  Applying the standard ACL near the destination is recommended to prevents possible over-filtering.

Extended ACLs are more complex and granular

- Extended ACLs are granular (specific) and provide more filtering options.
- They include source address, destination address, protocols and port numbers.
- Extended ACL should be applied closest to the source.
- Applying extended ACLs nearest to the source prevents traffic that should be filtered from traversing the network. That conserves bandwidth and additional processing required at each router hop from source to destination endpoints.



# 🔧 ACL: `Practical Examples` _by Purpose & Scenario_

Here are real-world examples for each ACL use case. These are pure gold for any network engineer’s toolkit:

## ACL Purpose: `Filtering`

### 📋 ACL: `SSH/Telnet :: Access Control`

- ACL Type: `Standard`
- Purpose: `Filtering`
- Use Case: Control device/network management access. This ACL ensures that only devices from the 10.10.0.0/24 subnet can initiate remote CLI sessions (SSH/Telnet) to the router or switch / All others will be **silently** blocked.

````py
!# Step 1 – Create STANDARD ACL to permit only trusted IPs or subnets for remote access
access-list 55 permit 10.10.0.0 0.0.0.255

!# Step 2 – Apply ACL to VTY lines to control SSH/Telnet access. INBOUND
line vty 0 4
   access-class 55 in
exit
````

---

### 📋 ACL: `Block Inter-VLAN Communication`

- ACL Type: `Extended`
- Purpose: `Filtering`
- Use Case: Blocks any IP traffic originating from VLAN 10 (10.10.10.0/24) from reaching VLAN 20 (10.10.20.0/24). All other traffic _(e.g. to other VLANs or the internet)_ will be allowed thanks to the `permit ip any any` at the end. (One on the few cases when `out` outbound interface is used in an ACL)

````py
!# Step 1 – Create an extended ACL to block traffic from VLAN 10 to VLAN 20
ip access-list extended VLAN-BLOCK
   deny ip 10.10.10.0 0.0.0.255 10.10.20.0 0.0.0.255
   permit ip any any
exit

!# Step 2 – Apply the ACL outbound on VLAN 10 interface
interface Vlan10
   ip access-group VLAN-BLOCK out
exit
````

---

### 📋 ACL: `Basic L3 Firewall Filter`

- ACL Type: `Extended`
- Purpose: `Filtering`
- Use Case: It acts like a basic stateless firewall = Only SSH traffic (TCP port 22) from any source is allowed to reach the server 192.168.1.10. / All other traffic is denied and logged, thanks to `deny ip any any log`.

````py
!# Step 1 – Create the ACL to allow only SSH to a specific server, deny everything else
ip access-list extended INBOUND-FILTER
   permit tcp any host 192.168.1.10 eq 22
   deny ip any any log
exit

!# Step 2 – Apply the ACL inbound on the external-facing interface
interface GigabitEthernet0/0
   ip access-group INBOUND-FILTER in
exit

````








## ACL Purpose: `Classification`

### 📋 ACL: `NAT Overload (PAT)` :: _`Classification` / `Standard`_

- ACL Type: `Standard`
- Purpose: `Classification`
- Use Case: Select which IPs or subnets will be translated to the router’s public IP using Port Address Translation (PAT).

````py
!# Step 1 – Create the ACL that defines which internal IPs are allowed to be NATed / using: 10.10.x.x/16 (Mask: 255.255.0.0)
access-list 10 permit 10.10.0.0 0.0.255.255
access-list 10 permit 10.30.0.0 0.0.255.255
access-list 10 permit 10.50.0.0 0.0.255.255

!# Step 2 – Create the NAT rule that applies overload using the ACL
ip nat inside source list 10 interface GigabitEthernet0/0 overload

!# Step 3 – Mark interfaces as NAT inside or outside
interface GigabitEthernet0/1
   description ** LAN (Inside) **
   ip address 10.10.0.1 255.255.0.0
   ip nat inside
exit

interface GigabitEthernet0/0
   description ** WAN (Outside) **
   ip address 123.1.1.2 255.255.255.252
   ip nat outside
exit
````

---

### 📋 ACL: `Policy-Based Routing (PBR)`

- ACL Type: `Extended`
- Purpose: `Classification`
- Use Case: "The bouncer at a nightclub who redirects the packets into the VIP area". This setup forces any traffic from the 10.10.10.0/24 subnet to follow a specific next-hop (192.168.100.1) instead of using the normal routing table. This is useful when you want certain traffic to exit through a different ISP, VPN, or gateway — even if it breaks the regular routing logic.

````py
!# Step 1 – Define an ACL to match specific source traffic
ip access-list extended VIP-TRAFFIC
   permit ip 10.10.10.0 0.0.0.255 any
exit

!# Step 2 – Create a route-map that matches this traffic and sets a custom next-hop
route-map VIP-PATH permit 10
   match ip address VIP-TRAFFIC
   set ip next-hop 192.168.100.1
exit

!# Step 3 – Apply the route-map to the interface (inbound)
!# Note: ip policy route-map is always applied inbound; no need to specify "in", since Policy-Based Routing only affects traffic entering the interface.
interface GigabitEthernet0/1
   ip policy route-map VIP-PATH
exit

````

---

### 📋 ACL: `ACL for QoS (Quality of Service)`

- ACL Type: `Extended`
- Purpose: `Classification`
- Use Case: For prioritizing VoIP traffic. This **class-map** can be used in a QoS policy to give priority to real-time voice traffic, ensuring better call quality.

````py
!# Step 1 – Define an ACL to match VoIP RTP traffic (UDP ports 16384–32767)
ip access-list extended VOICE-TRAFFIC
   permit udp any any range 16384 32767
exit

!# Step 2 – Create a class-map that matches this ACL
class-map match-all VOICE
   match access-group name VOICE-TRAFFIC
exit


````





---


````











# 🗃️ Resources

- https://www.youtube.com/watch?v=ob7rgwt9nuk&list=PLwAU7bA502wHx44LlUptxSG6UQ3ONx3hh
- https://community.cisco.com/t5/networking-knowledge-base/cisco-access-control-lists-acl/ta-p/4182349
  
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
