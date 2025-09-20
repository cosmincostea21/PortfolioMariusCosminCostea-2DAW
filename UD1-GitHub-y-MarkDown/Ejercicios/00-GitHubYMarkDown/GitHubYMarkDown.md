# Práctica GitHub + MarkDown
Marius Cosmin Costea

## Introducción
**GitHub** es una plataforma en línea que sirve para **almacenar, compartir y colaborar en proyectos de software**.  
Está basada en **Git**, un sistema de control de versiones que permite llevar un historial de cambios en el código.

### ¿Qué puedes hacer con GitHub?
- 📂 Guardar proyectos en **repositorios (repos)**.  
- 🤝 Colaborar con otras personas en el mismo proyecto.  
- 🌿 Crear **ramas (branches)** para trabajar sin afectar el código principal.  
- 🔄 Hacer **pull requests** para proponer cambios y revisarlos.  
- 📝 Documentar proyectos con archivos como `README.md`.  
- 🌐 Publicar páginas web gratis con **GitHub Pages**.

### ¿Qué vamos a ver en el siguiente informe?

A continuación vamos a ver los primeros pasos para utilizar GitHub, desde como crear repositorios hasta como hacer que otras personas colaboren en nuestros proyectos.

## 1. Creación de la cuenta en GitHub

Para crear una cuenta de GitHub debemos seguir los siguientes pasos:
> 1. Nos dirigimos a https://github.com/
> 2. Buscamos la opción de Sing in.
> 3. Completamos con nuestros datos y accedemos a nuestra cuenta

En mi caso ya presentaba una cuenta de GitHub donde previamente habia subuido proyectos de clase:

![img](https://raw.githubusercontent.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im1.png)


## 2. Mi repositorio.

Para crear repositorio es muy sencillo, buscamos la pesataña de new en la pantalla principal:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/newRepositorio.png)


Posteriormente seguimos los pasos y los parámetros marcados en la siguiente captura:
> 1. El nombre que le hemos puesto al repositorio es: repositorioPruebaGitHub

![repositorio](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im2.png)




## 3. Subir archivos.

Una vez creado el repositorio al acceder a el deberíamos visualizar lo siguiente:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im3.png)

Pinchamos en AddFile y se nos abre la siguiente pestaña:

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im4.png)


## 4. Historial de commits.

Una opción muy util es el historial de commits que nos ofrece GitHub, con ello podemos mantener un registro de las modificaciones, borrados o añadidos que hacemos dentro de nuestro repositorio.
Hemos subido un documento llamado DocumentoCosmin1 - A pesar de que los mensajes de commit son un poco ambiguos es algo que debemos de tener en cuenta, tener claridad en nuestos commits.
 >> ACLARACIÓN: El ultimo mensaje de subiendo archivo de prueba se refiere al documento llamado -> DocumentoCosmin1

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im5.png)


## 5. Creación de ramas.

Las **ramas** permiten trabajar en líneas de desarrollo independientes.
#### 5.1 ¿Para qué sirven?
- Separar trabajo sin afectar a `main`.
- Colaborar en equipo en paralelo.
- Mantener el historial organizado.
- Probar ideas de forma segura.
- Seguir flujos de trabajo (ej. Git Flow).

Pasos para crear en GitHub las ramas:
> 1. Nos dirigimos a nuestro repositorio.
> 2. Pinchamos sobre nuestra rama main.
> 3. En el menú desplegable escribimos un nombre para nuestra nueva rama.
> 4. Creamos la rama (En mi caso la rama nueva es Cosmin).

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im6.png)


### 5.2 Moficamos un archivo desde la rama nueva
> Previamente hemos subido un archivo y vamos a modificarlo desde la **rama Cosmin**  
> Accedemos al apartado de editar el archivo
> Realizamos el commit como se muestra en la imagen

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im7.png)


## 6. Merge.
El comando **`git merge`** se usa para **unir los cambios de una rama a otra**.

### 6.1 ¿Cómo funciona?
- Combina el historial de dos ramas.
- Mantiene los commits originales.
- Puede generar **conflictos** si el mismo archivo fue editado en ambas ramas.

Dentro de las opciones de pull-request, que se encuentra barra horizontal de navegación de GitHub, podemos observar tanto la notificacion de que tenemos incongruencia en las ramas como la 
opción new Pull-Request.

![img](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im8.png)


Una vez hemos accedido al pull-request debemos tener claro que los cambios van hacia la rama main desde la rama Cosmin para que se produzcan los cambios correctamente:

![](https://github.com/cosmincostea21/PortfolioMariusCosminCostea-2DAW/blob/main/UD1-GitHub-y-MarkDown/Ejercicios/00-GitHubYMarkDown/imagenes/im9.png)














