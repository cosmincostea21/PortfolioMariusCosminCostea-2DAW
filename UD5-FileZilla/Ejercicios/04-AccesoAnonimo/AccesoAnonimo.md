# Configuración de acceso anónimo

---

## 1. Acceso anónimo en FTP

El acceso anónimo en un servidor FTP permite que un usuario se conecte sin tener una cuenta real en el sistema. Normalmente se usa:

· **Nombre de usuario:** anonymous  
· **Contraseña:** vacía o un correo ficticio

En el sistema, ese acceso suele mapearse a un usuario especial (por ejemplo **ftp**) con permisos muy limitados.

En resumen: es como decir *“puedes entrar a mirar, pero no eres nadie concreto para mí”*.

El acceso anónimo tiene sentido cuando quieres compartir archivos públicamente sin obligar a cada persona a tener un usuario y contraseña.

Fragmento relevante del fichero **vsftpd.conf**:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/01-img.png)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/02-img.png)


Esto permite el acceso anónimo **solo con lectura**, sin posibilidad de subir, borrar o modificar archivos.

---

## 2. Limitar el directorio accesible para el usuario anónimo

Creamos el directorio, copiamos un archivo de prueba dentro y asignamos permisos adecuados.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/03-img.png)

Esto significa:

· El usuario ftp (anónimo) puede leer  
· No puede escribir, borrar ni modificar

Añadimos la ruta raíz del usuario anónimo:

```
anonymous_enable=YES
anon_root=/srv/ftp/anon
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/04-img.png)


Reiniciamos el servidor y comprobamos:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/05-img.png)


---

## 3. Establecer permisos de solo lectura al directorio

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/06-img.png)

Aquí había un error: el propietario debía ser **root**, no **ftp**.  
Si ftp fuera propietario, tendría permisos de escritura y el usuario anónimo podría modificar archivos.

El problema se solucionó al cambiar el propietario a root.

---

## 4. Comprobaciones en FileZilla

vsftpd es el **servidor FTP** y FileZilla es el **cliente FTP** que utilizamos para conectarnos.

Instalación:

```
sudo apt install filezilla
filezilla -v
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/07-img.png)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/08-img.png)


Abrimos el gestor de sitios:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/09-img.png)


Creamos nuestro sitio FTP:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/10-img.png)

Configuración del sitio:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/11-img.png)

Conexión al servidor:

Tras configurar nuestro cliente ftp, nos le damos a conectar. Saltará una ventana emergente la cual
nos pedirá si deseamos guardar la contraseña pero nuestro usuario anónimo no presenta ninguna por
lo que no guardamos.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/12-img.png)

---

## 5. Pruebas

Aquí vemos que el fichero **hosts** copiado previamente puede visualizarse mediante acceso anónimo:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/14-img.png)

Cuando hablamos de permisos de escritura, significa que **no podemos subir archivos**, pero sí descargarlos:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/15-img.png)

---

## 6. Incidencias

Hubo un problema al conectarse: el fichero `vsftpd.conf` tenía activado:

```
chroot_local_user=YES
```

Estas líneas pertenecían a la práctica anterior y debían comentarse:

```
#chroot_local_user=YES
#allow_writeable_chroot=YES
#local_root=/srv/ftpgroup-cosmin
```
![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/16-img.png)


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/04-AccesoAnonimo/images/17-img.png)

Las capturas anteriores muestran por qué aparecían errores al intentar conectar desde FileZilla.
