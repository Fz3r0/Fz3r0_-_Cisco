
## How to recover a password without losing configuration:

1 - Insert Console cable and login to the switch until you have the login prompt _(if you try any user/pass you will fail the login)_:

<img width="617" height="603" alt="image" src="https://github.com/user-attachments/assets/c40c577f-9abc-433f-9176-65923fe1b26b" />

2 - Turn off Router (keep you console window opened to see the boot process)

<img width="919" height="147" alt="image" src="https://github.com/user-attachments/assets/6773338e-163b-41fb-81fe-7828fdab118b" />

3 - Wait some seconds, and turn on the router:

<img width="932" height="127" alt="image" src="https://github.com/user-attachments/assets/eea25930-fc34-4cc8-b99a-7587c945045d" />

4 - Go to putty and right click top of the window, then click special commands > break. _(You need to click **MULTIPLE TIMES FAST** TO "CATCH THE BOOT", until you get to the Rommon mode)_

`CTRL + c` like crazy!

<img width="570" height="494" alt="image" src="https://github.com/user-attachments/assets/77abc79a-08e3-43df-a2a8-8aa7901bfbc5" />

5 - Once in ROMMON mode prompt, add the next command _(Change the default Configuration Register Value to 0x2142)_: 

````py
confreg 0x2142
````

6 - Restart the router:

````
reset
````

7 - Wait until the router boots...

<img width="545" height="214" alt="image" src="https://github.com/user-attachments/assets/70938559-41cf-4479-acf2-6bd9d51fc92a" />

