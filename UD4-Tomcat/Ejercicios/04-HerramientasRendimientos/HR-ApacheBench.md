# 🚀 Herramientas de Rendimiento

## 🧩 1. Instalación

Debemos realizar una instalación de **apache2-utils**, ya que incluye ApacheBench, una herramienta clásica para pruebas HTTP.  
ApacheBench es muy útil porque es ligero, no requiere interfaz gráfica y permite controlar la concurrencia y el número total de peticiones. He optado por utilizar esta herramienta ya que me parece la más apropiada y óptima para nuestro caso de estudio.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/04-HerramientasRendimientos/images/01-img.png)

Comprobamos que Tomcat responde correctamente a nuestra petición; la respuesta que nos devuelve nos demuestra que está listo para pruebas **HTTP/1.1 200**.  

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/04-HerramientasRendimientos/images/02-img.png)

---

## 📉 2. Prueba de carga baja

Vamos a enviar **100 peticiones** con el atributo `-n100` y **10 peticiones simultáneas** con el atributo `-c10`, verificando que ApacheBench pueda comunicarse con la aplicación y obtener métricas iniciales sin saturar el sistema.

**Comando:**

```
ab -n 100 -c 10 http://localhost:8080/mi-app/
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/04-HerramientasRendimientos/images/03-img.png)

### Parámetros más importantes

| **Parámetro** | **Qué mide** | **Por qué importa** |
|--------------|--------------|---------------------|
| **[Requests per second (RPS)](guide://action?prefill=Tell%20me%20more%20about%3A%20Requests%20per%20second%20(RPS))** | Peticiones atendidas por segundo | Mide la capacidad de procesamiento del servidor |
| **[Time per request (mean)](guide://action?prefill=Tell%20me%20more%20about%3A%20Time%20per%20request%20(mean))** | Tiempo medio por petición | Indica la latencia individual |
| **[Failed requests](guide://action?prefill=Tell%20me%20more%20about%3A%20Failed%20requests)** | Peticiones que no se completaron | Señala saturación o errores del servidor |
| **[Transfer rate](guide://action?prefill=Tell%20me%20more%20about%3A%20Transfer%20rate)** | Velocidad de transferencia de datos | Refleja eficiencia en la entrega de contenido |
| **[Percentiles de tiempo](guide://action?prefill=Tell%20me%20more%20about%3A%20Percentiles%20de%20tiempo)** | Distribución de tiempos de respuesta | Muestra consistencia y estabilidad bajo carga |

### Resultados obtenidos

| **Parámetro** | **Valor obtenido** | **Interpretación técnica** |
|---------------|--------------------|----------------------------|
| **[Requests per second (RPS)](guide://action?prefill=Tell%20me%20more%20about%3A%20Requests%20per%20second%20(RPS))** | 1661.30 req/s | capacidad de procesamiento muy alta; el servidor atiende más de 1600 peticiones por segundo sin errores. |
| **[Time per request (mean)](guide://action?prefill=Tell%20me%20more%20about%3A%20Time%20per%20request%20(mean))** | 6.019 ms | latencia media muy baja; cada petición se resuelve en ~6 ms. |
| **[Time per request (across all concurrent requests)](guide://action?prefill=Tell%20me%20more%20about%3A%20Time%20per%20request%20(across%20all%20concurrent%20requests))** | 0.602 ms | eficiencia con concurrencia; cada hilo gestiona peticiones muy rápido. |
| **[Failed requests](guide://action?prefill=Tell%20me%20more%20about%3A%20Failed%20requests)** | 0 | estabilidad total; no hubo saturación ni errores del servidor. |
| **[Transfer rate](guide://action?prefill=Tell%20me%20more%20about%3A%20Transfer%20rate)** | 1382.25 KB/s | velocidad de transferencia elevada; entrega fluida. |
| **[p50](guide://action?prefill=Tell%20me%20more%20about%3A%20p50)** | 4 ms | tiempos muy bajos en la mitad de las peticiones. |
| **[p90](guide://action?prefill=Tell%20me%20more%20about%3A%20p90)** | 10 ms | excelente consistencia. |
| **[p99](guide://action?prefill=Tell%20me%20more%20about%3A%20p99)** | 14 ms | solo el 1% supera los 14 ms. |
| **[Tiempo máximo](guide://action?prefill=Tell%20me%20more%20about%3A%20Tiempo%20m%C3%A1ximo)** | 18 ms | sin picos anómalos. |

---

## 📈 3. Prueba de carga media

Simulamos más tráfico aumentando las peticiones a **2000** y las simultáneas a **50**.
```
ab -n 2000 -c 50 http://localhost:8080/sample/
```

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/04-HerramientasRendimientos/images/04-img.png)

### Resultados obtenidos

| **Parámetro** | **Valor obtenido** | **Interpretación técnica** |
|---------------|--------------------|----------------------------|
| **[Requests per second (RPS)](guide://action?prefill=Tell%20me%20more%20about%3A%20Requests%20per%20second%20(RPS))** | 2820.96 req/s | rendimiento excelente; incluso superior a la prueba de baja carga. |
| **[Time per request (mean)](guide://action?prefill=Tell%20me%20more%20about%3A%20Time%20per%20request%20(mean))** | 17.724 ms | latencia aceptable; sube como es esperable. |
| **[Time per request (across all concurrent requests)](guide://action?prefill=Tell%20me%20more%20about%3A%20Time%20per%20request%20(across%20all%20concurrent%20requests))** | 0.354 ms | muy eficiente por hilo concurrente. |
| **[Failed requests](guide://action?prefill=Tell%20me%20more%20about%3A%20Failed%20requests)** | 0 | estabilidad total. |
| **[Transfer rate](guide://action?prefill=Tell%20me%20more%20about%3A%20Transfer%20rate)** | 2347.13 KB/s | velocidad muy alta. |
| **[p50](guide://action?prefill=Tell%20me%20more%20about%3A%20p50)** | 15 ms | rendimiento sólido. |
| **[p90](guide://action?prefill=Tell%20me%20more%20about%3A%20p90)** | 29 ms | buena consistencia. |
| **[p99](guide://action?prefill=Tell%20me%20more%20about%3A%20p99)** | 43 ms | sin picos graves. |
| **[Tiempo máximo](guide://action?prefill=Tell%20me%20more%20about%3A%20Tiempo%20m%C3%A1ximo)** | 88 ms | razonable para carga media. |

---

## ⚖️ 4. Comparación de las pruebas de carga

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/04-HerramientasRendimientos/images/05-img.png)

El servidor se mantiene estable en ambas pruebas: no aparecen errores, la respuesta es consistente y la capacidad de procesamiento incluso mejora con mayor concurrencia.  
La latencia aumenta, pero dentro de lo normal. El tiempo máximo sube, indicando pequeños picos de saturación.  
En conjunto, el servidor **escala bien**, mantiene estabilidad y solo muestra degradación en los puntos esperables.

---

## ⚙️ 5. Ajustes en `server.xml`

Los ajustes clave para limitar o mejorar el rendimiento del servidor se realizan en el archivo **server.xml** de Tomcat.  
Podemos aumentar el número de usuarios concurrentes o limitarlo según el análisis de carga esperado en producción.

Dentro de este fichero debemos buscar la etiqueta **Connector**, donde podemos añadir atributos como:

```
maxThreads="valor"
```

Los *threads* (hilos) son trabajadores que procesan las peticiones entrantes.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/04-HerramientasRendimientos/images/06-img.png)

### Otras propiedades interesantes

- **[minSpareThreads](guide://action?prefill=Tell%20me%20more%20about%3A%20minSpareThreads)**: evita la creación de nuevos hilos bajo carga, mejorando la respuesta.  
- **[maxConnections](guide://action?prefill=Tell%20me%20more%20about%3A%20maxConnections)**: controla cuántas conexiones TCP puede aceptar el servidor.  
- **[acceptCount](guide://action?prefill=Tell%20me%20more%20about%3A%20acceptCount)**: cola de espera; si es baja se rechazarán peticiones antes, si es alta aceptará más peticiones simultáneas.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD4-Tomcat/Ejercicios/04-HerramientasRendimientos/images/07-img.png)
