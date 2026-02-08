# Instalación de FileZilla (vsftpd en Ubuntu)

---

## 1. Descarga e instalación

En Ubuntu, el servidor FTP que utilizamos no es FileZilla Server (solo disponible para Windows), sino **vsftpd (Very Secure FTP Daemon)**.  
Es un servidor FTP seguro, ligero y muy utilizado en entornos Linux.

La instalación se realiza mediante:

```
sudo apt install vsftpd
```

vsftpd no tiene interfaz gráfica, por lo que toda la configuración se realiza desde la terminal.

**Salida de instalación (resumen):**

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/02-InstalacionVsftpd/images/01-img.png)

---

## 2. Configurar el puerto de escucha

El archivo de configuración principal se encuentra en:

```
/etc/vsftpd.conf
```

Puedes editarlo con:

```
sudo nano /etc/vsftpd.conf
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/02-InstalacionVsftpd/images/02-img.png)  


En este archivo añadimos o modificamos:

```
listen_port=21
listen=YES
listen_ipv6=NO
anonymous_enable=NO
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/02-InstalacionVsftpd/images/03-img.png)  

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/02-InstalacionVsftpd/images/04-img.png)  

### ¿Por qué usamos IPv4?

Porque la red de la máquina virtual, el host y la mayoría de clientes FTP funcionan por defecto con IPv4.  
Además, vsftpd arranca sin conflictos cuando se fuerza a usar solo IPv4.

### Nota sobre la IP

No configuramos una IP fija porque queremos que vsftpd escuche en cualquier interfaz.  
Si en futuras prácticas se requiere especificar una IP concreta, se añadiría:

```
listen_address=10.0.2.15
```

### Reiniciar el servicio

```
sudo systemctl restart vsftpd
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/02-InstalacionVsftpd/images/05-img.png)  

## 3. Habilitar el inicio automático

vsftpd suele venir habilitado por defecto. Para comprobarlo:

```
sudo systemctl is-enabled vsftpd
```

Para activarlo manualmente:

```
sudo systemctl enable vsftpd
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/02-InstalacionVsftpd/images/06-img.png)  

### Comprobar el estado del servicio

```
sudo systemctl status vsftpd
```

Si todo está correcto, debe aparecer como **active (running)**.


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/02-InstalacionVsftpd/images/07-img.png)  

---

## 4. Cliente FTP utilizado en las prácticas

En las prácticas utilizaremos **FileZilla Client**, no FileZilla Server.

### ¿Por qué FileZilla Client?

- Es multiplataforma (Windows, Linux, macOS).  
- Permite conectarse fácilmente a servidores FTP, FTPS y SFTP.  
- Tiene una interfaz gráfica muy intuitiva.  
- Es ideal para pruebas con vsftpd en Ubuntu.

FileZilla Client será la herramienta principal para conectarnos al servidor vsftpd configurado en la máquina virtual.

---

## 5. Relación entre vsftpd y FileZilla

- **vsftpd** → es el **servidor FTP** que instalamos en Ubuntu.  
- **FileZilla Client** → es el **cliente FTP** que usaremos para conectarnos al servidor.  

Ambos trabajan juntos:

1. vsftpd escucha en el puerto 21.  
2. FileZilla Client se conecta usando IP, usuario y contraseña.  
3. Se realizan transferencias en modo activo o pasivo según la configuración.
