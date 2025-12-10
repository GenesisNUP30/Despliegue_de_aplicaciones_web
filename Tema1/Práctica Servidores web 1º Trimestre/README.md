# Práctica 1º Trimestre. Servidores Web
## Instalar un servidor web interno para un instituto


### 1. Crear 2 dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. 
#### El primero servirá el contenido mediante wordpress y el segundo una aplicación en python
Para eso editamos el archivo /etc/hosts: 
```bash
sudo nano /etc/hosts
```
Recordemos que para estas operaciones necesitamos permisos de administrador por eso usamos el usuarios sudo. 
Dentro del archivo añadimos las siguientes líneas:

- 127.0.0.1 centro.intranet
- 127.0.0.1 departamentos.centro.intranet
- 127.0.0.1 servidor2.centro.intranet

Para guardar y cerrar Ctrl + C y Ctrl + X. 

![Imagen 1](/recursos/tema1/practica/1.png)


### 2. Instalar Apache, PHP y MySQL y activamos los módulos necesarios

Ahora tendríamos que instalar Apache y todo el paquete de MySQL, PHP. 
Primero actualizamos los paquetes del repositorio de Linux con <code>sudo apt update</code>
- Para instalar Apache:
```bash
sudo apt install apache2
```
- Para instalar MySQL:
  ```bash
  sudo apt install mysql-server
  ```
- Para instalar PHP:
  ```bash
  sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql php-cgi php-curl php-json
  ```
  Con este comando instalamos:
  - <code> php-mysql </code> : un módulo PHP que permite que este se comunique con bases de datos basadas en MySQL. 
  - <code> libapache2-mod-php </code> : para permitir que Apache gestione archivos PHP.

Después de hacer todas las configuraciones hacemos:
```bash
sudo systemctl restart apache2
```
Para comprobar que el servidor apache está activo hacemos
```bash
sudo systemctl status apache2
```

![Imagen 2](/recursos/tema1/practica/2.png)



### 3. Crear directorios para los dominios
Tenemos que crear los directorios para los 2 sitios con
```bash
sudo mkdir -p /var/www/centro.intranet
sudo mkdir -p /var/www/departamentos.centro.intranet
```

![Imagen 3](/recursos/tema1/practica/3.png)


### 3.1. Configurar VirtualHost en Apache
Para configurar centro.intranet:
```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```

Y dentro del archivio ponemos este contenido:
```bash
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
Este sitio servirá el contenido en WordPress. 

![Imagen 4](/recursos/tema1/practica/4.png)

Y para el otro sitio hacemos lo mismo:
```bash
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```

Y dentro de este fichero ponemos:
```bash
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    DocumentRoot /var/www/departamentos.centro.intranet

    WSGIScriptAlias / /var/www/departamentos.centro.intranet/app.wsgi

    <Directory /var/www/departamentos.centro.intranet>
        Require all granted
    </Directory>
</VirtualHost>
```

![Imagen 5](/recursos/tema1/practica/5.png)

Este servidor se ejecutará con una aplicación en Python, por eso usamos WSGI para permitir la ejecución de aplicaciones Python. 
Más adelante terminaremos de hacer la configuración para este sitio. 


Por último, habilitamos los sitios que hemos creado con
```bash
sudo a2ensite centro.intranet.conf
sudo a2ensite departamentos.centro.intranet.conf
```

Y reiniciamos Apache con <code>sudo systemctl restart apache2</code>

### 3.2 Instalar y configurar Wordpress para el sitio centro.intranet

1) Tenemos que descargar Wordpress:
   Entramos a la carpeta temp con <code> cd /temp </code>
   Una vez dentro ejecutamos los siguientes comandos:
   ```bash
   wget https://wordpress.org/latest.tar.gz
   tar -xzf latest.tar.gz
   ```

   Luego descomprimimos el contenido de la carpeta de la descarga en el directorio del sitio con
   ```bash
   sudo cp -r wordpress/* /var/www/centro.intranet/
   sudo chown -R www-www-data /var/www/centro.intranet/
   ```

2) Creamos la base de datos para WordPress:
   Para eso entramos a mysql con <code> sudo mysql </code>
   Dentro ejecutamos el siguiente comando:
   ```bash
   CREATE DATABASE wordpress;
   CREATE USER 'centrointranet'@'localhost' IDENTIFIED BY 'genesis';
   GRANT ALL PRIVILEGES ON wordpress.* TO 'centrointranet'@'localhost';
   FLUSH PRIVILEGES;
   ```

### 4. Activar el módulo “wsgi” para permitir la ejecución de aplicaciones Python





### 5. Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente.





### 6. Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación





### 7. Instala y configura awstat.






### 8. Instala un segundo servidor de tu elección (nginx, lighttpd) bajo el dominio “servidor2.centro.intranet”. Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar php. Instala phpmyadmin.





