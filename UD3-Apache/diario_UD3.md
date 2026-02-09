# 1. ¿Qué he aprendido?

He aprendido a instalar y configurar **Apache2 en Ubuntu**, creando mi primer **host virtual** y desplegando una página web básica.  
También he entendido la importancia de mantener el sistema actualizado mediante `update` y `upgrade`, así como de organizar correctamente los archivos del sitio dentro de `/var/www`.

Además, he adquirido conocimientos como:  
- Instalar Apache2 y verificar su funcionamiento con `systemctl status apache2`.  
- Crear un directorio propio en `/var/www/miweb` y asignar permisos adecuados mediante `chown`.  
- Configurar un **host virtual** creando un archivo `.conf` en `/etc/apache2/sites-available` y habilitándolo con `a2ensite`.  
- Reiniciar Apache para aplicar cambios con `systemctl restart apache2`.  
- Documentar el proceso en **Markdown**, incorporando capturas y explicaciones detalladas.

---

# 2. ¿Qué no entiendo?

Aunque he conseguido desplegar mi sitio web y acceder a él desde el navegador, aún tengo dudas sobre:  
- El funcionamiento interno de los **hosts virtuales** y cómo Apache gestiona varios sitios en un mismo servidor.  
- La diferencia entre los **archivos de configuración globales** y los específicos de cada sitio (`apache2.conf` frente a `000-default.conf`).  
- La gestión óptima de **permisos y propietarios** en entornos de producción.

---

# 3. ¿Qué es lo que más me ha gustado?

Me ha resultado especialmente motivador comprobar cómo, con unos pocos comandos y configuraciones, es posible levantar un **sitio web propio** y acceder a él desde el navegador.  
También ha sido muy útil documentar el proceso en **Markdown**, ya que facilita la comprensión y permite revisar los pasos con claridad.

---

# 4. ¿Qué es lo que menos me ha gustado?

La parte más compleja ha sido comprender la estructura de directorios de Apache y ajustar correctamente los permisos para que el servidor pudiera servir los archivos sin problemas.  
Aun así, la práctica ha sido clara, completa y bien organizada.

---

# 5. ¿Qué más me gustaría saber?

Me gustaría profundizar en:  
- La configuración de **múltiples hosts virtuales** en un mismo servidor.  
- La implementación de **HTTPS** mediante certificados SSL/TLS.  
- La optimización de la **seguridad y el rendimiento** de Apache en producción.  
- La integración de Apache con bases de datos o lenguajes del lado del servidor como PHP.
