# 🧩 1. ¿Qué es un proxy?

Un proxy es un equipo informático que hace de intermediario entre las conexiones de un cliente y un servidor de destino, filtrando todos los paquetes entre ambos. Siendo tú el cliente, esto quiere decir que el proxy recibe tus peticiones de acceder a una u otra página, y se encarga de transmitírselas al servidor de la web para que esta no sepa que lo estás haciendo tú.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/01-img.png)

---

## 🔧 1.1 ¿Qué vamos a hacer en esta práctica?

En nuestro caso nuestro servidor apache es el que va a actuar de intermediario entre la aplicación desplegada en tomcat y el usuario. Lo que hace el proxy posible en nuestro caso es separar tareas. Apache podría funcionar para gestionar otras tareas haciendo asi que el rendimiento de nuestra aplicación mejore notablemente.  
Para ser exactos lo que implementamos es un reverse proxy donde nuestro servidor apache filtra las peticiones.Configuración técnica del reverse Proxy.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/02-img.png)


Esta práctica es habitual porque reproduce un escenario real de producción en el que un servidor web (Apache) actúa como reverse proxy hacia un servidor de aplicaciones (Tomcat). Esto permite mejorar la seguridad, ocultando Tomcat; optimizar el rendimiento delegando el contenido estático a Apache; facilitar la gestión de certificados HTTPS; y preparar la infraestructura para balanceo de carga o despliegues escalables. Al configurarlo, logramos que la aplicación Java sea accesible a través del servidor web principal sin exponer puertos adicionales.

Apache actúa como servidor frontal escuchando en el puerto 80, mientras que Tomcat se mantiene como servidor de aplicaciones escuchando en el puerto 8080. Mediante un reverse proxy, Apache reenvía las peticiones al backend sin exponer directamente Tomcat al cliente.

---

# ⚙️ 2. Configuración técnica del reverse Proxy

Comprobamos que tanto tomcat como apache2 estén funcionando:

- **[Servidor apache2 abierto](guide://action?prefill=Tell%20me%20more%20about%3A%20Servidor%20apache2%20abierto)**

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/03-img.png)


- **[Servicio de tomcat en ejecución](guide://action?prefill=Tell%20me%20more%20about%3A%20Servicio%20de%20tomcat%20en%20ejecuci%C3%B3n)**  
  (Instalación mediante los paquetes de Ubuntu utilizando tomcat10)

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/04-img.png)

---

## 🧱 2.1 Módulos de proxy en apache

Instalamos los módulos que nos permitirán utilizar apache como proxy:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/05-img.png)

- **[proxy](guide://action?prefill=Tell%20me%20more%20about%3A%20proxy)** → Activa la función de proxy en Apache, permitiendo reenviar peticiones a otros servidores.  
- **[proxy_http](guide://action?prefill=Tell%20me%20more%20about%3A%20proxy_http)** → Permite que el proxy funcione específicamente con tráfico HTTP, como el que usa Tomcat.  
- **[rewrite](guide://action?prefill=Tell%20me%20more%20about%3A%20rewrite)** → Permite reescribir o redirigir URLs, útil para ajustar rutas antes de enviarlas al backend.

En conjunto, estos módulos hacen que Apache pueda recibir peticiones del cliente, reenviarlas a Tomcat y devolver la respuesta correctamente.

---

## 🏗️ 2.2 Creamos el VirtualHost

Creamos el VirtualHost. Lo vamos a crear en `/etc/apache2/sites-available`.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/06-img.png)

Activamos el sitio Web y reiniciamos el Servidor:
- **[Activación del sitio](guide://action?prefill=Tell%20me%20more%20about%3A%20Activaci%C3%B3n%20del%20sitio)**
- **[Reinicio del servidor](guide://action?prefill=Tell%20me%20more%20about%3A%20Reinicio%20del%20servidor)**

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/07-img.png)

---

## 🔍 2.3 Comprobación de Tomcat a través de Apache

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/08-img.png)


---

# 📦 3. Aplicación war en tomcat10

Importante tener en cuenta que al instalar tomcat10 a través de los paquetes de ubuntu tenemos en la carpeta de variables (`/var/lib/tomcat10`) donde podemos almacenar nuestras aplicaciones web.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/09-img.png)

Copiamos la aplicación dentro de nuestra carpeta de webapps:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/10-img.png)

Reiniciamos el servidor de tomcat y comprobamos que a través de apache2 vemos nuestra app.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/01-Apache2Proxy/images/11-img.png)

