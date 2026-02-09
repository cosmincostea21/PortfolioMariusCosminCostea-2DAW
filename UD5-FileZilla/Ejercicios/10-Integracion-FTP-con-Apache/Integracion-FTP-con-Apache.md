# Integración FTP con servidor WEB

## 1. Creación del directorio del sitio web

El sitio web del usuario *cosmin* se alojará en:

```
/var/www/ftpweb-cosmin
```

Creación del directorio:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/01-img.png)


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/02-img.png)


```
sudo mkdir -p /var/www/ftpweb-cosmin
sudo chown -R cosmin:cosmin /var/www/ftpweb-cosmin
sudo chmod -R 755 /var/www/ftpweb-cosmin
```

Con esto:

- El directorio existe
- Pertenece al usuario local que usará FTP
- Apache puede leerlo

---

## 2. Configuración del VirtualHost en Apache

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/03-img.png)


Archivo:

```
/etc/apache2/sites-available/ftpweb.conf
```

Contenido:

```
<VirtualHost *:80>
    ServerName ftpweb.local
    DocumentRoot /var/www/ftpweb-cosmin

    <Directory /var/www/ftpweb-cosmin>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

Activación:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/04-img.png)


```
sudo a2ensite ftpweb.conf
sudo systemctl reload apache2
```

Entrada en `/etc/hosts`:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/05-img.png)



```
127.0.0.1 ftpweb.local
```

---

## 3. Configuración de vsftpd con FTPS y directorio raíz personalizado

Archivo:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/06-img.png)


```
/etc/vsftpd.conf
```

Configuración:

```
listen_port=21
listen=YES

pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100

listen_ipv6=NO

anonymous_enable=NO

local_enable=YES
write_enable=YES

dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES

chroot_local_user=YES
allow_writeable_chroot=YES
local_root=/var/www/ftpweb-cosmin

local_umask=022

max_clients=5
max_per_ip=2

secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd

ssl_enable=YES
rsa_cert_file=/etc/ssl/certs/vsftpd.crt
rsa_private_key_file=/etc/ssl/private/vsftpd.key

force_local_logins_ssl=YES
force_local_data_ssl=YES

ssl_sslv2=NO
ssl_sslv3=NO
ssl_tlsv1=YES

require_ssl_reuse=NO
ssl_ciphers=HIGH
local_umask=022

```

Reinicio:

```
sudo systemctl restart vsftpd
```

---

## 4. Certificado TLS para FTPS

Aprovechamos los certificados creados en la anterior práctica

---

## 5. Configuración de FileZilla

En el Gestor de sitios:

- **Servidor:** localhost  
- **Puerto:** 21  
- **Cifrado:** Requiere FTP explícito sobre TLS  
- **Usuario:** cosmin  
- **Contraseña:** la del sistema  

Al conectar, vsftpd te enjaula en:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/07-img.png)


```
/var/www/ftpweb-cosmin
```

---

## 6. Subida de archivos (PUT) y permisos automáticos

Gracias a:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/12-img.png)


```
local_umask=022
```

vsftpd crea los archivos con permisos:

```
-rw-r--r--   (644)
```

Esto permite que Apache los sirva sin errores 403.

Proceso:

1. Crear `index.html` en tu PC  
2. Abrir FileZilla  
3. Arrastrar `index.html` al panel remoto  
4. Verificar que aparece con permisos 644  
5. Acceder desde navegador:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/10-img.png)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/11-img.png)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/10-Integracion-FTP-con-Apache/images/13-img.png)


```
http://ftpweb.local/index.html
```

---

## 7. Comprobación final

- FTPS funciona  
- El usuario cosmin está enjaulado en su DocumentRoot  
- Los archivos subidos tienen permisos correctos  
- Apache sirve la web sin errores  
