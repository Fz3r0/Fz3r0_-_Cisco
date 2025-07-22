

## 📟 Cisco IOS Prompt Modes

- Cisco IOS has various operational modes, each with a unique command-line prompt and purpose.
- Understanding these modes is essential for navigating, configuring, and troubleshooting Cisco devices.
- The prompt helps you identify what you can do at any given moment.

| Prompt                  | 🧭 Mode Name                     | 📝 Description                                                            |
| ----------------------- | -------------------------------- | ------------------------------------------------------------------------- |
| `>`                     | **User EXEC Mode**               | Basic access – allows limited commands like `ping`, `show`, `traceroute`. |
| `#`                     | **Privileged EXEC Mode**         | Full access – enter with `enable`; lets you view and troubleshoot.        |
| `(config)#`             | **Global Configuration Mode**    | Allows system-wide configuration changes with `configure terminal`.       |
| `(config-if)#`          | **Interface Configuration Mode** | Used to configure specific interfaces (e.g., `GigabitEthernet0/1`).       |
| `(config-line)#`        | **Line Configuration Mode**      | Used to configure terminal lines (e.g., VTY, console, aux).               |
| `(config-router)#`      | **Routing Protocol Mode**        | Used to configure routing protocols like OSPF, BGP, EIGRP, etc.           |
| `(config-vlan)#`        | **VLAN Configuration Mode**      | Used to configure VLANs on switches.                                      |
| `(config-access-list)#` | **Access List Config Mode**      | Configure standard or extended ACLs.                                      |
| `(vlan)#`               | **VLAN Database Mode**           | Legacy mode accessed with `vlan database` (mostly on older IOS).          |
| `rommon #>`             | **ROM Monitor Mode (ROMmon)**    | Low-level recovery mode used when the device cannot boot IOS.             |
| `switch:`               | **Boot Loader Mode**             | Limited mode used during corrupted image recovery (e.g., `boot tftp:`).   |
| `loader>`               | **Loader Mode**                  | Similar to boot loader; appears in certain older platforms.               |
| `setup>`                | **Initial Configuration Dialog** | Interactive setup wizard shown on first boot or after config deletion.    |

##  🔣 Cisco IOS: `Special Characters & Syntax`

- Cisco IOS uses a variety of special characters in both command syntax and output filtering.
- Understanding these characters helps when reading help menus, crafting complex commands, or parsing long outputs.
- Characters like `?`, `|`, `{}`, and `[]` have specific meanings and are used throughout CLI interaction.

| Character / Syntax | 🔧 Name / Function              | 🧠 Description                                                         | 💡 Example Usage                               |  
| ------------------ | ------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------- | 
| `?`                | **Help Prompt**                 | Displays available commands or options in the current context.         | `show ?`                                       |    
| `^`                | **Control Key Notation**        | Represents Ctrl key combinations (e.g., `^C` = Ctrl+C).                | `^Z` to end configuration mode.                |    
| `!`                | **Comment Marker**              | Marks configuration comments or separates sections.                    | `! This is a comment`                          |    
| `\|`               | **Pipe for Filtering**          | Sends command output to filters like `include`, `begin`, or `exclude`. | `show running-config \| include ip route`      |    
| `/`                | **Regex Separator**             | Used in pattern matching or as part of regular expressions.            | `show log \| include /UPDOWN/`                 |    
| `>`                | **User EXEC Prompt / Redirect** | Indicates User EXEC mode or used rarely in file redirection.           | `Router>`                                      |    
| `#`                | **Privileged EXEC Prompt**      | Indicates privileged EXEC mode or config submode.                      | `Router(config)#`                              |      
| `()`               | **Mode Context Indicators**     | Shows current config context (e.g., interface, line, router).          | `Router(config-if)#`, `Router(config-router)#` |     
| `[]`               | **Optional Argument**           | Marks optional input parameters in help messages.                      | `ping [ip-address]`                            |     
| `{}`               | **Required Choice Set**         | Indicates a mandatory selection between options.                       | `transport input {all\| none \| ssh \| telnet}` |
| `TAB`              | **Autocompletion**              | Auto-completes commands or arguments.                                  | Typing `conf` + `TAB` completes to `configure` | 
| `SPACE`            | **Command Separator**           | Separates elements of a command line.                                  | `interface GigabitEthernet0/1`                 | 


## ⌨️ Cisco IOS: Control Key Combinations

- Cisco IOS supports several keyboard shortcuts using the `Ctrl` key (^) to make command-line navigation and editing more efficient.
- These combinations help you interrupt processes, end configuration modes, repeat commands, and more.

****

| ⌨️ Key Combo | 🧠 Description                                                       | 💡 Common Usage Example                                      |
| --------- | --------------------------------------------------------------------- | --------------------------------------------------------- |
| `^`       | **"Ctrl" Key Notation**                                               | Represents `Ctrl` key combinations (e.g., `^C` = Ctrl+C). |
| `^Z`      | End configuration mode and return to privileged EXEC (`exit` + `end`) | Finish configuration and return to `#` prompt.            |
| `^C`      | Cancel command or break current process                               | Interrupt a `ping`, `traceroute`, or long-running output. |
| `^D`      | Log out or exit input (EOF)                                           | Ends input stream (rare, used with manual `more`).        |
| `^A`      | Move cursor to beginning of the line                                  | Quick navigation when editing long commands.              |
| `^E`      | Move cursor to end of the line                                        | Jump to the end of a typed command.                       |
| `^U`      | Erase entire line                                                     | Clears everything typed on the line.                      |
| `^W`      | Erase the last word                                                   | Deletes last word without clearing the full line.         |
| `^P`      | Recall previous command (same as ↑)                                   | Scrolls up through command history.                       |
| `^N`      | Recall next command (same as ↓)                                       | Scrolls down through command history.                     |
| `^R`      | Redisplay the current line                                            | Redraws command line (if output overlaps).                |
| `^L`      | Refresh the screen (terminal dependent)                               | Clears/refreshes screen – mostly in some emulators.       |


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
