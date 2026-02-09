# Práctica: Uso de clientes FTP gráficos con el servidor ftp‑cosmin

Esta práctica consiste en conectarse al servidor FTP que ya tenemos configurado, guardar la conexión en el cliente, realizar transferencias en ambos sentidos y observar los mensajes de estado que confirman que la operación ha sido correcta.

---

## 1. Datos de conexión del servidor ftp‑cosmin

En el gestor de sitios de FileZilla configuramos nuestro servidor de la siguente manera:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/07-Prueba-Cliente-Graficamente/images/01-img.png)

Servidor: localhost  
Puerto: 21  
Protocolo: FTP  
Cifrado: FTP simple (sin TLS)  
Usuario: cosmin  
Contraseña:
Directorio remoto inicial: / (que corresponde a /srv/ftpgroup-cosmin dentro del chroot)  
Carpeta de subida: upload  

Posteriormente pulsamos en conectar y nos aparecerá la ventana que nos pedirá la contraseña del usuario:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/07-Prueba-Cliente-Graficamente/images/02-img.png)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/07-Prueba-Cliente-Graficamente/images/03-img.png)


---

## 2. Resumen de la configuración del servidor

El servidor vsftpd está configurado para permitir acceso a usuarios locales, permitir escritura, encerrar al usuario en un chroot y usar /srv/ftpgroup-cosmin como raíz remota.

Los permisos necesarios en el sistema son:

- El directorio raíz del chroot no debe ser escribible:
  sudo chmod 755 /srv/ftpgroup-cosmin

- La carpeta donde se subirán archivos debe ser escribible por el usuario:
  sudo mkdir -p /srv/ftpgroup-cosmin/upload  
  sudo chown cosmin:cosmin /srv/ftpgroup-cosmin/upload

Con esto, el usuario puede subir archivos únicamente dentro de upload.

---

## 3. Transferencia bidireccional de archivos

### Subir un archivo (local → remoto)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/07-Prueba-Cliente-Graficamente/images/05-img.png)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/07-Prueba-Cliente-Graficamente/images/06-img.png)


1. En el panel izquierdo, navegar al Escritorio.  
2. En el panel derecho, entrar en upload.  
3. Arrastrar un archivo desde el panel izquierdo al derecho.  
4. En la ventana de estado deben aparecer mensajes como:  
   - 150 Ok to send data  
   - 226 Transfer complete  
5. El archivo aparecerá en “Transferencias satisfactorias”.

### Descargar un archivo (remoto → local)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/07-Prueba-Cliente-Graficamente/images/08-img.png)


1. En el panel derecho, seleccionar un archivo dentro de upload.  
2. En el panel izquierdo, elegir la carpeta local donde guardarlo.  
3. Arrastrar el archivo del panel derecho al izquierdo.  
4. Verificar que aparece en “Transferencias satisfactorias”.

---

## 5. Mensajes de estado importantes

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/07-Prueba-Cliente-Graficamente/images/07-img.png)


Estos mensajes confirman que la operación se realizó correctamente.

---
