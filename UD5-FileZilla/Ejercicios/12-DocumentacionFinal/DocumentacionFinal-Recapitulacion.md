# **Documentación del Servidor FTP/FTPS con vsftpd, FileZilla y Apache**

## **1. Introducción**

El objetivo de este documento es describir el proceso completo de instalación, configuración y uso de un servidor FTP/FTPS basado en **vsftpd**, junto con la utilización de **FileZilla**, el cliente FTP por línea de comandos y la integración con **Apache** para publicar contenido web.  

A lo largo de las prácticas se han configurado distintos escenarios: acceso con usuarios locales, cambios de directorio raíz, FTPS, modos activo y pasivo, y finalmente la publicación de una página web subida por FTP y servida por Apache.

---

## **2. Conceptos fundamentales**

### **2.1 FTP**
FTP es un protocolo de transferencia de archivos basado en dos canales: uno de control (puerto 21) y otro de datos. Permite subir, descargar y gestionar archivos, pero no cifra la información.

### **2.2 FTPS**
FTPS añade cifrado SSL/TLS al protocolo FTP. Mantiene la estructura de dos canales, pero protege credenciales y datos mediante certificados digitales.

### **2.3 SFTP**
SFTP es un protocolo distinto basado en SSH. Utiliza un único canal cifrado y el puerto 22. No fue el foco de estas prácticas, pero sirve como referencia de comparación.

---

## **3. Instalación y puesta en marcha de vsftpd**

La instalación se realizó con:

```bash
sudo apt install vsftpd
```

El servicio se habilitó para iniciar automáticamente:

```bash
sudo systemctl enable vsftpd
sudo systemctl start vsftpd
```

El archivo principal de configuración es:

```bash
/etc/vsftpd.conf
```

Cualquier error en este archivo puede impedir que el servicio arranque, por lo que cada cambio se acompañó de una comprobación del estado del servicio.

---

## **4. Configuración básica del servidor FTP**

Se activó el uso de IPv4 y el puerto estándar:

```bash
listen=YES
listen_ipv6=NO
listen_port=21
```

Se habilitó el acceso de usuarios locales y la escritura:

```bash
local_enable=YES
write_enable=YES
```

Y se activaron mensajes de directorio, logs y hora local:

```bash
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
```

Estas directivas proporcionan un funcionamiento básico y permiten auditar la actividad del servidor.

---

## **5. Directorios raíz utilizados en las prácticas**

Durante las prácticas se trabajó con varios directorios raíz, según el objetivo de cada ejercicio.

### **5.1 /srv/ftpgroup-cosmin**

Se utilizó para las prácticas de:

- Creación de grupo y usuarios específicos.
- Permisos compartidos.
- Enjaulado de usuarios locales.

Configuración en `vsftpd.conf`:

```bash
local_root=/srv/ftpgroup-cosmin
```

Creación y permisos:

```bash
sudo mkdir -p /srv/ftpgroup-cosmin
sudo chown root:ftpgroup-cosmin /srv/ftpgroup-cosmin
sudo chmod 770 /srv/ftpgroup-cosmin
```

### **5.2 /var/www/ftpweb-cosmin**

Este directorio se utilizó para integrar FTP con Apache. La idea era que el usuario subiera archivos mediante FTP y que Apache los sirviera como página web.

Configuración en `vsftpd.conf`:

```bash
local_root=/var/www/ftpweb-cosmin
```

Creación y permisos:

```bash
sudo mkdir -p /var/www/ftpweb-cosmin
sudo chown cosmin:cosmin /var/www/ftpweb-cosmin
sudo chmod 755 /var/www/ftpweb-cosmin
```

Además, se configuró:

```bash
local_umask=022
```

para que los archivos subidos desde FileZilla se crearan con permisos `644`, permitiendo que Apache los leyera sin problemas.

---

## **6. Usuarios, grupos y permisos**

Se creó un grupo específico:

```bash
sudo groupadd ftpgroup-cosmin
```

Y se añadieron usuarios al grupo:

```bash
sudo useradd -m -g ftpgroup-cosmin usuario1
sudo useradd -m -g ftpgroup-cosmin usuario2
```

El enjaulado de usuarios locales se activó con:

```bash
chroot_local_user=YES
allow_writeable_chroot=YES
```

Esta combinación es delicada: si se activa el chroot sin permitir escritura, vsftpd puede negarse a arrancar. Fue una de las incidencias detectadas y corregidas durante las prácticas.

---

## **7. Seguridad: FTPS**

Para habilitar FTPS se generó un certificado TLS:

```bash
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/vsftpd.key \
  -out /etc/ssl/certs/vsftpd.crt
```

Se activó FTPS explícito en `vsftpd.conf`:

```bash
ssl_enable=YES
rsa_cert_file=/etc/ssl/certs/vsftpd.crt
rsa_private_key_file=/etc/ssl/private/vsftpd.key
force_local_logins_ssl=YES
force_local_data_ssl=YES
ssl_tlsv1=YES
require_ssl_reuse=NO
ssl_ciphers=HIGH
```


## **8. Modos activo y pasivo**

Se configuró un rango de puertos pasivos:

```bash
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
```

En FileZilla se probaron ambos modos:

- **Modo activo:** el servidor inicia la conexión de datos hacia el cliente.
- **Modo pasivo:** el cliente inicia la conexión hacia un puerto del rango pasivo del servidor.

El modo pasivo resultó más estable y compatible con firewalls y NAT, tal y como se esperaba.

---

## **9. Uso de FileZilla**

FileZilla fue el cliente principal para probar el servidor. En el Gestor de sitios se configuró:

