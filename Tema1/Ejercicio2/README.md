# EJERCICIO 2 - Configuración básica de Apache

## Índice
- [Añadir puerto 80](#puerto-80)
- [Nuevo dominio](#nuevo-dominio)



## Puerto 80
Tenemos que editar el archivo de configuración de puertos de Apache **ports.conf**. Para ello usamos el siguiente comando:
```bash
sudo nano /etc/apache2/ports.conf
```
El comando **nano** se usa para editar un archivo que tenga permisos de escritura, como es en este caso. 

Al introducir el comando, se nos abrirá el archivo ports.conf que podremos editar. Por defecto el servidor Apache usa el puerto 80 para escuchar las peticiones http y eso lo sabemos por la línea que viene por defecto **Listen 80**. 
Por lo tanto, para hacer que Apache también use el puerto 81 añadiremos **Listen 81** debajo de Listen 80. 

![Paso1](/recursos/tema1/ejercicio2/puerto81.png)



## Nuevo dominio














