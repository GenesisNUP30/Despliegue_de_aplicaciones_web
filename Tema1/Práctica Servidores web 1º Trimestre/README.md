# Práctica 1º Trimestre. Servidores Web
## Instalar un servidor web interno para un instituto


### 1. Instalación del servidor web apache. 
#### Usaremos dos dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. El primero servirá el contenido mediante wordpress y el segundo una aplicación en python
Primero editamos el archivo hosts:
```bash
sudo nano /etc/hosts
```
Recordemos que para estas operaciones necesitamos permisos de admministrador por eso usamos el usuarios sudo. 
Dentro del archivo añadimos las siguientes líneas:

127.0.0.1 centro.intranet
127.0.0.1 departamentos.centro.intranet
127.0.0.1 servidor2.centro.intranet

![Imagen 1](/recursos/tema1/practica/1.png)

Ahora tendríamos que instalar Apache y todo el paquete de MySQL, PHP
Para instalar Apache:<code> sudo apt install apache2 </code>


### 2. Activar los módulos necesarios para ejecutar php y acceder a mysql




### 3. Instala y configura wordpress





### 4. Activar el módulo “wsgi” para permitir la ejecución de aplicaciones Python





### 5. Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente.





### 6. Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación





### 7. Instala y configura awstat.






### 8. Instala un segundo servidor de tu elección (nginx, lighttpd) bajo el dominio “servidor2.centro.intranet”. Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar php. Instala phpmyadmin.





