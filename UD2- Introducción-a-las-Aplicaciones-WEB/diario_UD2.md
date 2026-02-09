# 1. ¿Qué he aprendido?

He aprendido a instalar y configurar **Docker en Ubuntu** dentro de una máquina virtual en **VirtualBox**.  
También he comprendido la importancia de mantener el sistema actualizado mediante `update` y `upgrade`, así como la necesidad de añadir repositorios oficiales para garantizar instalaciones seguras.

Además, hemos trabajado con servidores y alojamiento web, incluyendo la instalación de **Apache en Windows 11**.

Entre los conocimientos adquiridos destacan:  
- Instalación de Docker y resolución de problemas de permisos mediante la incorporación del usuario al grupo `docker`.  
- Ejecución de contenedores de prueba como **Nginx** y **Tomcat**, entendiendo la diferencia entre un servidor web y uno de aplicaciones.  
- Uso de comandos básicos como `docker run`, `docker ps` y `docker search`.  
- Documentación del proceso utilizando **Markdown**, incorporando imágenes y explicaciones detalladas.

---

# 2. ¿Qué no entiendo?

Aunque he seguido los pasos correctamente, todavía me resulta complejo comprender en profundidad la **arquitectura interna de Docker**, especialmente la relación entre imágenes, contenedores y volúmenes.  
También me gustaría profundizar en la gestión de **puertos y redes**, ya que en esta práctica solo se ha trabajado con un mapeo básico (`-p 8080:80`).

---

# 3. ¿Qué es lo que más me ha gustado?

Me ha resultado especialmente interesante comprobar cómo es posible desplegar servicios completos como **Nginx** o **Tomcat** con apenas unos comandos y acceder a ellos desde el navegador.  
Asimismo, la documentación en **Markdown** me ha parecido muy útil para estructurar el trabajo de forma clara y profesional.

---

# 4. ¿Qué es lo que menos me ha gustado?

El aspecto más complicado ha sido resolver el error de permisos al ejecutar Docker por primera vez. Aunque la solución era sencilla (`usermod -aG docker $USER`), puede resultar frustrante para quienes se inician.  
Por lo demás, la práctica ha sido completa y bien organizada.

---

# 5. ¿Qué más me gustaría saber?

Me gustaría profundizar en:  
- La creación de **imágenes personalizadas** mediante `Dockerfile`.  
- La gestión de múltiples contenedores con **Docker Compose**.  
- El despliegue de aplicaciones complejas en entornos de producción.  
- Aspectos de **seguridad y mantenimiento** en contenedores.
