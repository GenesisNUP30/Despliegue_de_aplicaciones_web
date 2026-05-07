# Actividad 2 - Introducción a los contenedores Docker

## Contenedor 1 - El "Hola Mundo" de docker
Vamos a comprobar que todo funciona creando nuestro primer contenedor desde la imagen hello-world:
Para ello hacemos 

```bash
docker run hello-word
```

![Imagen 1_1](/recursos/tema5/actividad2/1_1.png)

Para listar los contenedores que se están ejecutando hacemos:
```bash
docker ps
```

![Imagen 1_2](/recursos/tema5/actividad2/1_2.png)

Como podemos ver, este contenedor no se está ejecutando. Para ver los contenedores que no se están ejecutando usamos el comando
```bash
docker ps -a
```

![Imagen 1_3](/recursos/tema5/actividad2/1_3.png)

Para eliminar el contenedor podemos identificarlo con su id:
```bash
docker rm 436d5d5ea018
```

o con su nombre:
```bash
docker rm silly-banach
```
 

## Contenedor 2 - Ejecución simple de contenedores
Con el comando run vamos a crear un contenedor donde vamos a ejecutar un comando, en este caso vamos a crear el contenedor a partir de una **imagen ubuntu**.
Como todavía no hemos descargado ninguna imagen del registro docker hub, es necesario que se descargue la imagen

```bash
docker run ubuntu echo 'Hello World'
```

![Imagen 2_1](/recursos/tema5/actividad2/2_1.png)

En mi caso, se me olvidó escribir el comando **echo**. Por eso solo se descargó la imagen y uve que ejecutar el comando otra vez

Comprobamos que el contenedor ha ejecutado el comando que hemos indicado y se ha parado:

```bash
docker ps -a
```

![Imagen 2_2](/recursos/tema5/actividad2/2_2.png)


Para visualizar las imágenes que ya tenemos descargadas en nuestro registro local:

```bash
docker images
```

![Imagen 2_3](/recursos/tema5/actividad2/2_3.png)


## Contenedor 3 - Ejecutando un contenedor interactivo



## Contenedor 4 - Creando un contenedor demonio



## Contenedor 5 - Creando un contenedor con un servidor web



## Contenedor 6 - Configuración de contenedores con variables de entorno


