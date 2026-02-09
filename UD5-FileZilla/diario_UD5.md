# 1. ¿Qué he aprendido?

He aprendido a instalar, configurar y utilizar un **servidor FTP/FTPS con vsftpd**, así como a trabajar con **FileZilla** y con el cliente FTP por línea de comandos. A lo largo de las prácticas he comprendido cómo funciona el protocolo FTP, qué diferencias existen entre FTP, FTPS y SFTP, y cómo se gestionan los distintos modos de transferencia (activo y pasivo).  

También he aprendido a crear y administrar **usuarios y grupos**, asignar permisos adecuados y configurar distintos **directorios raíz** según las necesidades de cada ejercicio. Durante el proceso he trabajado con varios entornos:  
- `/srv/ftpgroup-cosmin` para usuarios locales y permisos compartidos  
- `/var/www/ftpweb-cosmin` para integrar FTP con Apache  
- `/srv/ftp/anon` para pruebas puntuales de acceso anónimo  

Otro aspecto importante ha sido la **configuración de FTPS**, generando certificados y ajustando las directivas necesarias para asegurar las conexiones. Esto me ha permitido entender mejor cómo se protege la transferencia de datos y qué requisitos debe cumplir el servidor para aceptar conexiones cifradas.

Además, he integrado el servidor FTP con **Apache**, creando un VirtualHost y configurando el archivo `/etc/hosts` para acceder a un dominio local. Gracias a esto he podido subir archivos mediante FTP y verlos publicados en el navegador, comprendiendo cómo se relacionan ambos servicios.

Entre los conocimientos adquiridos destaco:  
- Instalación y configuración completa de vsftpd.  
- Gestión de usuarios, grupos y permisos.  
- Configuración de FTPS con certificados TLS.  
- Uso de FileZilla y del cliente FTP por terminal.  
- Configuración de modos activo y pasivo.  
- Integración con Apache mediante VirtualHost.  
- Identificación de directivas críticas en `vsftpd.conf` y su impacto en el servicio.  

---

# 2. ¿Qué no entiendo?

Aunque he podido configurar el servidor y realizar todas las prácticas, todavía tengo dudas sobre algunos aspectos más avanzados. Por ejemplo, me gustaría comprender mejor cómo gestiona vsftpd internamente el **chroot** y por qué ciertas combinaciones de directivas pueden impedir que el servicio arranque.  

También tengo dudas sobre la configuración más profunda del **modo pasivo**, especialmente en entornos con NAT o firewalls más complejos. Aunque he configurado un rango de puertos y he visto que funciona, sé que en redes reales puede requerir ajustes adicionales.  

Por último, aunque he integrado FTP con Apache, aún me gustaría entender mejor cómo se organizan los despliegues web cuando hay varios sitios o cuando se trabaja con permisos más estrictos.

---

# 3. ¿Qué es lo que más me ha gustado?

Lo que más me ha gustado ha sido comprobar cómo todas las piezas encajan: usuarios, permisos, directorios raíz, FileZilla, FTPS, Apache y VirtualHosts. Ha sido especialmente interesante ver cómo un archivo subido por FTP aparece inmediatamente en el navegador gracias a la configuración del servidor web.  

También me ha resultado útil trabajar con distintos directorios raíz, porque me ha permitido entender cómo vsftpd adapta su comportamiento según la configuración y cómo pequeños detalles, como una umask o un permiso mal asignado, pueden afectar al funcionamiento del servicio.  

El uso de FileZilla ha sido muy cómodo para visualizar la estructura de directorios y comprobar rápidamente si la configuración era correcta.

---

# 4. ¿Qué es lo que menos me ha gustado?

La parte más complicada ha sido identificar qué directiva estaba causando un error cuando el servicio no arrancaba. Algunas configuraciones, como `chroot_local_user` o los permisos del certificado TLS, son muy sensibles y requieren precisión.  

También ha sido algo confuso al principio distinguir qué configuración correspondía al servidor FTP y cuál al servidor Apache, especialmente cuando ambos trabajaban sobre el mismo directorio. Aun así, la práctica ha sido completa y me ha permitido entenderlo con claridad.

---

# 5. ¿Qué más me gustaría saber?

Me gustaría profundizar en configuraciones más avanzadas, como:  
- Cómo asegurar un servidor FTP/FTPS en un entorno real con firewall, NAT y reglas más estrictas.  
- Cómo gestionar varios sitios web y varios directorios raíz de forma simultánea.  
- Cómo automatizar copias de seguridad y auditorías del servidor FTP.  
- Cómo integrar Apache con otros servicios o con aplicaciones más complejas.  
- Cómo optimizar el rendimiento del servidor cuando hay muchos usuarios o muchas transferencias simultáneas.

También me gustaría aprender a diagnosticar problemas de red relacionados con FTP, ya que el protocolo depende de varios puertos y modos de conexión que pueden verse afectados por la configuración del sistema o del router.
