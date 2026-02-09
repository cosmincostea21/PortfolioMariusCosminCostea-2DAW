# Configuración de un servidor FTPS seguro con vsftpd

Este proyecto consiste en transformar un servidor FTP estándar en un servidor **FTPS explícito**, utilizando cifrado TLS para proteger tanto el inicio de sesión como las transferencias de datos. El objetivo final es garantizar que **todos los usuarios se conecten de forma segura**, rechazando cualquier intento de conexión sin cifrado.

---

## 1. Generación del certificado TLS

Para habilitar FTPS es necesario disponer de un certificado y una clave privada. Se generó un certificado autofirmado mediante OpenSSL:

- Certificado: `/etc/ssl/certs/vsftpd.crt`
- Clave privada: `/etc/ssl/private/vsftpd.key`

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/08-FTPS-Seguro/images/01-img.png)


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/08-FTPS-Seguro/images/02-img.png)


Este certificado permite que el servidor establezca conexiones cifradas mediante TLS.

---

## 2. Activación de FTPS explícito en vsftpd

Se modificó el archivo de configuración `/etc/vsftpd.conf` para:

- Activar el soporte FTPS explícito.
- Indicar las rutas del certificado y la clave privada.
- Habilitar únicamente protocolos seguros.
- Asegurar compatibilidad con clientes modernos como FileZilla.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/08-FTPS-Seguro/images/03-img.png)

Las directivas clave añadidas:

- `ssl_enable=YES`  
- `rsa_cert_file=/etc/ssl/certs/vsftpd.crt`  
- `rsa_private_key_file=/etc/ssl/private/vsftpd.key`  
- `ssl_tlsv1=YES`  
- `ssl_sslv2=NO`  
- `ssl_sslv3=NO`  
- `require_ssl_reuse=NO`  
- `ssl_ciphers=HIGH`  

Con esto, el servidor queda preparado para aceptar conexiones FTPS explícitas.

---

## 3. Obligación de usar conexión segura

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/08-FTPS-Seguro/images/07-img.png)


Para garantizar que ningún usuario pueda conectarse mediante FTP sin cifrar, se activaron dos directivas esenciales:

- `force_local_logins_ssl=YES`  
  Obliga a que el inicio de sesión se realice mediante TLS.

- `force_local_data_ssl=YES`  
  Obliga a que todas las transferencias de datos estén cifradas.

Estas opciones convierten el servidor en un entorno **100% FTPS**, rechazando cualquier intento de conexión insegura.

---

## 4. Reinicio del servicio

Tras aplicar los cambios, se reinició el servicio vsftpd para que la nueva configuración entrara en vigor:

```
systemctl restart vsftpd
```

---

## 5. Conexión FTPS desde FileZilla Client

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/08-FTPS-Seguro/images/04-img.png)

Para verificar la configuración, se creó una conexión guardada en FileZilla Client con los siguientes parámetros:

- Protocolo: FTP  
- Cifrado: **Requiere FTP explícito sobre TLS**  
- Host: localhost  
- Puerto: 21  
- Usuario: cosmin  

Al conectar, FileZilla mostró el aviso del certificado y, tras aceptarlo, estableció una sesión FTPS segura.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/08-FTPS-Seguro/images/05-img.png)


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/08-FTPS-Seguro/images/06-img.png)

Los mensajes de estado confirmaron el cifrado:

- `AUTH TLS`  
- `TLS connection established`  
- `Using TLSv1.3`  
- `PROT P`  

Esto demuestra que la conexión está protegida mediante TLS.

---

## 6. Verificación mediante transferencia segura

Para completar la validación, se realizó una transferencia de archivos dentro de la carpeta remota `upload`.  
El cliente mostró:

- `150 Opening BINARY mode data connection`  
- `226 Transfer complete`  

La transferencia apareció como **satisfactoria**, confirmando que el canal de datos también estaba cifrado.

---

## Resultado final

El servidor vsftpd quedó configurado como un **servidor FTPS explícito seguro**, con:

- Certificado TLS propio  
- Cifrado obligatorio para todos los usuarios  
- Rechazo automático de conexiones sin TLS  
- Compatibilidad con clientes modernos  
- Transferencias cifradas verificadas  

Este entorno cumple los requisitos de una configuración FTPS segura y funcional.

