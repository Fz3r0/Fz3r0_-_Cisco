
## Credenciales Default:

- Nexus Titanium: `admin/admin`
- Nexus 9k: `admin/(no password)`
- EVE Server: `root/eve`
- EVE Web: `admin/eve`
  
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

- Añadir permisos como dice la documentación de EVENG: https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-nexus-9000v-switch/

    - agregar: `/opt/unetlab/wrappers/unl_wrapper -a fixpermissions` 

![image](https://github.com/user-attachments/assets/6f6e7219-5a99-43d0-b330-4c9b82668efd)

---

6. Disfrutar!!! Ya sale en EVE-NG

OJO! Recomendable dejarlos con 8 gigas de RAM y no tocar eso:

![image](https://github.com/user-attachments/assets/e4a8f15d-9d9d-490d-8373-1d1455d0add8)

Después de encender hay que esperar de 1 a 5 minutos dependiendo la versión:

![image](https://github.com/user-attachments/assets/e1822754-289c-4c45-84ac-67ab719686fe)

En caso del Nexus 9k después de los 5 minutes de arranque para empezar de 0 lo mejor es seleccionar:

- Provisioning: `skip`
- Credenciales: `admin/(enter)`




# Resources

- https://youtu.be/4-JgE4oU7oI?si=LUhy-SZGBBI8hCwg
- https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-nexus-9000v-switch/