8 - Enter to the switch (it will not have any password), and copy the old config intro running config (so I don't lose the old config) tip: this is the inverse command that we usually to save :P), wait and you will see a prompt similar to `s705r2#`

````
enable

copy startup-config running-config


````

9 - Change the enable secret & admin/password + save configuration

en los routers puse `admin.cisco` y el otro `cisco.12345` cambialo!

````
configure terminal

username admin privilege 15 secret Cisco.12345
!
enable secret Cisco.12345

end
write memory

!
!


````

10 - Change register to `0x2102` + save configuration

````
configure terminal

config-register 0x2102

end
write memory

!
!


````

<img width="680" height="632" alt="image" src="https://github.com/user-attachments/assets/bc84252f-6f88-4409-970f-dc6a80696f5e" />

11 -  restart the router

````
reload


````

12 - You now can access access with the new admin user & pass `admin`:`admin.cisco` & enable pass: `cisco.12345`

<img width="611" height="794" alt="image" src="https://github.com/user-attachments/assets/dc5f2f02-fc21-45b1-aa2d-78a8242dc244" />

<img width="833" height="68" alt="image" src="https://github.com/user-attachments/assets/bba9a0bb-e41d-46ae-9ebe-51771ff3eebe" />



## Check current configuration, licenses, etc

````py
## Check running config

show running-configuration

## How to Verify License and Firmware on a Cisco ISR 4331

! # 1. Check active licenses installed and currently in use
show license summary
! Expected: Lists securityk9, appxk9, uck9, etc., with their status (IN USE, NOT IN USE).

! # 2. Check the hardware throughput level (license-based performance)
show platform hardware throughput level
! Expected: Will show something like 100000 kb/s (100 Mbps) or higher if upgraded.

! # 3. Check IOS XE version, ROM, and system image currently running
show version
! Expected output includes:
!   - Cisco IOS XE Software Version (ex: 17.03.05)
!   - System image file name (ex: bootflash:isr4300-universalk9.17.03.05.SPA.bin)
!   - ROM version (ex: 16.12(2r))
!   - Licensing details and router mode
!   - Memory, flash, and interface summary
````



# Backup current config @ local USB

Cisco ISR 4331 only supports USB drives formatted in FAT32, and it’s safest to use drives 8–16 GB max; when inserted, the router mounts it as `usb0:` or `usbflash0:` and you can copy configs, IOS images, or backups directly to/from it.

1. Insert the USB drive into the router.
    The router will recognize it as 'usbflash0:' (you can verify with 'dir usbflash0:').

2. Verify the USB is detected and list its contents.

````py
dir usb0:
````

Example: 

````py
Fz3r0-Router-4331#
*Jul 27 11:59:31.696: %IOSD_INFRA-6-IFS_DEVICE_OIR: Device usb0 added  <<<<<---------| log shows usb0: added
Fz3r0-Router-4331#

Fz3r0-Router-4331#dir usb0:                                            <<<<<---------| Dir shows empty usb (it was just formatted)
Directory of usb0:/

113     drwx             4096  Jul 27 2025 06:43:58 -05:00  System Volume Information

7806648320 bytes total (7806631936 bytes free)

Fz3r0-Router-4331#
````

3. Save the current startup configuration to the USB.

````
copy startup-config usb0:startup-config-backup.cfg
````

3. (Optional) Save the running configuration as well, just in case.

````py
copy running-config usb0:running-config-backup.cfg
````

4. (Optional) If you want to back up the IOS image or any file from flash, copy it too.

````py
copy flash:<filename> usb0:<filename>
````

5. Confirm the files are on the USB.

````py
dir usbflash0:
````

````
Directory of usb0:/

118     -rwx        702197190  Jul 27 2025 12:23:24 -05:00  isr4300-universalk9.17.03.04a.SPA.bin
117     -rwx          1005451  Jul 27 2025 12:20:45 -05:00  ISReigrp.pcap
116     -rwx        702560970  Jul 27 2025 12:11:49 -05:00  isr4300-universalk9.17.03.05.SPA.bin
115     -rwx            54088  Jul 27 2025 12:09:38 -05:00  running-config-backup.cfg
114     -rwx            44304  Jul 27 2025 12:08:55 -05:00  startup-config-backup.cfg
113     drwx             4096  Jul 27 2025 06:43:58 -05:00  System Volume Information

7806648320 bytes total (6400761856 bytes free)
s705r2#

*Jul 27 12:28:47.763: %IOSD_INFRA-6-IFS_DEVICE_OIR: Device usb0 removed



````















## Wipe config

### Mandatory

````py
! # 1. Erase the startup configuration so the router boots with no config
write erase

! # 2. Reload the router to apply the wipe and start fresh
reload

!
!

````

### Optional (CAUTION!!!)

````py
! # -. Check the flash storage for leftover files (old configs, backups, etc.)
dir flash:

! # -. Manually delete any unwanted files you find in flash
delete flash:<filename>

! # -. (Optional) Clear any stored license information to leave the router completely clean
license clear

!
!

````

### Example:

````
Fz3r0-Router-4331#
Fz3r0-Router-4331#write erase
************************************************************************************************************
Erasing Nvram will not clear license trust code.
************************************************************************************************************
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
Fz3r0-Router-4331#
*Jul 27 12:34:42.962: %SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
Fz3r0-Router-4331#
Fz3r0-Router-4331#reload

System configuration has been modified. Save? [yes/no]: no
Proceed with reload? [confirm]

*Jul 27 12:35:26.976: %SYS-5-RELOAD: Reload requested by admin on console. Reload Reason: Reload Command.Jul 27 12:35:39.350: %PMAN-5-EXITACTION: R0/0:vp: Process manager is exiting: process exit with reload chassis code

Initializing Hardware ...

.
.
.

````

For a blank init setup choose: 

````
         --- System Configuration Dialog ---

Would you like to enter the initial configuration dialog? [yes/no]: no <<<---

Would you like to terminate autoinstall? [yes]: yes <<<---


````


## CONFIGURE MANAGEMENT INTERFACE (Gi0)

Now we will have access with an ethernet port or the serial 

````py
! # Configure line:
enable
configure terminal 

! # Interface MANAGEMENT
interface GigabitEthernet0
   description *** OOB MANAGEMENT PORT ***
      ip address 192.168.1.201 255.255.255.0   
no shutdown
exit

! # DEFAULT ROUTE FOR MANAGEMENT - Points all management traffic to your gateway
ip route 0.0.0.0 0.0.0.0 192.168.1.254 

! # ENABLE SSH ACCESS FOR MANAGEMENT
hostname Fz3r0-R-4331
ip domain name fz3r0.com
crypto key generate rsa modulus 2048
ip ssh version 2

! # CREATE LOCAL USER FOR SSH LOGIN
username admin privilege 15 secret cisco.12345

! # CONFIGURE VTY LINES FOR SSH ONLY
line vty 0 4
   transport input ssh
   login loc
exit

end
write memory

!
!


````


















































---



































## Switch

1. Apagar el sdwitch por completo

2. sostener el boton de enfrente

3. conectar el cable de power par aque el switch prenda (el botón ya estaba presionado, y mantenerlo aun presionado)

- MANTENER EL BOTON PRESIONADO DESDE EL ARRANQUE HASTA QUE SE PONGA AMBAR, TARDA BASTANTE UNSO 20 SEGUNDOS MINIMO, UNA VEZ EN AMBAR YA SE PUEDE SOLTAR

<img width="1179" height="594" alt="image" src="https://github.com/user-attachments/assets/2a0aaa66-78ff-46da-8021-79ac7176f63d" />

Una vez ambos en ambar, ya estarás dentro de switch recover

<img width="685" height="511" alt="image" src="https://github.com/user-attachments/assets/94efb1e2-66af-4b27-978c-ad7a5417103c" />

4.  Initialize the flash file system.
- Command: `flash_init`

````
switch: flash_init
Initializing Flash...

flashfs[7]: 0 files, 1 directories
flashfs[7]: 0 orphaned files, 0 orphaned directories
flashfs[7]: Total bytes: 6784000
flashfs[7]: Bytes used: 1024
flashfs[7]: Bytes available: 6782976
flashfs[7]: flashfs fsck took 2 seconds....done Initializing Flash.

switch: 
````

5.  Ignore the startup configuration with the following command:

- Device: `SWITCH_IGNORE_STARTUP_CFG=1`

````
switch: SWITCH_IGNORE_STARTUP_CFG=1

switch: 
````

6. Boot the switch with the packages.conf file from flash.

Device: `boot flash:packages.conf`

7. Terminate the initial configuration dialog by answering `No`

````
switch: SWITCH_IGNORE_STARTUP_CFG=1

switch: boot flash:packages.conf
Getting rest of image
Reading full image into memory....done
Reading full base package into memory...: done = 79122052
Bundle Image
--------------------------------------
Kernel Address    : 0x6042f350
Kernel Size       : 0x402ecf/4206287
Initramfs Address : 0x60832220
Initramfs Size    : 0xdb9c62/14392418
Compression Format: .mzip

Bootable image at @ ram:0x6042f350
Bootable image segment 0 address range [0x81100000, 0x82110000] is in range [0x80180000, 0x90000000].
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@boot_system: 377
Loading Linux kernel with entry point 0x81653a10 ...
Bootloader: Done loading app on core_mask: 0xf

### Launching Linux Kernel (flags = 0x5)

.
.
.
.

Enable secret warning
----------------------------------
In order to access the device manager, an enable secret is required
If you enter the initial configuration dialog, you will be prompted for the enable secret
If you choose not to enter the intial configuration dialog, or if you exit setup without setting the enable secret,
please set an enable secret using the following CLI in configuration mode-
enable secret 0 <cleartext password>
----------------------------------
Would you like to enter the initial configuration dialog? [yes/no]: no <<<<<<<<<<<<<<<<<<< NO!


````

8. At the switch prompt, enter privileged EXEC mode with `enable` & `Copy the startup configuration to running configuration` .

````
Switch>
Switch>enable
Switch#copy startup-config running-config
Destination filename [running-config]? 

*Aug 17 23:27:31.573: %SYS-4-CONFIG_NEWER: Configuration from version 15.2 may not be correctly understood
qos queue-softmax-multiplier 100
    ^
% Invalid input detected at '^' marker.
Failed to generate persistent self-signed certificate.
    Secure server will use temporary self-signed certificate.

3075 bytes copied in 10.120 secs (304 bytes/sec)
SWITCH-OLD-PASSWORD-LOST#  <<<<<<<<<<<<<<< switch original name again!
````

9. Copy the startup configuration to running configuration.

- `copy startup-config running-config` 

Press Return in response to the confirmation prompts. The configuration file is now reloaded, and **you can change the
password.**

10. Enter global configuration mode and change the enable password & copy running-config startup-config

````
configure terminal

username admin privilege 15 secret Cisco.12345
!
enable secret Cisco.12345

end
write memory
copy running-config startup-config

!
!


````

11. Reload the device.

Device# reload

12. You will be again on "switch" with fabirc configs, no pass, no nothing. 



---


recuperar:

Procedimiento para evitar que el switch arranque en 'switch>' y cargue siempre la configuración:

1. Revisar archivos en flash (confirmar que existe packages.conf)
---------------------------------------------------------------
dir flash:

2. Apuntar el boot al archivo correcto
---------------------------------------------------------------
conf t
 boot system switch all flash:packages.conf
end
write memory

3. Verificar variables de boot
---------------------------------------------------------------
show boot

   (Debe salir: BOOT variable = flash:packages.conf;  Manual Boot = no)

4. Arreglar el config-register
---------------------------------------------------------------
conf t
 config-register 0x2102
end
write memory

5. Validar el config-register
---------------------------------------------------------------
show version | include register

   (Debe salir: Configuration register is 0x142 (will be 0x2102 at next reload))

6. Reiniciar para aplicar cambios
---------------------------------------------------------------
reload

7. Después del reload validar
---------------------------------------------------------------
show version | include register        ! debe ser 0x2102 (o 0x102 en algunos modelos)
show boot                              ! BOOT variable = flash:packages.conf; en ambos
show startup-config | include hostname ! tu hostname guardado
"""






























## Resources

- https://www.youtube.com/watch?v=WYQMJoIKvvA
- https://youtu.be/WYQMJoIKvvA?si=eTtJN8e7a4nOzObk
- https://www.youtube.com/watch?v=HXRiFsimDhE
- https://www.youtube.com/watch?v=a0kSJUYQrt4
- https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst3650/software/release/3se/system_management/configuration_guide/b_sm_3se_3650_cg.pdf

















<img width="915" height="36" alt="image" src="https://github.com/user-attachments/assets/616c16ac-0223-4e4e-a784-29662f7aafbe" />
































