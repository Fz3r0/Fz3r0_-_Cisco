

## 📟 Cisco IOS Prompt Modes

| Prompt                  | Mode Name                           | Description                                                               |
| ----------------------- | ----------------------------------- | ------------------------------------------------------------------------- |
| `>`                     | 🧑‍💻 **User EXEC Mode**            | Basic access – allows limited commands like `ping`, `show`, `traceroute`. |
| `#`                     | 🔐 **Privileged EXEC Mode**         | Full access – enter with `enable`; lets you view and troubleshoot.        |
| `(config)#`             | ⚙️ **Global Configuration Mode**    | Allows system-wide configuration changes with `configure terminal`.       |
| `(config-if)#`          | 🌐 **Interface Configuration Mode** | Used to configure specific interfaces (e.g., `GigabitEthernet0/1`).       |
| `(config-line)#`        | 📞 **Line Configuration Mode**      | Used to configure terminal lines (e.g., VTY, console, aux).               |
| `(config-router)#`      | 🛣️ **Routing Protocol Mode**       | Used to configure routing protocols like OSPF, BGP, EIGRP, etc.           |
| `(config-vlan)#`        | 🧱 **VLAN Configuration Mode**      | Used to configure VLANs on switches.                                      |
| `(config-access-list)#` | 🚫 **Access List Config Mode**      | Configure standard or extended ACLs.                                      |
| `(vlan)#`               | 🗂️ **VLAN Database Mode**          | Legacy mode accessed with `vlan database` (mostly on older IOS).          |


## 📟 `show running-config`

- The `show running-config` command is one of the most frequently used tools on Cisco IOS devices. 
- It displays the current active configuration stored in RAM, reflecting any changes made since the last save.
- With the help of filters like `section`, `include`, `begin`, or `exclude`, you can quickly narrow down specific parts of the configuration, making it easier to troubleshoot or audit settings.

| Command Example                                                                                          | Description                                                                 |
|----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| `show running-config`                                                                                    | 🛠️ Displays the full running configuration currently in RAM.               |
| `show running-config \| section interface`                                                               | 🌐 Shows each interface's configuration block.                             |
| `show running-config \| include hostname`                                                                | 🏷️ Filters lines that contain 'hostname'.                                  |
| `show running-config \| begin line vty`                                                                  | 🔐 Shows config starting at 'line vty' section.                            |
| `show running-config \| exclude !`                                                                       | 🧼 Omits comment lines (`!`) from output.                                  |
| `show archive config differences nvram:startup-config system:running-config`                             | 🧾 Compares startup-config vs running-config.                              |
| `show running-config interface GigabitEthernet1/0/1`                                                     | 🎯 Shows config for a specific interface.                                  |
| `show running-config \| include vlan`                                                                    | 🪢 Filters all lines with the word 'vlan'.                                 |
| `show running-config \| section router ospf`                                                             | 🗺️ Displays the OSPF routing process section.                              |
| `show running-config \| begin line con`                                                                  | 🖥️ Starts output from 'line con' section.                                  |
| `show running-config \| include ip route`                                                                | 🧭 Shows static route entries only.                                        |
| `show running-config \| section line`                                                                     | 🧵 Shows all `line` sections (console, aux, vty).                          |
| `show running-config \| section aaa`                                                                      | 🔐 Displays AAA authentication configuration.                              |
| `show running-config \| section snmp`                                                                     | 📡 Shows SNMP-related configuration.                                       |
| `show running-config \| section crypto`                                                                   | 🔐 Shows crypto config (RSA keys, trustpoints, PKI).                       |
| `show running-config \| include username`                                                                 | 👤 Displays local user accounts.                                           |
| `show running-config \| include access-list`                                                              | 🚧 Filters and shows ACL entries.                                          |
| `show running-config \| section ip dhcp`                                                                  | 📦 Displays DHCP pool and configuration.                                   |
| `show running-config \| section ntp`                                                                      | ⏰ Filters only NTP-related config.                                        |
| `show running-config \| include logging`                                                                  | 📝 Displays all logging commands (buffered, trap, host, etc).             |
| `show running-config \| include service`                                                                  | 🧩 Shows service-level config lines (e.g., `service timestamps`).         |



## 🔐 Cisco IOS: `Password Encryption Types`

- Cisco IOS supports multiple password encryption formats, each with varying levels of security and compatibility.  
- You can specify the encryption type using a number (e.g., `0`, `5`, `7`, `8`, or `9`) in commands like `username admin password`, `username admin secret`, `enable secret`, `line vty password`, `snmp-server user`, `energywise shared-secret`, `ppp chap password`, and `key-string` for routing protocol authentication.  
- It’s important to understand which types are secure (e.g., SHA-256, SCRYPT) and which are weak or obsolete (e.g., Type 7 or plaintext).

| Type   | 🔒 Encryption Method    | 🧠 Description                         | 📜 Example in `show running-config`                          |
|--------|--------------------------|----------------------------------------|--------------------------------------------------------------|
| `0`    | **Plaintext**            | No encryption (visible as-is)          | `username admin password 0 mypassword123` <br>or `username admin password mypassword123` |
| `7`    | **Cisco Type 7**         | Weak reversible encoding               | `username admin password 7 104D000A0618`                     |
| `5`    | **MD5 Hash**             | Legacy one-way hash                    | `username admin secret 5 $1$abc$klsdjfoaisjdlfkjlkajsf/`     |
| `8`    | **SHA-256 (PBKDF2)**     | Strong hash, modern IOS                | `username admin secret 8 $8$XUSPOuRbfydzjcWx...`             |
| `9`    | **SCRYPT Hash**          | Stronger than SHA-256                  | `username admin secret 9 $9$wJrQaTlg1HvMwkw...`              |



