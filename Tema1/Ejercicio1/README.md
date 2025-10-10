# EJERCICIO 1 - Instalación de Apache

## Contenidos
- [Instalación](#instalación)
  - [Apache](#apache)
  - [MySQL](#mysql)
  - [PHP](#php)

## Instalación
Algo importante a tener en cuenta es que para hacer toda la instalación necesitamos tener permisos de administrador. Para eso siempre vamos a usar la palabra **sudo**, con la que adquirimos el rol de administrador con el que podemos ejecutar todos los comandos necesarios. 

### Apache
1. Actualizamos los paquetes
   ```bash
    sudo apt update
   ```
    Si es la primera vez que usamos sudo nos pedirá la contraseña de usuario para poder obtener los privilegios adecuados para administrar los paquetes del sistema.

   ![Paso1](/recursos/tema1/ejercicio1/apache1.png)


3. Instalamos Apache
    ```bash
    sudo apt install apache2
   ```
   Introducimos la contraseña de usuario y confirmamos la instalación escribiendo s (de sí).

   ![Paso2](/recursos/tema1/ejercicio1/apache2.png)
   

5. Probamos el funcionamiento
   Para ver si nuestro servidor web está correctamente instalado, abrimos el navegador y en la barra de búsqueda escribimos **http://nuestra_ip**, es decir, **http://127.0.0.1** o lo
   que es lo mismo **localhost**.
   Se mostrará la página web predeterminada de Apache tal como se ve en la foto:


   ![Paso3](/recursos/tema1/ejercicio1/apache3.png)
   
   
### MySQL
Ahora vamos a instalar MySQL, que es un sistema de base de datos donde almacenaremos y gestionaremos los datos de nuestro sitio web.
Como ya hemos actualizado los paquetes para instalar Apache directamente introducimos el comando para instalar MySQL:

1. Instalación
   ```bash
    sudo apt install mysql-server
    ```
   
   ![Paso1](/recursos/tema1/ejercicio1/mysql1.png)

2. Comprobar el funcionamiento
   Iniciamos sesión en la terminal para verificar si podemos entrar:
   ```bash
     sudo mysql
   ```
   
   ![Paso2](/recursos/tema1/ejercicio1/mysql2.png)
   

   Como podemos comprobar, todo está correcto
   Para salir de la consola de MySQL hacemos:
   ```bash
     exit
   ```

### PHP
Es el componente que procesa el código para mostrar contenido dinámico al usuario final. Es necesario instalar **php-mysql**, un módulo de PHP que permite comunicarnos con bases de datos basadas en MySQL. Y también necesitaremos **libapache2-mod-php** para que Apache pueda gestionar archivos PHP. 

1. Instalación
   ```bash
     sudo apt install php libapache2-mod-php php-mysql
   ```

   ![Paso1](/recursos/tema1/ejercicio1/php1.png)
   

2. Comprobación
   Una vez instalado, para verficar la versión de PHP hacemos:
   ```bash
       php -v
   ```

   ![Paso1](/recursos/tema1/ejercicio1/php2.png)
   




