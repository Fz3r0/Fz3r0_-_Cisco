## Basic NXOS upgrade:

````
!# 1 Validate images on flash or usb

dir usb1:
dir bootflash:

!# 2 Save Running Config (Device will be reloaded!!!)

wr

!# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=   

!# 3. Copy image to device

!### - sftp or scp from other device/server:

copy sftp://admin@10.10.0.1/bootflash:/nxos64-cs.10.2.8.M.bin bootflash:
copy sftp://admin@10.10.0.1/nxos64-cs.10.2.8.M.bin bootflash:

copy scp://admin@10.10.0.1/nxos64-cs.10.2.8.M.bin bootflash:
copy scp://admin@10.10.0.1/bootflash:/nxos64-cs.10.2.8.M.bin bootflash:

!### - usb1:

copy usb1:nxos64-cs.10.2.8.M.bin bootflash:

!# IF "READY ONLY" ISSUE OCCUR YOU NEED TO SAVE AND RELOAD THE SWITCH TO FREE MEMORY
wr
reload

!# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=   

!# 2. Validate image is in bootflash:
 
dir bootflash: 

!# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=    

!# 3. Install Image:
install all nxos bootflash:nxos64-cs.10.2.8.M.bin

# reboot
Reboot? yes

!# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=    

!# 4. Validate After Reboot
show version


````









## 1

![image](https://github.com/user-attachments/assets/b6cd23e5-e5c8-4288-9cf4-3c63fee885da)

## 2

Hay 2 tipos de software y el NX-OS necesita los 2 par aarrancar!!!

- Primero el Kernell (keckstart)
- Luego el OS

![image](https://github.com/user-attachments/assets/4de677d8-edf6-4ed8-9e7a-53c025769327)

- (Los SMU son parches del sistema operatico)

![image](https://github.com/user-attachments/assets/1a09bb7b-a1b4-466b-8fac-5a425e476a0f)
















---

## Resources

- https://youtu.be/OkjhkyISoGs?si=kUmT4WgdXFGtAwea
- https://www.youtube.com/watch?v=OkjhkyISoGs&t=262s