## `show interfaces`

- The `show interfaces` command family is essential for monitoring and troubleshooting physical and logical interfaces on Cisco devices.  
- It provides detailed Layer 1 and Layer 2 information such as link status, duplex, speed, MAC address, counters, and error statistics.  
- Combined with related commands like `show ip interface`, `show interfaces status`, or `show interfaces trunk`, it offers deep visibility into interface behavior across routing and switching platforms (e.g., Catalyst, Nexus, ISR).  
- These commands are especially useful when verifying connectivity, diagnosing link issues, checking switchport roles, or auditing interface configurations on live devices.

| Command Example                                       | Description                                                                 |
|-------------------------------------------------------|-----------------------------------------------------------------------------|
| `show interface`                                     | 🛠️ Layer 1 & 2 details: status, errors, speed, MAC, CRCs, collisions.       |
| `show interface status`                              | 📊 Summary of all interfaces: VLAN, duplex, speed, status.                 |
| `show interface description`                         | 🏷️ Interface names + descriptions + status (up/down/admin down).          |
| `show ip interface`                                   | 🌐 Layer 3 behavior: ACLs, helper address, redirects, proxy ARP, etc.       |
| `show ip interface brief`                             | 📋 Quick summary: interface name, IP address, and line/protocol status.     |
| `show ipv6 interface`                                 | 🌱 IPv6 config and status for all interfaces.                              |
| `show interface GigabitEthernet1/0/1`                | ⚡ Detailed status and counters for GE port 1/0/1.                          |
| `show ip interface GigabitEthernet1/0/1`              | 🧠 IP-related config and status for a specific GE interface.               |
| `show interface FastEthernet0/1`                     | 🚗 L1/L2 info on a FastEthernet port (older gear).                         |
| `show interface Ethernet1/1`                         | 🚀 Common in Nexus — standard Ethernet port info (1G/10G/25G/etc).         |
| `show interface trunk`                               | 🔄 Lists all interfaces acting as trunks + allowed VLANs.                  |
| `show interface port-channel10`                      | 🔗 Port-Channel status (bundle status, members, load-balance, etc).        |
| `show ip interface port-channel10`                    | 🔒 L3 behavior and IP details of a Port-Channel.                           |
| `show interface Vlan100`                             | 🌐 SVI (VLAN interface) operational status and L2/L3 details.              |
| `show ip interface Vlan100`                           | 🌐 L3 behavior (ACLs, helper, redirects) for VLAN interface.               |
| `show interface Loopback0`                           | 🎯 Logical loopback interface, usually used as router ID or mgmt.          |
| `show interface Tunnel0`                             | 🛣️ Tunnel interface (GRE, IPsec, etc.) info.                              |
| `show interface management0`                         | 🧑‍💻 Management-only port used for out-of-band access.                      |
| `show interface nve1`                                | 🌐 NVE interface (VXLAN) info, used in EVPN/Nexus fabrics.                |
| `show interface switchport`                          | 🎚️ Shows switchport mode, access/trunk, native VLAN, negotiation.         |
| `show interface capabilities`                        | ⚙️ Displays supported features: speed, POE, trunking, etc.                 |
| `show interface counters errors`                     | ❌ Shows per-interface errors: CRC, input/output drops, overruns, etc.     |
| `show interface status err-disabled`                 | 🚫 Lists all interfaces currently in err-disabled state.                   |
| `show controllers`                                    | 🛠️ Hardware-level details (PHY, cable type, SFP info, useful for errors).  |
| `show interface \| include input rate`               | 📥 Filters only lines with input traffic rate (bps/pps).                   |
| `show interface \| include output rate`              | 📤 Filters only lines with output traffic rate (bps/pps).                  |


## show vlan and show interfaces trunk

| Command Example                                 | Description                                                                 |
|-------------------------------------------------|-----------------------------------------------------------------------------|
| `show vlan`                                     | 📋 Displays the full VLAN database: IDs, names, and assigned interfaces.    |
| `show vlan brief`                               | 🧾 Summarized view of VLANs and their assigned access ports.                |
| `show vlan id 10`                               | 🔍 Shows detailed info for a specific VLAN ID (name, state, ports).         |
| `show vlan name BLUE`                           | 🔍 Shows info for a specific VLAN by name.                                  |
| `show vlan internal usage`                      | 🧠 Displays VLANs automatically used by system features (like MST or VTP).  |
| `show interfaces trunk`                         | 🔄 Lists trunk interfaces, allowed VLANs, native VLAN, and status.          |
| `show interfaces trunk | include 10`            | 🔎 Filters trunk info for interfaces passing VLAN 10.                       |
| `show interfaces GigabitEthernet1/0/1 trunk`    | 📡 Trunking status for a specific interface (mode, native VLAN, allowed).   |
| `show interfaces switchport`                    | 🎚️ Shows access/trunk mode, VLAN assignment, and negotiation per interface.|
| `show interfaces switchport | include Access`         | 🧩 Filters switchport output to show access mode assignments.               |
| `show interfaces status`                        | 📊 VLAN assignment, port state, and speed for all interfaces.               |
| `show spanning-tree vlan 10`                    | 🌲 Shows STP role/state for VLAN 10 per port (useful with trunks).          |



## Security SSH & VTY Lines

<img width="512" height="640" alt="image" src="https://github.com/user-attachments/assets/cc0bc66b-09a9-4a92-8503-f6ed02b55d5b" />
