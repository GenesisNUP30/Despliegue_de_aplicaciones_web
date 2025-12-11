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


### 3. Configurar el sitio centro.intranet

### 3.1 Crear el directorio para el sitio
Hay que crear una carpeta en /var/www/ y para eso hacemos: 
```bash
sudo mkdir -p /var/www/centro.intranet
```

![Imagen 3_1](/recursos/tema1/practica/3_1.png)


#### 3.2. Configurar VirtualHost en Apache
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

![Imagen 3_2](/recursos/tema1/practica/3_2.png)

#### 3.3 Habilitar el sitio
Para que el sitio funcione tenemos que habilitarlo con: 
```bash
sudo a2ensite centro.intranet.conf
```

Y reiniciamos Apache con <code>sudo systemctl reload apache2</code>

![Imagen 3_3](/recursos/tema1/practica/3_3.png)

#### 3.4 Descargar WordPress y configurar el archivo de configuración

1) Descargar Wordpress:
   Entramos a la carpeta temp con <code> cd /tmp </code>
   Una vez dentro ejecutamos los siguientes comandos para descargar y descomprimir:
   ```bash
   wget https://wordpress.org/latest.tar.gz
   tar -xzf latest.tar.gz
   ```
   
   ![Imagen 3_4](/recursos/tema1/practica/3_4.png)
   
   Luego movemos el contenido de la carpeta descomprimida en el directorio del sitio con
   ```bash
   sudo mv wordpress /var/www/centro.intranet
   ```

2) Crear la base de datos para WordPress:
   Para eso entramos a mysql con <code> sudo mysql </code>
   Dentro ejecutamos los siguientes comandos:
   ```bash
   CREATE DATABASE wordpress;
   CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'wordpress';
   GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

   ![Imagen 3_5](/recursos/tema1/practica/3_5.png)
   
3) Configurar el archivo wp-config.php de WordPress
   Este archivo contiene los detalles de configuración base para WordPress, incluida la conexión a la base de datos. Para editarlo tenemos que ejecutar los siguientes comandos:
   
   ```bash
   cd /var/www/centro.intranet
   sudo cp wp-config-sample.php wp-config.php
   sudo nano wp-config.php
   ```
   
   Configuramos el archivo con los datos que configuramos en mysql: la base de datos, el usuario, contraseña, etc, tal y como vemos en la imagen. 
   
   ![Imagen 3_6](/recursos/tema1/practica/3_6.png)

   Ahora tenemos que añadir claves de seguridad ya que WordPress necesita unas claves 'salts'. Para generarlas abrimos otra terminal y ejecutamos:
   ```bash
   curl -s https://api.wordpress.org/secret-key/1.1/salt/
   ```
   
   ![Imagen 3_7](/recursos/tema1/practica/3_7.png)

   Copiamos todas las líneas que salgan, buscamos en el archivo wp-config.php la sección que dice define( 'AUTH_KEY', '...' ); y las líneas siguientes y la sustituimos por las líneas generadas.

   ![Imagen 3_8](/recursos/tema1/practica/3_8.png)

4) Configurar los permisos correctos
   Ejecutamos estos comandos: 
   ```bash
   sudo chown -R www-data:www-data /var/www/centro.intranet
   sudo chmod -R u=rwX,go=rX /var/www/centro.intranet
   ```
   
   ![Imagen 3_9](/recursos/tema1/practica/3_9.png)
   
   - u=rwX → el propietario (www-data) tiene lectura, escritura y ejecución (X solo en carpetas)
   - go=rX → grupo y otros tienen lectura y ejecución (solo en carpetas)
  
5) Reiniciar apache


#### 3.4 Terminar de instalar Wordpress 

Abrimos el navegador y entramos en http://centro.intranet donde nos encontraremos la carpeta de wordpress.
Entramos a la capeta y ya podemos continuar con la instalación
   

### 4. Activar el módulo “wsgi” para permitir la ejecución de aplicaciones Python





### 5. Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente.





### 6. Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación





### 7. Instala y configura awstat.






### 8. Instala un segundo servidor de tu elección (nginx, lighttpd) bajo el dominio “servidor2.centro.intranet”. Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar php. Instala phpmyadmin.





