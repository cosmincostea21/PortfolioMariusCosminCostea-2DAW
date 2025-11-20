# 📘 Instalación y Configuración de Apache en Ubuntu

## 1. Introducción
En esta práctica se muestra cómo instalar y configurar el servidor web **Apache2** en Ubuntu.  
El objetivo es comprender los pasos necesarios para poner en marcha el servicio y realizar una configuración básica que permita alojar sitios web de forma correcta.

---

## 2. Actividades realizadas

### 2.1 Instalación del servidor Apache
Se instala Apache con el siguiente comando:

```bash
sudo apt install apache2
```

![Instalación de Apache](ruta-de-la-imagen)

Una vez instalado, se configuran los perfiles de aplicación en **UFW** para permitir el tráfico web:

```bash
sudo ufw app list
sudo ufw allow 'Apache'
```

![Configuración UFW](ruta-de-la-imagen)

Se comprueba el estado del servicio:

```bash
sudo systemctl status apache2
```

![Estado del servicio Apache](ruta-de-la-imagen)

Finalmente, accedemos a la IP del servidor para verificar la página por defecto de Apache.  

![Página por defecto Apache](ruta-de-la-imagen)

---

### 2.2 Configuración del servidor Apache
El archivo principal de configuración es:

```
/etc/apache2/apache2.conf
```

![Archivo apache2.conf](ruta-de-la-imagen)

Los puertos se definen en:

```
/etc/apache2/ports.conf
```

![Archivo ports.conf](ruta-de-la-imagen)

---

## 3. Creación de un sitio web propio

Se crea un directorio para el nuevo sitio:

```bash
sudo mkdir /var/www/gci
```

Se gestionan permisos mediante un grupo de desarrolladores:

```bash
sudo groupadd webdev
sudo usermod -aG webdev cosmin
sudo chown -R root:webdev /var/www/gci
sudo chmod -R 775 /var/www/gci
sudo chmod g+s /var/www/gci
```

![Permisos en directorio web](ruta-de-la-imagen)

Se crea el archivo `index.html`:

```html
<html>
<head>
<title>Ubuntu es lo mejor !!</title>
</head>
<body>
<p>Estoy corriendo esto en un servidor linux ubuntu 24.4</p>
</body>
</html>
```

![Archivo index.html](ruta-de-la-imagen)

---

## 4. Configuración de Virtual Host

Se copia el archivo de configuración por defecto y se adapta:

```bash
cd /etc/apache2/sites-available/
sudo cp 000-default.conf gci.conf
```

En `gci.conf` se define:

```apache
<VirtualHost *:80>
    ServerAdmin cosmincostea0810@gmail.com
    DocumentRoot /var/www/gci
    ServerName gci.ejemploCosmin.com
</VirtualHost>
```

![Archivo gci.conf](ruta-de-la-imagen)

Se activa el sitio y se recarga Apache:

```bash
sudo a2ensite gci.conf
sudo systemctl reload apache2
```

![Activación VirtualHost](ruta-de-la-imagen)

Se añade el dominio al archivo `/etc/hosts`:

```
127.0.0.1 gci.ejemploCosmin.com
```

![Archivo hosts](ruta-de-la-imagen)

Al acceder al dominio configurado, se muestra la página personalizada:  

![Página personalizada](ruta-de-la-imagen)

---

## 5. Resultados y valoración
- Se logró instalar y configurar Apache correctamente.  
- Se creó un sitio web propio con permisos adecuados.  
- Se configuró un VirtualHost y se asoció un dominio local.  
- Se reforzaron conocimientos sobre comandos como `chown`, `chmod` y la gestión de grupos en Linux.

---

## 6. Conclusión
La instalación y configuración de Apache en Ubuntu permite comprender de manera práctica el funcionamiento de un servidor web.  
Crear un **VirtualHost** y alojar una página propia en un entorno controlado es un paso fundamental para el desarrollo web profesional.

---

## 7. Bibliografía
- [IONOS: Instalar Apache en Ubuntu](https://www.ionos.es/digitalguide/servidores/configuracion/instalar-apache-en-ubuntu/)  
- Install and Configure Apache | Ubuntu  
- Recomendaciones de ChatGPT  

---

¿Quieres que además te prepare una **versión con índice automático (Table of Contents)** para que sea aún más navegable?
