# **Documentación Final del Entorno Tomcat10**

---

## **1. Introducción general**

Este documento recoge el proceso completo de instalación, configuración y despliegue de un entorno basado en **Apache Tomcat 10**, incluyendo su integración con **Apache2 como reverse proxy**, la configuración de seguridad (incluyendo certificados SSL y keystores), pruebas de rendimiento y un despliegue alternativo mediante **Docker**.  
El objetivo es comprender no solo *cómo* se ha configurado el entorno, sino también *por qué* se hace de esta manera y qué ventajas aporta cada componente en un entorno real.

---

## **2. Arquitectura básica de Tomcat10**

Apache Tomcat10 es un **servidor de aplicaciones Java** diseñado específicamente para ejecutar aplicaciones basadas en **servlets**, **JSP** y tecnologías Jakarta EE. Esto significa que cualquier aplicación que utilice clases que extienden `HttpServlet` o páginas JSP necesita un contenedor como Tomcat para funcionar.

Su arquitectura se compone de varios módulos:

- **Catalina**, el contenedor de servlets que gestiona el ciclo de vida de las aplicaciones.
- **Coyote**, el conector HTTP que recibe las peticiones del cliente.
- **Jasper**, el motor encargado de compilar JSP en servlets Java.

El flujo interno es:

1. Coyote recibe la petición HTTP.  
2. Catalina la procesa.  
3. Jasper interviene si hay JSP.  
4. Se genera la respuesta.  
5. La respuesta vuelve al cliente.

## **3. Instalación de Tomcat10 mediante paquetes APT**

A diferencia de una instalación manual descargando Tomcat desde la web oficial, en este caso se optó por una instalación **mediante los paquetes oficiales de Ubuntu**, lo que garantiza:

- Integración con systemd  
- Actualizaciones automáticas  
- Estructura de directorios estándar  
- Mayor estabilidad en entornos de práctica o producción

### **3.1 Instalación**

```bash
sudo apt update
sudo apt install tomcat10
```

### **3.2 Comprobación del servicio**

```bash
sudo systemctl status tomcat10
```

### **3.3 Estructura de directorios de Tomcat10 instalado vía APT**

```
/etc/tomcat10/          → Archivos de configuración
/usr/share/tomcat10/    → Binarios del servidor
/var/lib/tomcat10/      → Aplicaciones desplegadas (webapps)
/var/log/tomcat10/      → Logs
/var/cache/tomcat10/    → Clases compiladas (work)
```

---

## **4. Configuración del servidor Tomcat10**

Los archivos más relevantes se encuentran en `/etc/tomcat10/`.

### **4.1 server.xml**

Define conectores, puertos, hilos y parámetros de rendimiento.

```xml
<Connector port="8080" protocol="HTTP/1.1"
    connectionTimeout="20000"
    redirectPort="8443"
    maxThreads="300"
    minSpareThreads="50"
    maxConnections="10000"
    acceptCount="200" />
```

### **4.2 tomcat-users.xml**

Define usuarios y roles para acceder a Manager y Host Manager.

```xml
<user username="admin" password="admin123"
      roles="manager-gui,admin-gui,admin-script" />
```

### **4.3 context.xml**

Controla recursos por aplicación, recarga automática y parámetros de entorno.

---

## **5. Apache2 como reverse proxy: explicación clara**

### **5.1 ¿Qué es Apache2 y por qué es un buen proxy?**

Apache2 es un servidor web maduro, estable y optimizado para servir contenido estático, gestionar certificados SSL y manejar miles de conexiones simultáneas.  
Cuando se usa como **reverse proxy**, Apache recibe las peticiones del cliente y las reenvía a otro servidor interno (Tomcat).

Apache es un buen proxy porque:

- **Protege** a Tomcat del acceso directo desde Internet.  
- **Optimiza** el rendimiento sirviendo contenido estático más rápido que Tomcat.  
- **Simplifica** la gestión de HTTPS.  
- **Permite escalar** hacia varios Tomcat (balanceo de carga).  
- **Aísla** la aplicación Java del tráfico directo del cliente.

---

## **6. Configuración del reverse proxy Apache2 → Tomcat10**

### **6.1 Activación de módulos necesarios**

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### **6.2 ¿Qué es un Virtual Host?**

Un **Virtual Host** permite que Apache sirva diferentes sitios web o servicios dependiendo del dominio o puerto solicitado.  
En este caso, se creó un Virtual Host que escucha en el puerto 80 y reenvía todas las peticiones hacia Tomcat10 en el puerto 8080.

