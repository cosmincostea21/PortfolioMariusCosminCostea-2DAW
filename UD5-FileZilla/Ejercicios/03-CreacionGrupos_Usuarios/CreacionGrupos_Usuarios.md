# Creación de usuarios y grupos

---

## 1. Guía paso a paso

### 1.1 Creación de grupo con limitaciones

```
sudo groupadd ftpgroup-cosmin
getent group | grep ftp*
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/01-img.png)

Resultado esperado:

```
ftp:x:124:
ftpgroup-cosmin:x:1002:
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/02-img.png)


---

### 1.2 Creación de dos usuarios asociados al grupo

```
sudo useradd -m -s /bin/bash -G ftpgroup-cosmin user1
sudo useradd -m -s /bin/bash -G ftpgroup-cosmin user2
getent group | grep ftp*
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/03-img.png)


Resultado esperado:

```
ftpgroup-cosmin:x:1002:user1,user2
```

---

### 1.3 Definir directorio raíz

Creación del directorio:

```
sudo mkdir -p /srv/ftpgroup-cosmin
tree /srv/
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/04-img.png)


Cambio de propietario:

```
sudo chown root:ftpgroup-cosmin /srv/ftpgroup-cosmin
ls -ld /srv/ftpgroup-cosmin
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/05-img.png)


Resultado esperado:

```
drwxrwx--- 2 root ftpgroup-cosmin 4096 ene 23 17:54 /srv/ftpgroup-cosmin
```

---

### 1.4 Definir directorio raíz para los usuarios

Edición del archivo de configuración:

```
sudo nano /etc/vsftpd.conf
```

Añadir:

```
chroot_local_user=YES
allow_writeable_chroot=YES
local_root=/srv/ftpgroup-cosmin
```
![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/06-img.png)



Estas directivas encierran a los usuarios dentro del directorio asignado.

---

### 1.5 Permisos de lectura, escritura y borrado

Comprobación:

```
ls -l /srv
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/07-img.png)


Permisos recomendados:

```
sudo chmod 770 /srv/ftpgroup-cosmin
```

Si se quiere restringir más:

```
sudo chmod 750 /srv/ftpgroup-cosmin
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/03-CreacionGrupos_Usuarios/images/08-img.png)


---

### 1.6 Límites de conexión

En `/etc/vsftpd.conf`:

```
max_clients=5
max_per_ip=2
```

Explicación:

- **max_clients** limita el número total de conexiones simultáneas.
- **max_per_ip** limita cuántas conexiones puede abrir una misma IP.

---

## 2. Diferencias entre permisos de grupo y permisos de usuario

### Permisos de usuario (propietario)

- Aplican solo al dueño del archivo o directorio.
- Tienen prioridad sobre los permisos del grupo.
- Determinan si el propietario puede leer, escribir o ejecutar.

Ejemplo:  
Si el propietario tiene `rw-`, puede modificar el archivo aunque el grupo no tenga permisos de escritura.

### Permisos de grupo

- Afectan a todos los usuarios pertenecientes al grupo asignado.
- Permiten gestionar permisos comunes sin configurarlos uno por uno.
- Pueden ser lectura, escritura o ejecución.

Ejemplo:  
Si un directorio pertenece al grupo `ftpgroup-cosmin` y tiene permisos `rwx`, todos los usuarios del grupo podrán leer, crear y borrar archivos.

### Diferencia clave

- **Usuario** → afecta solo al propietario.  
- **Grupo** → afecta a todos los usuarios del grupo.  
- Si un usuario es propietario, **sus permisos prevalecen**.
