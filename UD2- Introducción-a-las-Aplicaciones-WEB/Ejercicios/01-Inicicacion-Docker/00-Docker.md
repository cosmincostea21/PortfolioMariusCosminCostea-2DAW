¡Perfecto! Aquí tienes tu documento actualizado en formato Markdown, con las tres imágenes adicionales insertadas en orden bajo el nombre `xx-img.png`. He ubicado cada una en el punto correspondiente del flujo de instalación, según el orden lógico del proceso:

---

# 🐳 Instalación de Docker en Ubuntu paso a paso

Este documento explica cómo instalar Docker en Ubuntu utilizando VirtualBox. Cada paso está acompañado de capturas de pantalla y explicaciones pensadas para estudiantes de Desarrollo de Aplicaciones Web.

---

## 1️⃣ Instalación de Ubuntu en VirtualBox

Se ha instalado Ubuntu Desktop en una máquina virtual usando Oracle VirtualBox. Esta configuración permite trabajar en un entorno seguro y aislado para pruebas y desarrollo.

![img1](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/00-img.png)

📸 *Captura*: Se muestra el escritorio de Ubuntu corriendo en VirtualBox.

---

## 2️⃣ Actualización del sistema

Se ha creado un alias llamado `update` en el archivo `.bashrc` para simplificar la ejecución de los comandos `sudo apt update && sudo apt upgrade`.

![img2](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/01-img.png)

📸 *Captura*: Se ejecuta el alias `update` y se observa cómo el sistema verifica e instala actualizaciones. Al final, se recomienda usar `sudo apt autoremove` para limpiar paquetes innecesarios.

🔍 **Importante**: Mantener el sistema actualizado garantiza seguridad y compatibilidad con nuevas versiones de software.

---

## 3️⃣ Instalación de dependencias

Antes de instalar Docker, se añaden los repositorios oficiales y se actualiza la lista de paquetes.

![img3](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/02-img.png)

📸 *Captura*: Se muestra cómo se añade el repositorio de Docker usando `echo` y `tee`, seguido de `sudo apt update`.

💡 **Nota técnica**: Aunque el sistema es Ubuntu 24.04 (codename: noble), el repositorio añadido corresponde a "jammy", lo cual puede causar incompatibilidades. Se recomienda verificar que el repositorio coincida con la versión del sistema.

---

## 4️⃣ Instalación de Docker Desktop

Se instala Docker Desktop desde un archivo `.deb` descargado previamente.

![xx-img1](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/xx-img.png)

📸 *Captura*: Se ejecuta `sudo apt-get install ./docker-desktop-amd64.deb`, lo cual instala Docker junto con varias dependencias como `qemu`, `libboost`, y herramientas de virtualización.

📛 *Error común*: Al ejecutar `docker version`, aparece un error de permisos. Esto ocurre porque el usuario no pertenece al grupo `docker`.

---

## ✅ Solución al error de permisos

Se añade el usuario al grupo `docker` con el comando:

```bash
sudo usermod -aG docker $USER
```

Luego se cierra sesión y se vuelve a iniciar para aplicar los cambios.

![xx-img2](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/xx-img.png)

📸 *Captura*: Se ejecuta `docker run hello-world` y se confirma que Docker funciona correctamente.

---

## 📦 Búsqueda de imágenes en Docker Hub

Se utilizan los comandos `docker search nginx` y `docker search tomcat` para buscar imágenes oficiales.

📸 *Captura*: Se muestran resultados con estrellas y etiquetas `[OK]` que indican imágenes oficiales.

| Comando                | Qué busca                          | Imagen oficial | Uso principal                                |
|------------------------|------------------------------------|----------------|----------------------------------------------|
| `docker search nginx`  | Imágenes relacionadas con Nginx    | nginx          | Servidor web y proxy inverso                 |
| `docker search tomcat` | Imágenes relacionadas con Tomcat   | tomcat         | Servidor de aplicaciones Java (servlets, JSP)|

---

## 🚀 Ejecución de contenedores

Se descargan y ejecutan dos contenedores:

```bash
docker run -d -p 8080:80 --name webserver nginx
docker run -d -p 8081:8080 --name appserver tomcat
```

![xx-img3](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD2-%20Introducci%C3%B3n-a-las-Aplicaciones-WEB/Ejercicios/01-Inicicacion-Docker/imagenes/xx-img.png)

📸 *Captura*: Se observa cómo Docker descarga las imágenes y crea los contenedores.

---

## 🔍 Verificación de procesos activos

Se usa `docker ps` para listar los contenedores en ejecución.

📸 *Captura*: Se muestran los contenedores `webserver` (nginx) y `appserver` (tomcat) con sus respectivos puertos expuestos.

---

## 🌐 Prueba en navegador

Se accede a los contenedores desde el navegador:

- `http://localhost:8080` muestra la página de bienvenida de Nginx.
- `http://localhost:8081` muestra un error 404 de Tomcat, lo cual es normal si no se ha desplegado ninguna aplicación.

---

## 🧰 Requisitos mínimos para desplegar una aplicación web

### 🖥️ Hardware y software

- CPU con soporte de virtualización
- 4 GB de RAM mínimo
- Ubuntu 24.04 LTS
- Docker Desktop

### 🌐 Infraestructura de red

- Conexión a internet para descargar imágenes
- Puertos abiertos para acceso a contenedores

### ⚙️ Configuración del servidor

- Contenedores configurados con puertos expuestos
- Imágenes oficiales y actualizadas

### 🔐 Seguridad y mantenimiento

- Actualizaciones periódicas (`apt update`, `docker pull`)
- Uso de imágenes verificadas
- Gestión de usuarios y permisos en Docker

---

¿Quieres que te lo convierta en una presentación o guía imprimible para entregar? También puedo ayudarte a crear una portada o índice si lo necesitas.
