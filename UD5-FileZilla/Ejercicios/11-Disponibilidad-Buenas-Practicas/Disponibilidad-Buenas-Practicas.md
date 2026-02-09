# **Recomendaciones para Garantizar la Disponibilidad y Seguridad de un Servidor FTP en Producción**

## **1. Límites de conexión**

- **Restringir el número máximo de clientes simultáneos** para evitar saturación del servicio.  
  Ejemplo recomendado:  
  - `max_clients=5`  
  - `max_per_ip=2`
- **Configurar límites de velocidad** (throttling) para evitar abusos de ancho de banda.
- **Habilitar modo pasivo con un rango de puertos definido**, facilitando la gestión en firewall y evitando bloqueos inesperados.
- **Desactivar acceso anónimo** salvo que sea estrictamente necesario.
- **Forzar FTPS (FTP sobre TLS)** para evitar robo de credenciales en conexiones concurrentes.

---

## **2. Logs y auditoría**

- **Activar el registro de transferencias** (`xferlog_enable=YES`) para disponer de un historial completo de actividad.
- **Guardar logs en un directorio protegido**, accesible solo por administradores.
- **Habilitar syslog** para centralizar eventos críticos del servicio.
- **Auditar accesos fallidos** para detectar intentos de fuerza bruta.
- **Rotar los logs automáticamente** mediante `logrotate` para evitar que crezcan indefinidamente.
- **Registrar conexiones TLS** para verificar que los clientes usan cifrado.

---

## **3. Copias de seguridad**

- **Realizar copias de seguridad periódicas** del directorio raíz del FTP (DocumentRoot).
- **Incluir en las copias:**
  - Archivos subidos por usuarios
  - Configuración de vsftpd
  - Certificados TLS
  - Configuración de Apache (si el FTP alimenta un sitio web)
- **Automatizar las copias** mediante `cron` o herramientas como `rsnapshot`.
- **Mantener copias fuera del servidor** (off‑site) para recuperación ante desastres.
- **Probar la restauración periódicamente** para garantizar que las copias son válidas.

---

## **4. Firewall y NAT**

- **Abrir únicamente los puertos necesarios:**
  - Puerto 21 (control FTP)
  - Puerto 20 (modo activo)
  - Rango pasivo configurado (ej. 40000–40100)
- **Permitir solo conexiones FTPS** si el servidor está expuesto a Internet.
- **Configurar NAT correctamente** si el servidor está detrás de un router:
  - Redirigir puerto 21
  - Redirigir rango pasivo
  - Establecer IP pública en `pasv_address`
- **Bloquear intentos de fuerza bruta** con herramientas como:
  - Fail2ban
  - UFW con rate limiting
- **Restringir acceso por IP** si el servidor solo debe ser accesible desde redes internas.

---

## **5. Permisos y seguridad del sistema**

- **Usar `local_umask=022`** para garantizar que los archivos subidos tengan permisos 644.
- **Enjaular a los usuarios** (`chroot_local_user=YES`) para evitar que naveguen por el sistema.
- **Asegurar que los directorios superiores tienen permisos 755**, permitiendo acceso a Apache sin comprometer seguridad.
- **Evitar que el usuario FTP tenga acceso SSH**, asignándole un shell restringido si es necesario.
- **Mantener certificados TLS actualizados** y con permisos 600.
- **Actualizar el servidor regularmente** para corregir vulnerabilidades.

---

## **6. Disponibilidad y continuidad del servicio**

- **Supervisar el servicio con systemd** para reinicios automáticos:
  - `Restart=on-failure`
- **Monitorizar el estado del servidor** con herramientas como:
  - Nagios
  - Zabbix
  - Prometheus
- **Implementar alta disponibilidad** si el servicio es crítico:
  - Balanceadores de carga
  - Réplicas del servidor
- **Evitar caídas por disco lleno** mediante alertas de uso de espacio.
- **Aislar el servicio FTP en una máquina virtual o contenedor** para evitar que otros servicios afecten su disponibilidad.

---

## **Conclusión**

Un servidor FTP en producción requiere una combinación de seguridad, control de acceso, auditoría, copias de seguridad y una configuración robusta de red. Aplicar estas recomendaciones garantiza:

- Disponibilidad continua del servicio  
- Protección frente a ataques  
- Integridad de los datos  
- Cumplimiento de buenas prácticas profesionales  
