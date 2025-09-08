# 🔥🧱🛡️ Cisco: `SD-WAN: Introduction & Concept`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` `Console Cable` `Serial`

---





# 🛰️ Configure SPAN and RSPAN

Scenario: Mirror the traffic of **three different AP** ports to a **single Wireshark laptop** using one **RSPAN VLAN 888**.

- Two APs live on Switch-1.
- One AP lives on Switch-2.
- All mirrored traffic arrives at Switch-1, port Gi1/0/5, where your laptop is.

Think of VLAN 888 as the “pipe” that carries mirrored packets from source ports to your analyzer port.

````py
AP1 (SW1 Gi1/0/11) ┐
AP2 (SW1 Gi1/0/17) ├─> RSPAN VLAN 888 ── trunks ──> SW1 Session 10 ──> Gi1/0/5 -> Wireshark
AP3 (SW2 Gi1/0/17) ┘
````

### 🧩 Session plan you will configure

1. Switch-1 :: Session 10 — Destination - Wireshark / Capturing Laptop
2. Switch-1 :: Session 11 — Source - AP-1 (SW1 - Gi 1/0/11)
3. Switch-1 :: Session 12 — Source - AP-2 (SW1 - Gi 1/0/17)
4. Switch-2 :: Session 13 — Source - AP-3 (SW2 - Gi 1/0/17)

## 🛠️ `RSPAN Configuration` 

You just need to follow 3 easy steps: 

1. Create the RSPAN VLAN and allow it on trunks (both switches)
2. Create the source sessions (the ports you want to mirror)
3. Create the destination session (where Wireshark is)

---

### ⭕ 1) `Configure the RSPAN VLAN and trunks`

- Create the dedicated remote-span VLAN on both switches (VLAN 888):

```py
enable
configure terminal
!
vlan 888
   name RSPAN-888
   remote-span
end

!
```

- Allow the VLAN 888 across all relevant 802.1Q trunks that interconnect the path between Switch-2 and Switch-1 (or any other switches).
- **IMPORTANT**: Use `ADD` so you don’t wipe the existing allowed VLAN list!!!

```py
enable
configure terminal
!
interface GigabitEthernet 1/0/23
   switchport trunk allowed vlan ADD 888
end

!
```

Validate:

```py
show interfaces trunk | include 888
show vlan remote-span
```

---

### ⭕ 2) `Configure the Source sessions` (mirror the AP ports)

- On the switch where the source interface physically resides (e.g. Access Points), define a monitor session with the access port as the source and the RSPAN VLAN as the destination.

Notes:

- `both` mirrors ingress and egress (RX and TX). You may use `rx` or `tx` if you only need one direction.
- To mirror an entire VLAN instead of a single port (VSPAN), use `monitor session <id> source vlan <vlan-id>`.

#### 📡 `AP-1 on Switch-1` :: Session 11

```py
enable
configure terminal
!
! # Session 11: source = Gi 1/0/11 (both directions)
monitor session 11 source interface Gi 1/0/11 both
! # Destination = remote RSPAN VLAN 888
monitor session 11 destination remote vlan 888
end
```
```py
! Verify
show monitor session 11
show monitor session all
```

#### 📡 `AP-2 on Switch-1` :: Session 12

```py
enable
configure terminal
!
! # Session 12: source = Gi 1/0/17 (both directions)
monitor session 12 source interface Gi 1/0/17 both
! # Destination = remote RSPAN VLAN 888
monitor session 12 destination remote vlan 888
end
```
```py
! Verify
show monitor session 12
show monitor session all
```

#### 📡 `AP-3 on Switch-2` — Session 13 

```py
enable
configure terminal
!
! # Session 13: source = Gi 1/0/11 (both directions)
monitor session 13 source interface Gi 1/0/11 both
! # Destination = remote RSPAN VLAN 888
monitor session 13 destination remote vlan 888
end
```
```py
! Verify
show monitor session 13
show monitor session all
```

> Keep the session number unique per source. If your 4th or anu other AP is added on Switch-2, use any other session like Session 14. 

---

### ⭕ 3) Configure the Destination session (Wireshark analyzer on Switch-1)

Terminate the RSPAN VLAN at Switch-1 and forward it out your analyzer port.

#### 🦈 Wireshark / Laptop

```py
enable
configure terminal
!
! # VSession 10: source = remote RSPAN VLAN 888
monitor session 10 source remote vlan 888
! # VDestination = analyzer interface (plug your Wireshark laptop here)
monitor session 10 destination interface gi 1/0/5
end
```
```py
! # Verify
show monitor session 10
show monitor session all
show interface status | include Gi1/0/5
```


## ✅ Expected Result and Validations

All mirrored traffic from:

- Switch-1 Gi 1/0/11
- Switch-1 Gi 1/0/17
- Switch-2 Gi 1/0/11 

...is delivered out of Switch-1 Gi 1/0/5 to your Wireshark host.

Useful checks:

```py
show vlan remote-span
show monitor session all
show interfaces trunk | include 888
```

### ⚠️ Important behavior of the destination port

- The destination port becomes a monitor (analyzer) port while the session is active. It does not forward user traffic, cannot be a trunk, cannot be part of an EtherChannel, and ignores features like voice VLAN or port-security. It simply emits the mirrored frames toward your analyzer.
- Once you remove the SPAN/RSPAN session, the destination port automatically returns to its configured switchport state as defined in the running-config. The source port always keeps operating normally while being mirrored.

## 🧹 Cleanup: remove SPAN when done

Do this on any switch where a session is configured.

```py
enable
configure terminal
!
no monitor session 10
end
```

If you created multiple sessions, remove each explicitly:

```py
no monitor session 10
no monitor session 11
no monitor session 12
no monitor session 13
```

## 🧩 Platform Notes (good to know)

* Session limits vary by platform and software. Some Catalyst families only support a small number of concurrent SPAN/RSPAN sessions. If a command is rejected, you may have hit a platform limit.
* Interface naming can be flexible in the CLI parser, but on many systems it is typically written without a space (Gi1/0/5, GigabitEthernet1/0/23). Your current syntax may still work; be consistent with what your device accepts.
* Ensure VLAN 888 is allowed across every trunk hop between the source and destination switches, otherwise the RSPAN frames will not reach the destination session.

---

## 🧪 Troubleshooting Tips

* If you see no packets on Wireshark:

  * Verify `show vlan remote-span` on both ends.
  * Confirm `show interfaces trunk | include 888` along the full path.
  * Make sure the analyzer interface is actually the destination in Session 10 and is up/up.
  * Check that your source sessions are set to `both` (or the correct direction) and that the correct interfaces are referenced.
* If a command is not recognized:

  * You might be on a platform or software release with slightly different SPAN syntax or feature limits.
 





 

# 📋 RSPAN: Quick Operations Playbook

#### 🦈 Analyzer side (Switch-1):

```py
! # Wireshark switch side
monitor session 10 source remote vlan 888
monitor session 10 destination interface gi1/0/5
```
```
show monitor session 10
show interface status | include Gi1/0/5
```

#### 📡 AP mirrors (example on Switch-1):

```
! # AP-1
monitor session 11 source interface Gi 1/0/11 both
monitor session 11 destination remote vlan 888

! # AP-2
monitor session 12 source interface Gi 1/0/17 both
monitor session 12 destination remote vlan 888
```
```
show monitor session 11
show monitor session 12
show monitor session all
```

Shutdown when finished:

```
no monitor session 10
no monitor session 11
no monitor session 12
no monitor session 13
```

---






# 📚🗂️🎥 Resources

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
