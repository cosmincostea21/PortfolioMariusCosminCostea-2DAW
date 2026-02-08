# Acceder a FTPS mediante el navegador

## Por qué no puedo acceder desde el navegador

Mi servidor vsftpd está configurado para exigir seguridad en todo momento. Las directivas:

- `force_local_logins_ssl=YES`  
- `force_local_data_ssl=YES`  

obligan a que tanto el inicio de sesión como las transferencias se realicen mediante **TLS**. Esto convierte el servidor en un entorno **exclusivamente FTPS**.

Cuando intento acceder desde un navegador usando:

```
ftp://cosmin@localhost
```

el navegador intenta abrir una conexión **FTP sin cifrar**, porque:

- Los navegadores modernos eliminaron el soporte FTP en 2021.  
- Nunca implementaron FTPS explícito ni el comando `AUTH TLS`.  
- No pueden negociar un canal seguro con mi servidor.  

Como mi servidor exige TLS desde el primer momento, rechaza la conexión y el navegador muestra un error. Esto confirma que la seguridad está funcionando correctamente y que el navegador no es compatible con un servidor FTPS moderno.

---

## Detalles técnicos de la incompatibilidad

- FTPS explícito requiere que el cliente envíe el comando `AUTH TLS` para iniciar la negociación segura.  
- Los navegadores no implementan este comando.  
- Mi servidor, al tener TLS obligatorio, rechaza cualquier intento de login sin `AUTH TLS`.  
- El navegador no puede continuar porque no entiende la respuesta del servidor.  
- La conexión se cierra antes de mostrar cualquier contenido.  

En resumen: **mi servidor exige cifrado, pero el navegador no sabe hablar FTPS**.

---

## Ventajas y desventajas del navegador como cliente FTP

| Aspecto | Ventajas del navegador | Desventajas del navegador |
|--------|-------------------------|----------------------------|
| Acceso | No requiere instalación | No soporta FTPS explícito ni TLS |
| Facilidad | Interfaz simple y conocida | No permite subir archivos en la mayoría de navegadores |
| Seguridad | — | Solo soporta FTP sin cifrar, inseguro y obsoleto |
| Compatibilidad | Funciona solo con servidores FTP simples | Incompatible con servidores que exigen cifrado, como el mío |
| Funciones | Útil para descargas rápidas si el servidor lo permite | No muestra mensajes de estado ni diagnósticos |
| Control | — | No permite configurar puertos, modo pasivo/activo ni parámetros avanzados |
| Gestión de archivos | — | No permite renombrar, borrar, cambiar permisos ni gestionar colas |