- Servidor: `localhost`
- Puerto: `21`
- Cifrado: **FTP explícito sobre TLS**
- Usuario: `cosmin` (u otros usuarios locales)
- Contraseña: la del sistema

Con esta configuración se realizaron las siguientes acciones:

- Subida de archivos (equivalente al comando `put`).
- Descarga de archivos (equivalente a `get`).
- Comprobación de que el directorio remoto mostrado correspondía al `local_root` configurado en cada momento.
- Verificación de que los archivos subidos a `/var/www/ftpweb-cosmin` aparecían con permisos `644` gracias a `local_umask=022`.

FileZilla permitió visualizar de forma clara el efecto de los cambios en `vsftpd.conf`, especialmente al cambiar el directorio raíz o al activar FTPS.

---

## **10. Uso del cliente FTP por línea de comandos**

Además de FileZilla, se utilizó el cliente FTP de terminal para comprobar el funcionamiento sin interfaz gráfica:

```bash
ftp localhost
```

Una vez dentro, se usaron comandos como:

- `ls` para listar archivos.
- `cd` para cambiar de directorio.
- `put index.html` para subir un archivo.
- `get archivo` para descargarlo.
- `bye` para salir.

Estas pruebas confirmaron que el servidor respondía correctamente tanto a clientes gráficos como a clientes de consola.

---

## **11. Integración con Apache: VirtualHost y publicación web**

Una parte importante de la práctica fue demostrar que los archivos subidos por FTP podían ser servidos por Apache como contenido web.

### **11.1 Creación del directorio web**

Se utilizó el directorio:

```bash
/var/www/ftpweb-cosmin
```

como DocumentRoot del sitio web. Se ajustaron permisos:

```bash
sudo mkdir -p /var/www/ftpweb-cosmin
sudo chown cosmin:cosmin /var/www/ftpweb-cosmin
sudo chmod 755 /var/www/ftpweb-cosmin
```

### **11.2 Configuración de /etc/hosts**

Para poder acceder al sitio mediante un nombre amigable, se añadió la siguiente línea al archivo `/etc/hosts`:

```bash
127.0.0.1 ftpweb.local
```

Esto hace que el nombre `ftpweb.local` resuelva a la propia máquina.

### **11.3 Creación del VirtualHost en Apache**

Se creó un archivo de configuración, por ejemplo:

```bash
/etc/apache2/sites-available/ftpweb.conf
```

con el siguiente contenido:

```apache
<VirtualHost *:80>
    ServerName ftpweb.local
    DocumentRoot /var/www/ftpweb-cosmin

    <Directory /var/www/ftpweb-cosmin>
        Require all granted
    </Directory>
</VirtualHost>
```

Se activó el sitio y se recargó Apache:

```bash
sudo a2ensite ftpweb.conf
sudo systemctl reload apache2
```

### **11.4 Subida de la página web por FTP**

Desde FileZilla (o desde el cliente FTP de terminal), se subió un archivo `index.html` al directorio remoto, que en este contexto apuntaba a:

```bash
/var/www/ftpweb-cosmin
```

Gracias a la configuración de `local_umask=022`, el archivo se creó con permisos `644`, lo que permitió a Apache leerlo sin problemas.

Finalmente, se accedió desde el navegador a:

```bash
http://ftpweb.local/
```

y se comprobó que la página `index.html` subida por FTP se mostraba correctamente. Este fue el punto de unión entre el servidor FTP y el servidor web.

---

## **12. Directivas críticas en vsftpd.conf**

Durante las prácticas se identificaron varias directivas especialmente delicadas:

- `chroot_local_user` y `allow_writeable_chroot`: si se enjaula al usuario sin permitir escritura, el servicio puede fallar.
- `local_root`: debe apuntar a un directorio existente; de lo contrario, el usuario puede quedar en `/` o producir errores.
- `local_umask`: determina los permisos de los archivos subidos; un valor incorrecto puede provocar errores 403 en Apache.
- `pasv_min_port` y `pasv_max_port`: si el rango es inválido o está bloqueado por el firewall, el modo pasivo no funciona.
- `ssl_enable`, `rsa_cert_file` y `rsa_private_key_file`: requieren un certificado válido y permisos correctos en la clave privada.

Cada cambio en estas directivas se acompañó de un reinicio del servicio:

```bash
sudo systemctl restart vsftpd
```

y de pruebas con FileZilla o el cliente FTP de terminal.

---

## **13. Recomendaciones de administración**

- Usar FTPS siempre que sea posible, especialmente fuera de redes internas.
- Mantener certificados actualizados y con permisos adecuados.
- Limitar el número de conexiones:
  ```bash
  max_clients=5
  max_per_ip=2
  ```
- Revisar periódicamente los logs de vsftpd.
- Realizar copias de seguridad de los directorios raíz activos (`/srv/ftpgroup-cosmin`, `/var/www/ftpweb-cosmin`).
- Proteger el servidor con firewall y, si es necesario, herramientas como Fail2ban.
- Evitar cambios masivos en `vsftpd.conf` sin probarlos de forma incremental.

---

## **14. Conclusión**

Las prácticas realizadas han permitido recorrer todo el ciclo de vida de un servidor FTP/FTPS: desde la instalación de vsftpd hasta la publicación de una página web subida por FTP y servida por Apache.  

Se ha trabajado con distintos directorios raíz (`/srv/ftpgroup-cosmin` y `/var/www/ftpweb-cosmin`), se han probado clientes gráficos y de terminal, se han configurado modos activo y pasivo, y se ha visto en la práctica cómo pequeñas decisiones de configuración (como una umask o un chroot mal ajustado) pueden marcar la diferencia entre un servicio funcional y uno inaccesible.