### **6.3 Configuración del Virtual Host**

Archivo: `/etc/apache2/sites-available/tomcat10-proxy.conf`

```apache
<VirtualHost *:80>
    ServerName localhost
    ProxyPreserveHost On
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/
</VirtualHost>
```

Activación:

```bash
sudo a2ensite tomcat10-proxy
sudo systemctl reload apache2
```

---

## **7. Seguridad aplicada en Tomcat y SSL**

La seguridad en este entorno se ha trabajado en dos niveles: **control de acceso a Tomcat** y **cifrado de las comunicaciones mediante SSL**.

### **7.1 Control de acceso a herramientas administrativas**

Tomcat incluye dos aplicaciones sensibles:

- **Manager App**  
- **Host Manager**

Ambas permiten acciones críticas como desplegar aplicaciones, reiniciarlas o crear hosts virtuales.  
Por defecto vienen deshabilitadas por seguridad y requieren usuarios con roles específicos definidos en `tomcat-users.xml`.

Roles importantes:

- `manager-gui` → acceso a Manager App  
- `admin-gui` → acceso a Host Manager  
- `admin-script` → automatización vía HTTP

### **7.2 Certificados SSL y keystore**

En el entorno configurado también se ha tenido en cuenta el uso de **certificados SSL**, necesarios para habilitar conexiones HTTPS seguras.  
Estos certificados suelen almacenarse en un **keystore**, que es simplemente un archivo protegido donde se guardan claves y certificados.  
En este caso, la gestión del certificado se realiza desde **Apache2**, que actúa como frontal HTTPS, mientras que Tomcat recibe el tráfico ya descifrado a través del proxy.  
Este enfoque es habitual en producción, ya que Apache se encarga de la parte criptográfica y Tomcat queda protegido detrás de él.

### **7.3 Persistencia de configuración**

Los cambios hechos desde la interfaz web (Manager / Host Manager) no se guardan automáticamente en los XML.  
Debe usarse la opción **Persist**.

### **7.4 Ocultación de Tomcat detrás de Apache**

Esto añade seguridad porque:

- Tomcat no expone su puerto 8080 al exterior.  
- Apache filtra y controla el tráfico.  
- Se evita que un atacante acceda directamente al contenedor de servlets.

---

## **8. Pruebas de rendimiento (resumen)**

Se realizaron pruebas con **ApacheBench** para evaluar el rendimiento del servidor.

### **Prueba 1: carga baja**

```
ab -n 100 -c 10 http://localhost:8080/sample/
```

- ~1600 peticiones/segundo  
- Latencia muy baja  
- 0 errores  

### **Prueba 2: carga media**

```
ab -n 2000 -c 50 http://localhost:8080/sample/
```

- ~2800 peticiones/segundo  
- Latencia mayor pero estable  
- 0 errores  

**Conclusión:** Tomcat10 escala bien y mantiene estabilidad incluso con concurrencia elevada.

---

## **9. Despliegue en contenedores Docker**

### **9.1 Descarga de la imagen**

```bash
sudo docker pull tomcat:latest
```

### **9.2 Lanzamiento del contenedor**

```bash
sudo docker run -d --name tomcat-demo -p 8080:8080 tomcat:latest
```

### **9.3 Copia de la aplicación**

```bash
sudo docker cp sample.war tomcat-demo:/usr/local/tomcat/webapps/
```

---

## **10. Conclusión**

Estas prácticas me han permitido entender de forma clara cómo se despliega y gestiona una aplicación Java en un entorno real. Trabajar con Tomcat10 me ha mostrado por qué sigue siendo un servidor tan utilizado hoy en día: es estable, sencillo de administrar y está pensado específicamente para ejecutar aplicaciones basadas en servlets, que siguen siendo muy comunes en empresas y sistemas internos.

La integración con Apache2 me ha ayudado a comprender cómo se construyen arquitecturas web profesionales, donde un servidor frontal actúa como capa de seguridad y optimización antes de que el tráfico llegue a la aplicación. Ver cómo Apache protege, filtra y mejora el rendimiento, y cómo puede gestionar certificados SSL y keystores, me ha hecho entender su importancia en producción.

Las pruebas de rendimiento y el despliegue en Docker completan la visión moderna del proceso: comprobar cómo responde el servidor bajo carga y cómo un contenedor permite tener un entorno limpio y reproducible.

En resumen, estas prácticas me han servido para ver cómo se despliega una aplicación de forma profesional, segura y escalable, y por qué tecnologías como Tomcat, Apache, SSL/keystores y Docker siguen siendo tan útiles hoy en día.
