# 🔐 Seguridad Tomcat

## 🧑‍💻 1. Administración de usuarios

Accedemos al directorio `/etc/tomcat10` y abrimos el XML `tomcat-users.xml`.

Vamos a definir dos roles **manager** y **admin** de la siguiente manera:

```
<!-- Ejemplo típico, tú añadirías tu propio contenido -->
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="admin" password="admin123" roles="manager-gui,admin-gui"/>
```

---

## 🚫 2. Restringir el acceso al Manager

Debemos instalar el paquete **tomcat10-admin**:

- **tomcat10** → Solo el servidor base, no incluye Manager.  
- **tomcat10-admin** → Añade Manager y Host Manager + archivos de contexto (`manager.xml`, `host-manager.xml`) que permiten la autenticación por roles.  
- **Sin tomcat10-admin**, aunque tengas usuarios en `tomcat-users.xml`, **no hay Manager** para probar acceso autenticado.

---

### 🌐 Acceso al Manager

Vamos a la página del Manager.

Las credenciales son las establecidas anteriormente:

```
admin / admin123
```

---

# 🔒 3. Configuramos HTTPS

HTTP transmite datos sin cifrar, lo que significa que usuarios y contraseñas pueden ser interceptadas.

- **HTTPS** cifra la comunicación usando SSL/TLS, garantizando confidencialidad e integridad.  
- El **keystore** contiene un certificado autofirmado para pruebas locales.  

Esto garantiza que el acceso al Manager sea seguro.

### 🔧 Generación del certificado

```
sudo keytool -genkeypair -alias tomcat \
-keyalg RSA -keysize 2048 \
-keystore /etc/tomcat10/tomcat.keystore \
-validity 365
```

### 📋 Opciones del comando

| Opción | Función |
|-------|---------|
| **-genkeypair** | Genera un par de claves: pública y privada. |
| **-alias tomcat** | Nombre que identifica este certificado dentro del keystore. |
| **-keyalg RSA** | Algoritmo de cifrado de la clave (RSA es estándar). |
| **-keysize 2048** | Tamaño de la clave en bits (2048 es seguro para pruebas). |
| **-keystore /etc/tomcat10/tomcat.keystore** | Archivo donde se guardará el certificado y clave privada. |
| **-validity 365** | Tiempo de validez en días (1 año para pruebas). |

**Contraseña utilizada para el KeyStore:** `tomcat1234`

---

### 📁 Verificación de la keystore

Vemos que se ha creado la keystore correctamente.

Es muy importante que Tomcat tenga acceso a nuestra keystore; si no, **no va a funcionar**.

---

## 🔐 3.1 Habilitar HTTPS

Seguidamente vamos a editar `server.xml` para habilitar HTTPS.

El aviso que sale a continuación es normal porque nuestro certificado es **autofirmado**.

Como vemos en la dirección de nuestro gestor de aplicaciones web, tenemos **HTTPS** con la advertencia del navegador en el candado de seguridad.
