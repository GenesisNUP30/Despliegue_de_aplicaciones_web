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
En este caso vamos a usar diferentes opciones:
- **-i**: para abrir una sesión interactiva
- **-t**: crear un pseudo-terminal que nos va a permitir interaccionar con el contenedor
- **--name**: indicar un nombre del contenedor con la opción --name
- ***nombre_imagen**: la imagen que vamos a utilizar para crearlo que en este caso es ubuntu
- **nombre_comando**: el comando que vamos a ejecutar, en este caso bash, que lanzará una sesión bash en el contenedor:

```bash
docker run -it --name contenedor1 ubuntu bash
```

![Imagen 3_1](/recursos/tema5/actividad2/3_1.png)

Para salirnos o desconectarnos de él hacemos **exit**

El contenedor se para cuando salimos de él. Para volver a conectarnos a él:
```bash
docker start contenedor1
docker attach contenedor1
```

![Imagen 3_2](/recursos/tema5/actividad2/3_2.png)

Si el contenedor se está ejecutando podemos ejecutar comandos en él con el subcomando **exec**:

```bash
docker exec contenedor1 ls -al
```

![Imagen 3_3](/recursos/tema5/actividad2/3_3.png)

Para mostrar información del contenedor ejecutamos:

```bash
docker inspect contenedor1
```

![Imagen 3_4](/recursos/tema5/actividad2/3_4.png)

La información está en JSON y entre otras cosas se muestra:
- El id del contenedor.
- Los puertos abiertos y sus redirecciones
- Los bind mounts y volúmenes usados.
- El tamaño del contenedor


## Contenedor 4 - Creando un contenedor demonio
Para hacerlo usamos la opción -d del comando run, para que la ejecución del comando en el contenedor se haga en segundo plano.

```bash
docker run -d --name contenedor2 ubuntu bash -c "while true; do echo hello world; sleep 1; done"
```

![Imagen 4_1](/recursos/tema5/actividad2/4_1.png)

El comando bash -c le dice al contenedor que inicie una terminal Bash y ejecute el comando que está entre comillas. En este caso, lo que se ejecuta es un bucle infinito donde se escribe en pantalla el texto hello world y luego esperar 1 segundo, para volver a empezar una y otra vez. 

Vamos a comprobar que el contenedor se está ejecutando:

```bash
docker ps
```

![Imagen 4_2](/recursos/tema5/actividad2/4_2.png)

Y ahora vemos lo que está haciendo el contedor:

```bash
docker logs contenedor2
```

![Imagen 4_3](/recursos/tema5/actividad2/4_3.png)

Como vemos se está ejecutando el bucle correctamente. 

Para parar el contenedor hacemos

```bash
docker stop contenedor2
```

![Imagen 4_4](/recursos/tema5/actividad2/4_4.png)



## Contenedor 5 - Creando un contenedor con un servidor web



## Contenedor 6 - Configuración de contenedores con variables de entorno


