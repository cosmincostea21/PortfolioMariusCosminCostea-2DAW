# 1. 📘 ¿Qué he aprendido?

He aprendido a instalar y configurar **Apache2 en Ubuntu**, creando mi primer **host virtual** y desplegando una página web sencilla.
He comprendido la importancia de mantener el sistema actualizado con `update` y `upgrade`, así como la necesidad de organizar correctamente los archivos de un sitio web dentro de `/var/www`.

Por otro lado, también he aprendido a:

* Instalar Apache2 y comprobar que el servicio funciona correctamente con `systemctl status apache2`.
* Crear un directorio para mi página web dentro de `/var/www/miweb` y asignar los permisos adecuados con `chown`.
* Configurar un **host virtual** creando un archivo `.conf` en `/etc/apache2/sites-available` y habilitándolo con `a2ensite`.
* Reiniciar el servicio de Apache para que los cambios surtan efecto con `systemctl restart apache2`.
* Documentar todo el proceso en **Markdown**, incluyendo capturas de pantalla y explicaciones paso a paso. ✍️

---

# 2. ❓ ¿Qué no entiendo?

Aunque he podido levantar mi página web y acceder a ella desde el navegador, todavía me cuesta entender en profundidad:

* Cómo funcionan internamente los **hosts virtuales** y cómo Apache gestiona múltiples sitios en un mismo servidor.
* La diferencia entre los **archivos de configuración globales** y los de cada sitio (`apache2.conf` vs `000-default.conf`).
* Cómo se gestionan los **permisos y propietarios de los archivos** de manera óptima para producción.

---

# 3. 🌟 ¿Qué es lo que más me ha gustado?

Lo que más me ha gustado ha sido ver cómo, con unos pocos comandos y configuraciones, pude levantar mi **propio sitio web** y acceder a él desde un navegador.
También me ha parecido muy útil el hecho de **documentar todo el proceso en Markdown con imágenes**, ya que facilita recordar los pasos y entender cómo funcionan los host virtuales. 😄

---

# 4. 👎 ¿Qué es lo que menos me ha gustado?

Lo más complicado ha sido comprender la estructura de directorios de Apache y asegurarse de que los permisos fueran correctos para que el servidor pudiera servir los archivos.
Por lo demás, la práctica ha sido muy clara y bien estructurada. ✔️

---

# 5. 🚀 ¿Qué más me gustaría saber?

Me gustaría aprender a:

* Configurar **múltiples host virtuales** en un mismo servidor Apache.
* Implementar **HTTPS** con certificados SSL/TLS para mis sitios web.
* Optimizar la **seguridad y rendimiento** de Apache en entornos de producción.
* Entender cómo integrar Apache con bases de datos o lenguajes del lado del servidor como PHP.
