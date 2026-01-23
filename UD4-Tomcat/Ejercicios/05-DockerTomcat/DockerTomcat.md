# 🐳 Instalación básica de Docker en Linux

Para instalar Docker de forma rápida en Ubuntu/Debian:

```
sudo apt update
sudo apt install docker.io -y
```

Comprobar que Docker está instalado:

```
docker --version
```

---

Aquí tienes **todo el repaso de comandos básicos de Docker en una tabla Markdown**, limpia y lista para integrar en tu documentación:

---

# 🧭 Repaso de comandos básicos para navegar en Docker

| Acción | Comando |
|--------|----------|
| Ver contenedores en ejecución | `docker ps` |
| Ver todos los contenedores | `docker ps -a` |
| Ver imágenes descargadas | `docker images` |
| Iniciar un contenedor detenido | `docker start nombre_contenedor` |
| Detener un contenedor | `docker stop nombre_contenedor` |
| Eliminar contenedor | `docker rm nombre_contenedor` |
| Eliminar imagen | `docker rmi nombre_imagen` |
| Descargar una imagen | `docker pull nombre_imagen` |
| Entrar dentro de un contenedor | `docker exec -it nombre_contenedor bash` |
| Copiar archivos al contenedor | `docker cp archivo nombre_contenedor:/ruta/` |

---

# 🐳 Tomcat en contenedor Docker

## 1. Descargar la imagen de Tomcat

Descargamos la imagen de Tomcat y lanzamos un contenedor en el puerto 8080 con el nombre `tomcat-demo`:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/05-DockerTomcat/images/01-img.png)

```
docker run -d -p 8080:8080 --name tomcat-demo tomcat
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/05-DockerTomcat/images/02-img.png)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/05-DockerTomcat/images/03-img.png)

El contenedor queda en ejecución y Tomcat disponible en el puerto 8080.

### Copiar la aplicación `sample.war` al contenedor

```
docker cp sample.war tomcat-demo:/usr/local/tomcat/webapps/
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/05-DockerTomcat/images/04-img.png)


Con Apache detenido y Tomcat nativo inactivo, accedemos al puerto 8080 y comprobamos que la aplicación funciona correctamente.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/05-DockerTomcat/images/05-img.png)


![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/05-DockerTomcat/images/06-img.png)


---

## 2. Diferencias entre Tomcat nativo y Tomcat en contenedor

| Aspecto | Tomcat nativo | Tomcat en contenedor |
|---------|----------------|------------------------|
| Instalación y configuración | Requiere instalar Java, configurar rutas y servicios manualmente | Ya viene listo en la imagen Docker, sin configuraciones iniciales |
| Dependencia del sistema operativo | Depende del SO; puede variar entre máquinas | Comportamiento idéntico en cualquier entorno con Docker |
| Aislamiento | Comparte librerías y recursos con el sistema | Aislado completamente; no afecta ni depende del host |
| Actualización | Manual, propensa a errores | Basta con usar una nueva imagen (`docker pull`) |
| Despliegue de aplicaciones | Copiar WARs al servidor y reiniciar | Copiar WAR al contenedor o montar volúmenes |
| Escalabilidad | Requiere configuraciones complejas | Escala fácilmente con Docker Compose o Kubernetes |
| Portabilidad y limpieza | Puede dejar residuos y depende del entorno | Contenedores desechables, reproducibles y limpios |
| Gestión de logs | Logs locales del sistema | Logs centralizados con `docker logs` |

Tomcat nativo funciona bien en entornos tradicionales, pero requiere más configuración y depende del sistema operativo.  
Tomcat en contenedor es más portable, más fácil de desplegar, más limpio, más escalable y más consistente.

---

Si quieres, puedo **integrar también esta parte dentro del documento completo de Apache, Tomcat, Proxy, Seguridad y Rendimiento** que ya hemos generado. Solo dime si quieres que te entregue **todo unido en un único archivo Markdown**.
