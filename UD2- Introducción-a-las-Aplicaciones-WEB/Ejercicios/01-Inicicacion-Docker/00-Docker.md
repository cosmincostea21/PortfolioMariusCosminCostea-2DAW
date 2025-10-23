# 🧠 Informe de Virtualización 

## 1 Instalación de Ubuntu

Captura: pantalla final de la instalación de Ubuntu.
Explicación: muestra que el proceso de instalación del sistema operativo ha finalizado correctamente y la máquina virtual está lista para uso.

---

## 2 Update & Upgrade

Captura: terminal donde se muestra el alias `update` en `.bashrc` y la ejecución de actualización.
Explicación paso a paso:

1. Se añadió un alias `update` en `~/.bashrc` para simplificar la actualización del sistema.
2. Comando sugerido para ejecutar:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
3. Resultado esperado: lista de paquetes comprobada y actualizaciones aplicadas; mensaje final indicando “Todos los paquetes están actualizados” o resumen de paquetes actualizados.

---

## 3 Instalación de dependencias

Captura: instalación de paquetes necesarios antes de Docker.
Explicación paso a paso:

1. Se instalan paquetes como `ca-certificates`, `curl`, `gnupg`, `lsb-release` (u otros requeridos).
2. Objetivo: preparar el sistema para añadir repositorios externos y para la instalación de Docker Desktop o Docker Engine.

---

## 4 Instalación de Docker

Capturas: comandos para añadir el repositorio de Docker, instalación del `.deb` y salida con error de permiso.
Explicación paso a paso:

1. Se añade la clave y el repositorio oficial de Docker (ej. creación de `/etc/apt/sources.list.d/docker.list` con la línea adecuada).
2. Se ejecuta `sudo apt update` y se instala el paquete `.deb` (por ejemplo `docker-desktop-amd64.deb`).
3. Captura muestra salida de instalación con muchos paquetes adicionales (dependencias de virtualización).
4. Error mostrado:

   ```
   permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock
   ```

   Causa: el usuario no tiene permisos para comunicarse con el daemon de Docker.
5. Solución (mostrar en captura o aplicar): añadir el usuario al grupo `docker`:

   ```bash
   sudo usermod -aG docker $USER
   ```

   y luego cerrar sesión y volver a iniciar para aplicar los cambios.

---

## 5 Instalación de imágenes en Docker (búsqueda)

Capturas: salida de `docker search nginx` y `docker search tomcat`.
Explicación paso a paso:

1. `docker search nginx` devuelve la lista de imágenes relacionadas; la oficial aparece como `nginx`.
2. `docker search tomcat` devuelve la lista de imágenes relacionadas; la oficial aparece como `tomcat`.
3. Uso indicado:

   * `nginx`: servidor web / proxy inverso.
   * `tomcat`: servidor de aplicaciones Java (servlets/JSP).

---

## 6 Descargar e iniciar contenedores

Capturas: `docker run hello-world`, `docker run -d -p 8080:80 --name webserver nginx`, `docker run -d -p 8081:8080 --name appserver tomcat`, y progreso de descarga (pull).
Explicación paso a paso:

1. `docker run hello-world` — confirma que Docker puede descargar imágenes y ejecutar contenedores (salida informativa “Hello from Docker!”).
2. `docker run -d -p 8080:80 --name webserver nginx` — Docker descarga `nginx:latest` si no está local y crea un contenedor en segundo plano mapeando el puerto 80 del contenedor al 8080 del anfitrión.
3. `docker run -d -p 8081:8080 --name appserver tomcat` — descarga `tomcat:latest` y crea un contenedor mapeando el puerto 8080 del contenedor al 8081 del anfitrión.
4. Las capturas muestran las capas descargándose y la confirmación “Downloaded newer image for ...”.

---

## 7 Procesos de Docker / Comprobación

Captura: salida de `docker ps` mostrando contenedores en ejecución.
Explicación paso a paso:

1. Ejecutar `docker ps` para listar contenedores activos.
2. Salida típica mostrada en la captura:

   * `CONTAINER ID` `IMAGE` `COMMAND` `STATUS` `PORTS` `NAMES`
   * Ejemplos en la captura:

     * `59656afdc029` — `nginx` — `0.0.0.0:8080->80/tcp` — `webserver`
     * `446da8968e3a` — `tomcat` — `0.0.0.0:8081->8080/tcp` — `appserver`
3. Interpretación: ambos contenedores están “Up” y los puertos están mapeados correctamente.

---

## 8 Prueba en navegador y mensajes HTTP

Capturas: navegador mostrando “Welcome to nginx!” y posible 404 para Tomcat si no hay contenido desplegado.
Explicación paso a paso:

1. Abrir `http://localhost:8080` → debería aparecer la página por defecto de Nginx (“Welcome to nginx!”).
2. Abrir `http://localhost:8081` → debería mostrar la página de inicio de Tomcat; si aparece HTTP 404, puede ser porque no hay una aplicación desplegada en la ruta raíz o la app no está todavía instalada.
3. Mensajes en la captura indican que los servicios responden; si hay 404, comprobar logs del contenedor o despliegue del WAR en Tomcat.

---

## 9 Comandos útiles y soluciones observadas

Capturas: fragmentos con `apt-cache policy docker-ce-cli`, `sudo apt-get install ./docker-desktop-amd64.deb`, `sudo usermod -aG docker $USER`, `sudo apt autoremove`, etc.
Explicación paso a paso:

1. `apt-cache policy docker-ce-cli` — verificar versiones e instalación de paquetes Docker.
2. `sudo apt-get install ./docker-desktop-amd64.deb` — instalar el paquete descargado; revisar dependencias adicionales sugeridas.
3. `sudo apt autoremove` — limpiar paquetes instalados automáticamente que ya no son necesarios.
4. Si se encuentra problema de permisos con `/var/run/docker.sock`, aplicar `sudo usermod -aG docker $USER` y reiniciar sesión.

---

## 🔁 Resumen final de flujo (pasos reproducibles)

1. Instalar Ubuntu en la VM.
2. Actualizar sistema:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
3. Instalar dependencias necesarias para Docker.
4. Añadir repositorio y clave de Docker; instalar Docker Desktop o Docker Engine (`.deb`).
5. Añadir usuario al grupo `docker`:

   ```bash
   sudo usermod -aG docker $USER
   ```

   y cerrar sesión / volver a iniciar.
6. Descargar y ejecutar contenedores:

   ```bash
   docker run hello-world
   docker run -d -p 8080:80 --name webserver nginx
   docker run -d -p 8081:8080 --name appserver tomcat
   ```
7. Comprobar contenedores:

   ```bash
   docker ps
   ```
8. Verificar acceso desde el navegador en `http://localhost:8080` y `http://localhost:8081`.

---

## Notas importantes (aparecen en las capturas)

* Error de permiso con Docker daemon: corregir añadiendo al usuario al grupo `docker`.
* Durante la instalación de Docker Desktop se listan muchas dependencias de virtualización (qemu, libvirt, etc.) — es normal en entornos de escritorio/VM que incluyan soporte para máquinas virtuales y componentes gráficos.

