# Prueba con cliente por línea de comandos.

## Instalamos los clientes por linea de comandos.

Los clientes que vamos a usar son ftp, lftp y curl. Ejecutamos el siguiente comando:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/01-img.png)


## Conexion al servidor localhost 

## Configuración del servidor FTP (vsftpd)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/02-img.png)

Iniciamos sesión con nuestro usuario de sistema que tiene permisos sobre la carpeta a la cual apunta nuestro servidor

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/03-img.png)


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/04-img.png)

Este no es el objetivo de la práctica ya que nuestro servidor apuntaba a la $HOME por lo que a continuación dejo el fichero vsftpd.conf configurado para
/srv/ftpgroup-cosmin



### Objetivo

Creamos nuestro fichero test.txt y escribimos una frase:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/05-img.png)


Aislar al usuario en un directorio propio (`/srv/ftpgroup-cosmin`) y permitirle subir archivos solo dentro de una carpeta segura (`upload`).

### Directivas necesarias en `/etc/vsftpd.conf`

```
listen=YES
listen_port=21
listen_ipv6=NO

pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100

anonymous_enable=YES
anon_root=/srv/ftp/anon

local_enable=YES
write_enable=YES

use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES

chroot_local_user=YES
allow_writeable_chroot=YES
local_root=/srv/ftpgroup-cosmin

max_clients=5
max_per_ip=2

secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd

ssl_enable=NO
```


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/06-img.png)


### Explicación de las directivas clave

- **local_enable=YES**  
  Permite que usuarios del sistema inicien sesión por FTP.

- **write_enable=YES**  
  Habilita comandos de escritura (subidas, borrados, renombrados).

- **chroot_local_user=YES**  
  Encierra al usuario en su directorio raíz remoto para evitar que navegue por el sistema.

- **allow_writeable_chroot=YES**  
  Necesario para que el usuario pueda escribir dentro del chroot.

- **local_root=/srv/ftpgroup-cosmin**  
  Define el directorio remoto que verá el usuario al conectarse.

---

## Permisos necesarios en el sistema de archivos

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/07-img.png)


### Estructura creada

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/08-img.png)


```
/srv/ftpgroup-cosmin
└── upload
```

### Permisos aplicados

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/09-img.png)


```
sudo chmod 755 /srv/ftpgroup-cosmin
sudo mkdir -p /srv/ftpgroup-cosmin/upload
sudo chown cosmin:cosmin /srv/ftpgroup-cosmin/upload
```

### Explicación

- El directorio raíz del chroot **no puede ser escribible** por el usuario (requisito de vsftpd).
- La carpeta `upload` sí debe ser escribible para permitir subidas.
- Por eso `upload` pertenece a `cosmin:cosmin`.

---

## Reinicio del servicio

```
sudo systemctl restart vsftpd
```

---

## Conexión desde el cliente FTP

### Conectarse al servidor

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/10-img.png)


```
ftp localhost
```

### Ver el directorio remoto

```
pwd
```

Salida esperada:

```
/
```

(que corresponde a `/srv/ftpgroup-cosmin` dentro del chroot)

### Listar contenido remoto

```
ls
```

Salida esperada:

```
upload
```

---

## Subida del archivo desde el Escritorio

### Cambiar directorio local

```
lcd /home/cosmin/Escritorio
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/12-img.png)


### Comprobar archivo local

```
!ls
```

Debe aparecer `test.txt`.

### Entrar en la carpeta remota con permisos

```
cd upload
```

### Subir el archivo

```
put test.txt
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/13-img.png)


Salida correcta:

```
150 Ok to send data.
226 Transfer complete.
```

---

## Descarga del archivo

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/06-Prueba-Cliente-Comandos/images/14-img.png)


```
get test.txt
```

---

## Explicación global del proceso

1. **Configuras vsftpd** para que el usuario quede enjaulado en un directorio seguro y aislado.  
2. **Ajustas permisos** para cumplir los requisitos de seguridad del chroot y permitir escritura solo donde corresponde.  
3. **Reinicias el servicio** para aplicar los cambios.  
4. **Te conectas por FTP** desde la terminal.  
5. **Navegas entre directorios locales y remotos** usando `lcd`, `cd`, `ls` y `!ls`.  
6. **Subes el archivo** desde tu Escritorio a la carpeta `upload` del servidor usando `put`.  
7. (Opcional) **Descargas archivos** con `get` para completar la práctica.
