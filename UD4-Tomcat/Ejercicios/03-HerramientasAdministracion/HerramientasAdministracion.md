# 🛠️ Herramientas de Administración

## 1. Manager y Host Manager

Tomcat incluye dos herramientas web pensadas para administrar el servidor sin tocar archivos manualmente. **Tomcat Manager** se centra en gestionar aplicaciones: desplegarlas, iniciarlas, detenerlas o ver su estado. **Tomcat Host Manager**, en cambio, permite crear y administrar virtual hosts, es decir, distintos dominios o sitios dentro del mismo servidor.  
Juntos ofrecen una forma rápida y centralizada de controlar tanto las aplicaciones como la estructura de hosts del entorno Tomcat.

---

## 🌐 1.1 Manager

Tomcat Manager sirve para administrar aplicaciones web en un servidor Tomcat sin reiniciarlo, permitiendo tareas como desplegar, detener, iniciar o eliminar aplicaciones, además de consultar el estado del servidor.

### Funciones principales del Manager

- **[gestionar aplicaciones desplegadas](guide://action?prefill=Tell%20me%20more%20about%3A%20gestionar%20aplicaciones%20desplegadas)**: iniciar, parar, recargar o eliminar apps  
- **[desplegar nuevas aplicaciones](guide://action?prefill=Tell%20me%20more%20about%3A%20desplegar%20nuevas%20aplicaciones)**: subir WAR o desplegar por URL  
- **[ver información del servidor](guide://action?prefill=Tell%20me%20more%20about%3A%20ver%20informaci%C3%B3n%20del%20servidor)**: estado, sesiones, recursos, diagnósticos  

---

### 🔎 1.1.1 ¿Qué es?

El Manager App de Tomcat es una aplicación web incluida en Tomcat que permite administrar aplicaciones web desplegadas en el servidor: instalarlas, actualizarlas, reiniciarlas, detenerlas o eliminarlas.

Accedemos a través de la siguiente dirección URL:

```
http://localhost:8080/manager/html
```

---

## ⚙️ 2. Funciones principales

### 📦 Despliegue de aplicaciones (Deploy)

Permite subir y desplegar aplicaciones web en formato `.war` o desde un directorio. Incluye opciones para desplegar desde:

- **[un archivo WAR local](guide://action?prefill=Tell%20me%20more%20about%3A%20un%20archivo%20WAR%20local)**
- **[una URL](guide://action?prefill=Tell%20me%20more%20about%3A%20una%20URL)**
- **[un directorio ya existente](guide://action?prefill=Tell%20me%20more%20about%3A%20un%20directorio%20ya%20existente)**

### 🔄 Recarga de aplicaciones (Reload)

Recarga una aplicación sin necesidad de reiniciar todo el servidor. Se usa cuando se han modificado clases o archivos de configuración.

### ▶️⏹️ Parada y arranque de aplicaciones (Stop / Start)

Permite detener una aplicación temporalmente o volver a iniciarla. Muy útil para mantenimiento o pruebas.

### 🗑️ Eliminación de aplicaciones (Undeploy)

Elimina completamente una aplicación del servidor.

---

## 🔐 3. Certificados e información del servidor

Apartados donde podemos ver posibles fallas, certificados de seguridad e información variada de nuestro servidor como:

- **[versión del sistema operativo](guide://action?prefill=Tell%20me%20more%20about%3A%20versi%C3%B3n%20del%20sistema%20operativo)**
- **[arquitectura del sistema](guide://action?prefill=Tell%20me%20more%20about%3A%20arquitectura%20del%20sistema)**
- **[dirección IP](guide://action?prefill=Tell%20me%20more%20about%3A%20direcci%C3%B3n%20IP)**

---

# 🏠 1.2 Host Manager

Tomcat Host Manager se usa para crear, eliminar y administrar virtual hosts dentro de un servidor Tomcat, sin necesidad de editar archivos XML manualmente.

Funciones principales:

- **[crear nuevos virtual hosts](guide://action?prefill=Tell%20me%20more%20about%3A%20crear%20nuevos%20virtual%20hosts)**: añadir dominios o subdominios al servidor  
- **[eliminar hosts existentes](guide://action?prefill=Tell%20me%20more%20about%3A%20eliminar%20hosts%20existentes)**: limpiar configuraciones que ya no se usan  
- **[iniciar o detener hosts](guide://action?prefill=Tell%20me%20more%20about%3A%20iniciar%20o%20detener%20hosts)**: controlar su disponibilidad  
- **[persistir cambios en la configuración](guide://action?prefill=Tell%20me%20more%20about%3A%20persistir%20cambios%20en%20la%20configuraci%C3%B3n)**: guardar ajustes en los archivos de Tomcat  

Accedemos a través de:

```
https://localhost:8443/host-manager/html
```

*(Estás utilizando otro puerto porque ahí tienes el certificado SSL.)*

---

## 🏗️ 1.2.1 Creación de hosts virtuales

Permite añadir un nuevo host virtual indicando:

- **[Nombre del host](guide://action?prefill=Tell%20me%20more%20about%3A%20Nombre%20del%20host)** (por ejemplo, midominio.com)  
- **[Directorio base](guide://action?prefill=Tell%20me%20more%20about%3A%20Directorio%20base)** donde se almacenarán las aplicaciones (appBase)  
- Atributos opcionales como:
  - **[autoDeploy](guide://action?prefill=Tell%20me%20more%20about%3A%20autoDeploy)**
  - **[deployOnStartup](guide://action?prefill=Tell%20me%20more%20about%3A%20deployOnStartup)**
  - **[unpackWARs](guide://action?prefill=Tell%20me%20more%20about%3A%20unpackWARs)**

El Host Manager genera automáticamente la estructura de directorios necesaria y registra el nuevo host en la configuración interna de Tomcat.

**Uso típico:** alojar varios proyectos o sitios web en una misma instancia de Tomcat.

---

## 🖥️ 1.2.2 Nombre de las máquinas virtuales

*(Sección mencionada en tu texto, la mantengo tal cual para que puedas completarla si lo deseas.)*

---

## 💬 1.2.3 Interfaz de texto

Permite ejecutar comandos mediante peticiones HTTP.  
Para que estos comandos sean posibles debemos añadir un rol a nuestro usuario admin: **admin-script**.

Creamos el rol y se lo añadimos de la siguiente manera:

```
<role rolename="admin-script"/>
<user username="admin" password="admin123" roles="admin-script"/>
```

### Comandos principales

- **[list](guide://action?prefill=Tell%20me%20more%20about%3A%20list)**  
- **[add](guide://action?prefill=Tell%20me%20more%20about%3A%20add)**  
- **[remove](guide://action?prefill=Tell%20me%20more%20about%3A%20remove)**  
- **[start](guide://action?prefill=Tell%20me%20more%20about%3A%20start)**  
- **[stop](guide://action?prefill=Tell%20me%20more%20about%3A%20stop)**  
- **[persist](guide://action?prefill=Tell%20me%20more%20about%3A%20persist)**  

### Ejemplo para añadir un host

```
https://localhost:8443/host-manager/text/add?name=midominio.com&appBase=webapps_midominio
```

Esta interfaz es útil para automatización, scripts o integración con herramientas DevOps.

---

## 💾 1.2.4 Persistencia de la configuración (Persist)

Los cambios realizados desde la interfaz web **no se guardan automáticamente** en `server.xml`.

La función Persist permite:

- **[Guardar los hosts creados o modificados](guide://action?prefill=Tell%20me%20more%20about%3A%20Guardar%20los%20hosts%20creados%20o%20modificados)**
- **[Escribir los cambios en los archivos de configuración](guide://action?prefill=Tell%20me%20more%20about%3A%20Escribir%20los%20cambios%20en%20los%20archivos%20de%20configuraci%C3%B3n)**

Sin persistencia, los cambios se perderían al reiniciar el servidor.

---

## 🧭 1.2.5 Información del servidor

*(Sección abierta para que añadas capturas o detalles si lo deseas.)*
