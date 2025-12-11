# 1. ¿Qué es Tomcat?

**Apache Tomcat** (o, sencillamente, *Tomcat*) es un contenedor de servlets que se puede usar para compilar y ejecutar aplicaciones web realizadas en **Java**.  
Implementa y da soporte tanto a **servlets** como a páginas **JSP (Java Server Pages)** o **Java Sockets**.  
Además, Tomcat es compatible con otras tecnologías del ecosistema Java como **Java Expression Language** y **Java WebSocket**.

---

## 1.1 ¿Qué son los servlets?

Los **servlets** son clases Java que extienden `javax.servlet.http.HttpServlet` y permiten responder a solicitudes **HTTP**.

### Uso típico de los servlets:
- Procesar formularios enviados desde un navegador.
- Generar contenido dinámico (HTML, JSON, XML).
- Manejar sesiones de usuario y lógica de negocio.

---

## 1.2 Fundamentos y componentes básicos de Tomcat

### 🔹 Catalina
Es el contenedor de servlets de Tomcat, encargado de ejecutar aplicaciones web basadas en Servlets y JSP.  
Se configura principalmente en el directorio `conf/` mediante **server.xml**.

### 🔹 Coyote
Es el conector **HTTP/1.1** que recibe las peticiones en un puerto TCP y las envía a Catalina para su procesamiento.  
Se define también en `conf/server.xml`.

### 🔹 Jasper
Motor JSP de Tomcat que compila las páginas JSP en servlets Java.  
Detecta cambios en tiempo de ejecución y recompila automáticamente. Se integra dentro de Catalina.

### 🔹 Manager y Host Manager
Aplicaciones web incluidas en `webapps/`.  
- **Manager**: permite desplegar, parar o eliminar aplicaciones.  
- **Host Manager**: gestiona hosts virtuales y su configuración.

---

### 📂 Estructura básica de directorios

- **bin/** → scripts de arranque y apagado.  
- **conf/** → archivos de configuración (`server.xml`, `web.xml`).  
- **webapps/** → aplicaciones desplegadas.  
- **lib/** → librerías compartidas.  
- **logs/** → registros de ejecución y errores.  

---

## 1.2.1 Flujo interno de funcionamiento

1. **Coyote** recibe la petición.  
2. La petición es enviada al contenedor **Catalina**.  
3. **Catalina** gestiona servlets y JSP.  
4. Se genera la respuesta.  
5. La respuesta se devuelve al cliente.  

---

📌 El despliegue de aplicaciones se realiza en `webapps/` y puede ser administrado con **Manager**.

# 1.2.1 Flujo interno de funcionamiento

1. **Coyote** recibe la petición.  
2. La petición es enviada al contenedor **Catalina**.  
3. **Catalina** gestiona servlets y JSP.  
4. Se genera la respuesta.  
5. Se devuelve al cliente.  

📌 El despliegue de aplicaciones se realiza en `webapps/` y puede ser administrado con **Manager**.

---

# 2. Instalación de Tomcat

Para instalar Tomcat debemos tener **Java** previamente instalado en el equipo.  
La instalación realizada aquí es **manual** y no mediante paquetes de Ubuntu.

### Instalación manual
1. Descargar Tomcat desde la página oficial:  
   👉 [Descargar Tomcat 10](https://tomcat.apache.org/download-10.cgi)  
2. Mover el contenido de la carpeta descomprimida (`apache-tomcat-10.1.50`) a:  
   ```
   /usr/local/tomcat/
   ```
3. Configurar variables de entorno.  
4. Aplicar los cambios:  
   ```
   source ~/.bashrc
   ```

### Instalación mediante paquetes (Ubuntu/Debian)
Lo más común y cómodo es instalar **Tomcat 9**, que usa `javax.servlet.*` y es compatible con la mayoría de tutoriales:

```bash
sudo apt update
sudo apt install tomcat9
```

✅ Instalación completa: nuestro servidor Tomcat estará corriendo en el puerto **8080**.

---

# 3. Archivos clave de configuración

Tomcat se ha instalado en `/usr/local/tomcat/`.  
Para acceder a los archivos de configuración debemos entrar al directorio `conf/`.

### 1. server.xml
- **Función**: Archivo principal de configuración del servidor Tomcat.  
- **Elementos configurables**:
  - Connectors: puertos y protocolos (HTTP, HTTPS, AJP).  
  - Engine/Host: gestión de aplicaciones y dominios virtuales.  
  - Realms: autenticación contra bases de datos o LDAP.  
  - Thread pools: número de hilos para manejar peticiones.  
- **Ejemplo típico**: cambiar el puerto de escucha modificando:  
  ```xml
  <Connector port="8080" ... />
  ```

---

### 2. web.xml
- **Función**: Configuración global de despliegue para todas las aplicaciones web.  
- **Elementos configurables**:
  - Servlets y mappings: qué clases Java responden a qué URLs.  
  - Filtros: lógica que intercepta peticiones/respuestas (ej. seguridad, logging).  
  - Listeners: inicialización de recursos al arrancar la aplicación.  
  - Error pages: páginas personalizadas para errores HTTP.  
  - MIME types: asociaciones de extensiones con tipos de contenido.  

---

### 3. tomcat-users.xml
- **Función**: Define usuarios, contraseñas y roles para acceder a aplicaciones administrativas como **Manager App** o **Host Manager**.  
- **Elementos configurables**:
  - Usuarios:  
    ```xml
    <user username="admin" password="secret" roles="manager-gui"/>
    ```
  - Roles: permisos como `manager-gui`, `admin-gui`, `manager-script`.

---

### 4. context.xml
- **Función**: Configura parámetros de contexto para aplicaciones individuales.  
- **Elementos configurables**:
  - DataSources (JNDI): conexiones a bases de datos.  
  - Session management: persistencia y configuración de sesiones.  
  - Environment entries: variables accesibles desde la aplicación.  
  - Reloading: control de recarga automática de aplicaciones.  

---

# 4. Despliegue de aplicación

1. Descargar un ejercicio de ejemplo desde:  
   👉 [Sample App](https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/?utm_source=copilot.com)  
2. Mover el fichero `.war` a la carpeta `webapps/`.  
3. Abrir el navegador en la dirección:  
   ```
   http://localhost:8080/sample
   ```

