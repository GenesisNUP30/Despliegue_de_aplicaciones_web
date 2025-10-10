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

2. Instalamos Apache
    ```bash
    sudo apt install apache2
   ```
   Introducimos la contraseña de usuario y confirmamos la instalación escribiendo s (de sí).

3. Probamos el funcionamiento
   Para ver si nuestro servidor web está correctamente instalado, abrimos el navegador y en la barra de búsqueda escribimos **http://nuestra_ip**, es decir, **http://127.0.0.1** o lo
   que es lo mismo **localhost**.
   Se mostrará la página web predeterminada de Apache para Ubuntu 24 tal como se ve en la foto: 

   

   
   


