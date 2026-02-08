# Configurar el rango de puertos

---

## 1. Configurar el rango de puertos pasivos

Debemos acceder al fichero **vsftpd.conf**, habilitar las siguientes líneas y reiniciar el servicio:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/05-ConexionPuertos/images/01-img.png)

```
listen=YES
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
```

Esto define un rango de **101 puertos** para conexiones pasivas.

---

## 2. Conexión en modo activo

Abrimos FileZilla:

**Editar → Opciones → FTP → Modo de transferencia → Activo**

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/05-ConexionPuertos/images/02-img.png)

Realizamos la conexión anónima.  
En este caso no influye y podemos conectarnos correctamente.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/05-ConexionPuertos/images/03-img.png)

---

## 3. Activamos el modo pasivo

En FileZilla:

**Editar → Opciones → FTP → Modo de transferencia → Pasivo**

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/05-ConexionPuertos/images/04-img.png)

Conectamos de nuevo al servidor:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD5-FileZilla/Ejercicios/05-ConexionPuertos/images/05-img.png)

Como era de esperar, funciona correctamente.

---

## 4. Tabla comparativa

| Característica | Modo Activo (PORT) | Modo Pasivo (PASV) |
|----------------|--------------------|---------------------|
| Quién inicia la conexión de datos | El servidor hacia el cliente | El cliente hacia el servidor |
| Puertos usados en el cliente | Puerto aleatorio entrante | Puerto aleatorio saliente |
| Puertos usados en el servidor | Solo el 20 | Rango definido (40000–40100) |
| Compatibilidad con firewalls | Mala (bloquea conexiones entrantes) | Excelente (solo conexiones salientes) |
| Configuración necesaria | Ninguna en el servidor | Definir rango de puertos pasivos |
| Uso recomendado | Redes sin firewall estricto | Redes con firewall, NAT o routers domésticos |
| Estabilidad | Puede fallar | Muy estable |

