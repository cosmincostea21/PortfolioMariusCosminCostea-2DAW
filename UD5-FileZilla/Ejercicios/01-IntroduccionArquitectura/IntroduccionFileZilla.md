
# Introducción FileZilla

---

## 1. Servidores FTP, FTPS y SFTP

### 1.1 Servidor FTP
Un **servidor FTP (File Transfer Protocol)** permite almacenar, organizar y transferir archivos entre un cliente y un servidor mediante una arquitectura cliente–servidor. Utiliza dos canales separados:

- **Canal de control** — comandos como login, cambiar directorio o listar archivos  
- **Canal de datos** — transferencias: subidas, descargas y listados

FTP **no cifra la información**, por lo que contraseñas y datos viajan en texto plano. Esto lo hace inseguro en redes públicas.

### 1.2 Servidor FTPS
Un **servidor FTPS (FTP Secure)** añade cifrado **SSL/TLS** al protocolo FTP.

Características principales:

- Datos protegidos contra interceptación  
- Credenciales cifradas  
- Uso de cifrado **explícito (AUTH TLS)** o **implícito**  
- Mantiene la arquitectura de dos canales (control y datos)  
- Requiere certificados digitales para autenticar y cifrar

### 1.3 Servidor SFTP
Un **servidor SFTP (SSH File Transfer Protocol)** es un protocolo distinto a FTP, aunque su nombre pueda confundir. Funciona sobre **SSH** y ofrece:

- Un único canal cifrado  
- Un único puerto (22)  
- Autenticación por contraseña o claves SSH  
- Mayor seguridad y robustez que FTP/FTPS  

SFTP no utiliza los puertos 20/21 ni modos activo/pasivo.

### 1.4 Tabla comparativa

| Protocolo | Seguridad | Puertos | Arquitectura | Cifrado | Uso típico |
|-----------|-----------|---------|--------------|---------|------------|
| **FTP** | Sin cifrado | 21 (control) + 20 o rango pasivo | Dos canales | No | Entornos internos o legacy |
| **FTPS** | Cifrado SSL/TLS | 21 + rango pasivo o 990 (implícito) | Dos canales | Sí, con certificados | Empresas que requieren compatibilidad con FTP pero seguro |
| **SFTP** | Cifrado SSH nativo | 22 | Un solo canal | Sí, obligatorio | Transferencias seguras modernas |

---

## 2. ¿Cómo funciona FileZilla?

### 2.1 ¿Qué es FileZilla?
**FileZilla Server** convierte un ordenador en un servidor FTP o FTPS. Permite aceptar conexiones, autenticar usuarios y gestionar transferencias.

Funcionamiento básico:

- Servicio escuchando en el puerto 21  
- Validación de usuario y contraseña  
- Canal de control para comandos  
- Canal de datos para transferencias  
- Modo activo o pasivo  
- Cifrado SSL/TLS si se usa FTPS  

### 2.2 Relación con servidor FTP
FileZilla Server **implementa** los protocolos FTP y FTPS:

- Sin cifrado → funciona como **servidor FTP**  
- Con TLS y certificados → funciona como **servidor FTPS**  

No es un protocolo distinto, sino el software que permite ofrecer el servicio.

---

## 3. Esquema del protocolo FTP

En FTP, el **modo activo** y el **modo pasivo** definen cómo se establece el canal de datos. Ambos usan el puerto 21 para el canal de control.

### Modo activo (PORT)

- El cliente abre un puerto aleatorio (>1024)  
- El cliente indica al servidor que se conecte a ese puerto  
- El servidor inicia la conexión desde su puerto 20  
- Puede fallar si el cliente está tras firewall o NAT  

**Resumen técnico:**

- Control: cliente → servidor (21)  
- Datos: servidor:20 → cliente:puerto_aleatorio  

### Modo pasivo (PASV)

- El servidor abre un puerto dentro del rango pasivo  
- El servidor indica al cliente que se conecte a ese puerto  
- El cliente inicia la conexión  
- Más compatible con redes modernas  

**Resumen técnico:**

- Control: cliente → servidor (21)  
- Datos: cliente:puerto_aleatorio → servidor:puerto_pasivo  

---

Si quieres, puedo ayudarte a convertir estos apuntes en un PDF, añadir diagramas o generar un esquema visual.
