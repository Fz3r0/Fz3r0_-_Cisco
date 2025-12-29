# 🦈 Cisco Catalyst Packet Capture (EPC) + SFTP Transfer

This tutorial explains how to capture traffic directly on a **Cisco Catalyst switch** using **Embedded Packet Capture (EPC)**, export the capture to local flash, and securely transfer the `.pcap` file to an **SFTP server** for analysis in **Wireshark**.

This method is ideal when:

- You cannot **SPAN** traffic to an external sniffer
- You need traffic from an **exact switch interface**
- You want a **precise, low-impact** capture

## 🧩 **Supported Cisco Catalyst Platforms**

Embedded Packet Capture (EPC) is supported on many **Catalyst IOS-XE** switches, including:

- **Catalyst 3650 / 3850**
- **Catalyst 9300 / 9300L / 9300X**
- **Catalyst 9400**
- **Catalyst 9500**

**Notes:**

- Support depends on **IOS-XE version**
- Buffer limits vary by model
- Always verify **flash capacity** before capturing

## 🔍 **Pre-checks**

### 📂 **Check flash capacity**

- `dir bootflash:`

### 🧠 **Verify existing captures**

- `show monitor capture`

## 🟢 **Step 0 – Identify Host IP on the Interface**

Identify the **MAC address** learned on the switch port, then resolve it to an **IP address**.

`show mac address-table interface Gi 1/0/1`  
`show ip arp | include f0f0.f0f0.f0f0`

Save the result as:

`<IP_HOST>` (e.g. 10.10.10.1)

## 🟡 **Step 1 – Create a Filtered ACL for the Capture**

### 👀 **Review existing ACLs**

`show access-lists`

### 🛠️ **Create a dedicated capture ACL**

This ensures only traffic **to and from the target host** is captured (like a Wireshark IP filter):

````py
configure terminal  

ip access-list extended PCAP-ACL  
   permit ip host 10.10.10.1 any  
   permit ip any host 10.10.10.1  
end
````

## 🔵 **Step 2 – Define the Packet Capture**

The capture uses a 10 MB **circular buffer** and a **60-second time limit**.
- The circular buffer does **not** stop the capture when it becomes full. Instead, once the 10 MB limit is reached, older packets are continuously overwritten by newer ones.
- The capture will automatically stop **only** when the 60-second duration is reached (or if stopped manually). As a result, the exported file contains the **most recent traffic** captured just before the capture stopped.

````py
! # Use PCAP-ACL created for filter
monitor capture PCAP_Fz3r0-1 access-list PCAP-ACL
! # Select interface to capture ingress/egress traffic
monitor capture PCAP_Fz3r0-1 interface GigabitEthernet1/0/1 both 
! # Limit capture size (10mb)
monitor capture PCAP_Fz3r0-1 buffer circular size 10
! # Limit capture duration (60 secs)
monitor capture PCAP_Fz3r0-1 limit duration 60
````

## ▶️ **Step 3 – Start and Stop the Capture**

### ▶️ **Start capture**

Note: There is no need to manually stop the capture if there's a time or buffer limit. 

- `monitor capture PCAP_Fz3r0-1 start`

Wait **30 to 60 seconds** while the issue occurs.

### ⏹️ **Stop capture**

- `monitor capture PCAP_Fz3r0-1 stop`

## 🧪 **Step 3.1 – Capture Verification**

`show monitor capture PCAP_Fz3r0-1`

Use this to verify **packet count**, **size**, and **status**.

## 💾 **Step 4 – Export Capture to Flash**

`monitor capture PCAP_Fz3r0-1 export location bootflash:/PCAP_Fz3r0-1.pcap`

Verify file creation:

`dir bootflash: | include PCAP_Fz3r0-1`

## 🧹 **Step 5 – Cleanup (Recommended)**

### ❌ **Remove capture instance**

````py
no monitor capture PCAP_Fz3r0-1
show monitor capture
````

### 🗑️ **Remove ACL**

````py
configure terminal
   no ip access-list extended PCAP-ACL
end
````

## 🌐 **Phase 2 – Transfer Capture to SFTP Server (Secure / SSH)**

### 📤 **Step 1 – Copy file from the switch**

- `copy bootflash:/PCAP_Fz3r0-1.pcap sftp:`

You will be prompted for:

Address or name of remote host []? 66.66.66.66 / fz3r0.sftp-server.net

Destination username []? Fz3r0-admin

Destination filename [PCAP_Fz3r0-1.pcap]? (enter)

Password []? p4ssw0rd

If the command finishes **without errors**, the transfer is successful.

### 🗑️ **Step 2 – Delete Local File (Optional)**

Once confirmed on the SFTP server:

- `delete bootflash:/PCAP_Fz3r0-1.pcap`

---

### 🖥️ **Step 3 – Access File via FileZilla**

- **Protocol:** `SFTP`  
- **Port:** `22`  
- **Host:** `fz3r0.sftp-server.net`  
- **Username:** `Fz3r0-admin`  
- **Password:** `p4ssw0rd`

Open the `.pcap` file in **Wireshark** and have fun.
