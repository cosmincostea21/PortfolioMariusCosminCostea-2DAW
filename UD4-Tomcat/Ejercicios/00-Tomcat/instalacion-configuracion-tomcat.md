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
