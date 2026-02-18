# 📦 Cisco Catalyst – File Transfer Methods (SFTP, USB, SCP)

This write-up documents different methods to transfer files to and from a **Cisco Catalyst IOS-XE switch**, including:

* 📤 Switch → SFTP
* 📥 SFTP → Switch
* 🔌 USB drive → Switch
* 🔌 Switch → USB drive
* 🔁 Switch → Switch (SCP)

This is especially useful for:

* Offline firmware staging
* Backups
* File transfers (.pcap, suppor logs, etc)
* Lab environments
* Air-gapped upgrades

Lab IP examples used:

- 🔁 **Switch-A**: `10.10.10.10`
- 🔁 **Switch-B**: `10.10.20.20`
- 📥 **SFTP Server**: `10.10.10.100`

Firmware example used:

- `cat9k_iosxe.17.12.05.SPA.bin`

## 🧠 Pre-Checks (Always!!!)

Always run the next pre-checks to avoid "suprises" <.<

### 🔎 Identify Storage Devices

To see how the switch detects internal and external storage devices:

```bash
show file systems
```

You will see something like:

```
File Systems:

Size(b)      Free(b)       Type    Flags    Prefixes
11353194496  5091016704    disk    rw       flash:
             -             opaque  ro       system:
             -             opaque  rw       usbflash0:
```

Look for:

* `flash:` → Internal storage
* `usbflash0:` → USB device
* `usbflash1:` → Secondary USB (if present)

After identifying the correct prefix:

```bash
dir usbflash0:
```

### 📂 Check Current Flash Content and Available Space

Check existing files:

```bash
dir flash:
```

Check Available Space, (at the bottom of "dir flash:" command, look at:)

```
xxxxx bytes total (yyyyy bytes free)
```

Make sure you have **at least 1.5 GB free** for a Catalyst 9K image.

## 🌐 `Method 1` :: Transfer File From Switch to SFTP

To Transfer File **From Switch to SFTP**, follow next steps:

### 📤 Copy File to SFTP

```bash
copy flash:cat9k_iosxe.17.12.05.SPA.bin sftp:
```

You will be prompted for:

```
Address or name of remote host? 10.10.10.100

Destination username? fz3r0-admin

Destination filename? (press Enter)

Password? p4ssw0rd.12345

```

### ✅ Verify Transfer Success

1. Switch Side - Check that command finishes without errors, transfer is successful.:

```
[OK - xxxxx bytes]
```

2. SFTP side: Verify File in the SFTP server

Use SFTP client (Filezilla, WinSCP, etc) to check for the file in the server (e.g. sftp server - 10.10.10.100)


## 🌐 `Method 2` :: Transfer File From SFTP to Switch

To Transfer File **From SFTP to Switch**, follow next steps:

### 📥 Copy From SFTP to Flash

```bash
copy sftp: flash:
```

You will be prompted for:

```
Address or name of remote host? 10.10.10.100

Source filename? cat9k_iosxe.17.12.05.SPA.bin

Destination filename? (press Enter)

Username? fz3r0-admin

Password? p4ssw0rd.12345

```

### 📦 Verify File Exists

```bash
dir flash: | include .bin
```

### 🔎 Verify File Integrity using MD5 (if needed)

```bash
verify /md5 flash:cat9k_iosxe.17.12.05.SPA.bin
```

Compare the hash with the official Cisco MD5 value.

## 🔌 `Method 3` :: USB ↔ Switch (Air-Gapped Method)

To Transfer File **From USB drive to Switch** or **From Switch to USB drive**, follow next steps:

### 📂 Check USB

Insert USB into switch.

```bash
dir usbflash0:
```

You should see something like:

```
cat9k_iosxe.17.12.05.SPA.bin
capture.pcap
file-random.jpg
```

---

### 📥 Copy From USB to Flash

```bash
copy usbflash0:cat9k_iosxe.17.12.05.SPA.bin flash:
```

### 📦 Verify

```bash
dir flash: | include .bin
```

---

### 📥 Copy From Flash to USB

```bash
copy flash:cat9k_iosxe.17.12.05.SPA.bin usbflash0:
```

Or example with a capture file:

```bash
copy flash:capture.pcap usbflash0:
```

### 📦 Verify

```bash
dir usbflash0:
```




# 🔁 `Method 4` :: Switch to Switch Using SCP

To Transfer File **From Switch to Switch (using SCP)**, follow next steps:

### 🔐 Step 1 – Enable SCP Server on Destination Switch

On **Switch-B (10.10.20.20)**:

```bash
configure terminal
 ip scp server enable
 ip ssh version 2
end
```

Make sure a local user exists:

```bash
username fz3r0-admin privilege 15 secret p4ssw0rd.switch
```

### 📤 Step 2 – Copy From Switch-A to Switch-B

On **Switch-A (10.10.10.10)**:

```bash
copy flash:cat9k_iosxe.17.12.05.SPA.bin scp:
```

Prompts:

```
Address or name of remote host? 10.10.20.20

Destination username? fz3r0-admin

Destination filename? (press Enter)

Password? p4ssw0rd.switch

```

### 📦 Verify on Destination

On Switch-B:

```bash
dir flash: | include 17.12.05
```

## 🧪 Always Verify After Transfer

### 🔎 Check File Size

```bash
dir flash:cat9k_iosxe.17.12.05.SPA.bin
```

Compare with official Cisco size.

### 🔐 Verify MD5

```bash
verify /md5 flash:cat9k_iosxe.17.12.05.SPA.bin
```

Never skip integrity verification in production environments.


## 🧹 Optional Cleanup

If you staged the image or any file temporarily:

```bash
delete flash:old_image.bin

or

delete flash:capture.pcap
```

Or clean unused packages (only if doing firmware work):

```bash
install remove inactive
```

⚠ Only use this when you understand install mode behavior.


