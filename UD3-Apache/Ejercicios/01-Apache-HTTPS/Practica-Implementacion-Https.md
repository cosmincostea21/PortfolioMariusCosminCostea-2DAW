# Apache: HTTPS

## 1. Investigación

### 1.1 Protocolo HTTPS e importancia en la seguridad
El protocolo de transferencia de hipertexto seguro (**HTTPS**) es la versión segura de HTTP, utilizado para enviar datos entre un navegador y un sitio web.  
HTTPS cifra las comunicaciones mediante **TLS (Transport Layer Security)**, antes conocido como SSL. Este protocolo usa **infraestructura de clave pública asimétrica**, con dos claves:

- **Clave privada**: controlada por el propietario del sitio web, ubicada en el servidor y utilizada para desencriptar la información.  
- **Clave pública**: disponible para todos los usuarios, permite cifrar la información que solo puede ser desencriptada por la clave privada.

![img0](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/00-img.png)

---

### 1.2 Tipos de certificados SSL/TLS
Un certificado SSL/TLS incluye la clave pública del servidor, el dominio y la firma de la autoridad certificadora (CA). Sirve para:

- **Encriptar la conexión** entre navegador y servidor.  
- **Autenticar la identidad** del servidor y evitar suplantaciones.

#### Tipos según dominios cubiertos
| Tipo | Qué cubre |
|------|-----------|
| **Dominio único** | Solo un dominio concreto. No incluye subdominios. |
| **Comodín (Wildcard)** | Un dominio y todos sus subdominios (ejemplo: `*.ejemplo.com`). |
| **Multidominio (MDC)** | Varios dominios distintos en un único certificado. |


#### Niveles de validación
- **DV (Validación de dominio)**: verificación básica, rápida y barata.  
- **OV (Validación de organización)**: la CA comprueba datos legales de la organización.  
- **EV (Validación extendida)**: máxima confianza, con verificación exhaustiva de la entidad.

![img1](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/01-img.jpg)

---

### 1.3 Módulos en Apache2
Para habilitar HTTPS en Apache sobre Ubuntu se necesitan principalmente:

- **mod_ssl**: maneja SSL/TLS y configuración de certificados.  
- **mod_headers**: permite añadir cabeceras de seguridad como HSTS.  
- **mod_socache_shmcb**: gestiona caché de sesiones SSL/TLS para mejorar rendimiento.  
- **mod_md**: automatiza obtención y renovación de certificados (útil con Let's Encrypt).

![img3](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/03-img.png)

---

## 2. Ejecución técnica

### 2.1 Estado del servidor Apache
Se verifica que Apache está activo con:

```bash
sudo systemctl status apache2
```

![img4](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/04-img.png)

---

### 2.2 Activación de módulos
```bash
sudo a2enmod ssl
sudo a2enmod headers
systemctl restart apache2
```

![img5](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/05-img.png)

![img6](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/06-img.png)

![img7](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/07-img.png)

---

### 2.3 Generación de certificado autofirmado
Nos aseguramos que tenemos dentro de nuestra carpeta ssl los directorios que hacen referencia a los certificados de seguridad.

![img8](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/08-img.png)


Se crea un certificado autofirmado con OpenSSL:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/ssl/private/apache-selfsigned.key \
-out /etc/ssl/certs/apache-selfsigned.crt
```

Parámetros principales:
- `-x509`: certificado autofirmado.  
- `-nodes`: clave privada sin contraseña.  
- `-days 365`: duración del certificado.  
- `-newkey rsa:2048`: nueva clave RSA de 2048 bits.  
- `-keyout`: ruta de la clave privada.  
- `-out`: ruta del certificado público.

![img9](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/09-img.png)

![img10](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/10-img.png)


---

### 2.4 Configuración del VirtualHost en Apache
Se añade el bloque de configuración en el puerto 443:

```apache
<VirtualHost *:443>
    ServerAdmin cosmincostea0810@gmail.com
    DocumentRoot /var/www/gci
    ServerName gci.ejemplocosmin.com

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
</VirtualHost>
```

![img11](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/11-img.png)

---

### 2.5 Comprobaciones
Se verifica el acceso con:

```bash
curl -k -I https://gci.ejemplocosmin.com
```

Resultado: **200 OK**.  
Sin embargo, los navegadores muestran advertencia porque el certificado es **autofirmado** y no emitido por una CA reconocida.

![img12](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/12-img.png)

![img13](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/13-img.png)

---

### 2.6 Logs de Apache
Los registros muestran que el acceso se realiza correctamente:

```bash
sudo tail -f /var/log/apache2/access.log
```
![img14](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD3-Apache/Ejercicios/01-Apache-HTTPS/images/14-img.png)

---

## Conclusión
- Se habilitó HTTPS en Apache con un certificado autofirmado.  
- El servidor funciona correctamente, pero los navegadores no confían en certificados autofirmados.  
- Para producción, se recomienda usar certificados emitidos por una **CA reconocida** (ej. Let's Encrypt).
