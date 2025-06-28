

## 📟 Cisco IOS Prompt Modes Cheat Sheet

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
