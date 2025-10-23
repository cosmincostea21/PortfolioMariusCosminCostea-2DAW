
# 🚀 Instalación y Configuración de Docker en Ubuntu

## Introducción  
En este documento se explica paso a paso cómo instalar **Ubuntu en VirtualBox**, actualizar el sistema, instalar **Docker** y desplegar contenedores de prueba como **Nginx** y **Tomcat**.  
El objetivo es que cualquier persona, incluso con conocimientos básicos, pueda seguir el proceso y comprender no solo los comandos, sino también **qué significan y por qué son necesarios**.  

---

## 1. Instalación de Ubuntu  
El primer paso consiste en instalar Ubuntu dentro de **Oracle VirtualBox**, lo que nos permite crear un entorno virtualizado para trabajar sin afectar directamente al sistema operativo principal.  
Esto es útil porque:  
- Podemos experimentar sin riesgo.  
- Es posible crear múltiples entornos de prueba.  
- Se facilita la portabilidad del proyecto.  

![Instalación finalizada](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/00-img.png)

---

## 2. Update & Upgrade  
Una vez instalado Ubuntu, es fundamental **actualizar los paquetes del sistema**. Esto asegura que contamos con las últimas versiones de seguridad y correcciones de errores.  

- `sudo apt update` → Actualiza la lista de paquetes disponibles.  
- `sudo apt upgrade` → Instala las versiones más recientes de los paquetes ya instalados.  

En este caso, se creó un **alias** llamado `update` en el archivo `.bashrc` para simplificar el proceso.  

![Update y Upgrade](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/01-img.png)

---

## 3. Instalación de dependencias  
Antes de instalar Docker, es necesario añadir los **repositorios oficiales** y las **claves GPG** que garantizan la autenticidad de los paquetes.  
Esto se hace porque la versión de Docker incluida en los repositorios de Ubuntu puede no estar actualizada.  

Pasos principales:  
1. Añadir la clave GPG de Docker.  
2. Configurar el repositorio oficial de Docker.  
3. Actualizar la lista de paquetes (`sudo apt update`).  

![Dependencias Docker](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/02-img.png)

---

## 4. Instalación de Docker  
Con las dependencias listas, instalamos Docker Desktop descargando el paquete `.deb`.  

- Durante la instalación, se añaden librerías necesarias como `qemu`, `libvirt`, `uidmap`, entre otras.  
- Al ejecutar `docker version` por primera vez, aparece un error de permisos: el usuario no tiene acceso al **socket de Docker**.  

![Instalación Docker](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/03-img.png)

![Error permisos Docker](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/04-img.png)

Para solucionarlo:  
```bash
sudo usermod -aG docker $USER
```
![AñadirAlGrupo](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/05-img.png)


Esto añade al usuario al grupo `docker`, permitiendo ejecutar comandos sin `sudo`. Tras reiniciar sesión, probamos con:  

```bash
docker run hello-world
```

![Docker Hello World](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/06-img.png)

---

## 5. Búsqueda de imágenes en Docker Hub  
Docker Hub es el repositorio oficial de imágenes. Podemos buscar imágenes con:  

```bash
docker search nginx
docker search tomcat
```

- **Nginx** → Servidor web ligero y rápido, usado como proxy inverso.  
- **Tomcat** → Servidor de aplicaciones Java para ejecutar servlets y JSP.  

![Búsqueda Nginx](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/07-img.png)

![Búsqueda Tomcat](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/08-img.png)

---

## 6. Descarga e inicio de contenedores  
Ejecutamos un contenedor de **Nginx** en el puerto `8080`:  

```bash
docker run -d -p 8080:80 --name webserver nginx
```

![Contenedor Nginx](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/09-img.png)

Ejecutamos un contenedor de **Tomcat** en el puerto `8081`:  

```bash
docker run -d -p 8081:8080 --name appserver tomcat
```

![Contenedor Tomcat](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/10-img.png)

---

## 7. Procesos activos en Docker  
Con el comando `docker ps` podemos ver los contenedores en ejecución, sus puertos y nombres asignados.  

![Procesos Docker](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/11-img.png)

Accedemos desde el navegador:  
- **http://localhost:8080** → Página de bienvenida de Nginx.  
- **http://localhost:8081** → Página de Tomcat (inicialmente muestra un error 404 porque no hay aplicaciones desplegadas).  

![Página Nginx](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/12-img.png)

![Página Tomcat](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/13-img.png)

---

## Conclusión  
Hemos completado con éxito la instalación de **Ubuntu en VirtualBox**, la configuración de **Docker** y la ejecución de contenedores básicos.  
Este proceso nos enseña varios puntos clave:  

- La importancia de mantener el sistema actualizado.  
- Cómo añadir repositorios externos y claves GPG para instalar software seguro.  
- El uso de Docker para desplegar aplicaciones de forma rápida y aislada.  
- La diferencia entre un **servidor web (Nginx)** y un **servidor de aplicaciones (Tomcat)**.  

Con esta base, ya es posible **desplegar aplicaciones web reales** dentro de contenedores, escalar servicios y experimentar con arquitecturas modernas basadas en microservicios.  

---
