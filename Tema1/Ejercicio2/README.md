# EJERCICIO 2 - Configuración básica de Apache

## 1. Hacer que Apache escuche por el puerto 81
Tenemos que editar el archivo de configuración de puertos de Apache **ports.conf**, que especifica los puertos en los que el servidor escuchará las peticiones entrantes. Para ello usamos el siguiente comando:
```bash
sudo nano /etc/apache2/ports.conf
```
El comando **nano** se usa para editar un archivo que tenga permisos de escritura, como es en este caso. 

Al introducir el comando, se nos abrirá el archivo ports.conf, que podremos editar. Por defecto el servidor Apache usa el puerto 80 para escuchar las peticiones http y eso lo sabemos por la línea que viene por defecto **Listen 80**. 
Por lo tanto, para hacer que Apache también use el puerto 81 añadiremos **Listen 81** debajo de Listen 80. 

![Paso1](/recursos/tema1/ejercicio2/puerto81.png)


Para guardar los cambios hacemos Ctrl + O y para salir Ctrl + X. 

Ahora, reiniciamos el servidor Apache para que se hagan efectivos los cambios. Para ello usamos el comando 
```bash
sudo service apache2 restart
```
A partir de ahora, podemos usar el puerto 81 escribiendo en el navegador **localhost:81** o **127.0.0.1:81**. Se abrirá la página por defecto de Apache como se ve en la foto con la diferencia de que estaremos el puerto 81. 

![Paso2](/recursos/tema1/ejercicio2/puerto81_1.png)



## 2. Añadir el dominio “marisma.intranet” en el fichero “hosts”
Al agregar este dominio podremos acceder a nuestra página escribiendo marisma.intranet en el navegador. Por eso, tenemos que editar el fichero **/etc/hosts**, que se encarga de asociar nombres de dominio con direcciones IP. 
















