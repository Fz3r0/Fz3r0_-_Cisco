
---

1. entrar a ver los naming del nexus a ve ng: https://www.eve-ng.net/index.php/documentation/qemu-image-namings/
- `nxosv9k` -	NX9K Cisco Nexus ( SATA best perf) `sataa`
- `titanium` -	NXOS Titanium Cisco	`virtioa`

---

2. Encender EVE NG virtual y entrar por filezilla o WinSCP usando `root/eve` puerto `22`

---

3. Ir al directorio donde tengo mis imagenes: `Qemu image location is /opt/unetlab/addons/qemu/`

![image](https://github.com/user-attachments/assets/87658d75-bb61-4f4c-9712-d61fe3d06e5f)

---

4. Crear las 2 carpetaS junto con la verison que usaremos y cambiar el nombre del nexus:

![image](https://github.com/user-attachments/assets/68b51236-f9dd-4a72-9e8a-429d60efc31c)


- `nxosv9k-10.1.1` y el nombre del archivo cambiarlo por `sataa`

![image](https://github.com/user-attachments/assets/67452a8e-7629-40d6-a59a-03f8343cc2b2)


- `titanium-7.3.0` y el nombre del archivo cambiarlo por `virtioa`

![image](https://github.com/user-attachments/assets/7fbf8792-1804-446c-a022-4624545b87f3)

---

5. Arreglar permisos (Fix permissions)

- Entrar a EVE server pero ahora por SSH con las mismas credenciales `root/eve` puerto `22`

-  


# Resources

- https://youtu.be/4-JgE4oU7oI?si=LUhy-SZGBBI8hCwg
- https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-nexus-9000v-switch/
