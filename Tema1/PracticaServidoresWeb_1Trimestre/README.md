# 🖥️ Práctica 1º Trimestre – Servidores Web  
## 🏫 Instalar un servidor web interno para un instituto


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


#### 3.2 Configurar VirtualHost en Apache
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
   
   ![Imagen 3_4_1](/recursos/tema1/practica/3_4_1.png)
   
   Luego movemos el contenido de la carpeta descomprimida en el directorio del sitio con
   ```bash
   sudo mv wordpress/* /var/www/centro.intranet
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

   ![Imagen 3_4_2](/recursos/tema1/practica/3_4_2.png)
   
3) Configurar el archivo wp-config.php de WordPress
   Este archivo contiene los detalles de configuración base para WordPress, incluida la conexión a la base de datos. Para editarlo tenemos que ejecutar los siguientes comandos:
   
   ```bash
   cd /var/www/centro.intranet
   sudo cp wp-config-sample.php wp-config.php
   sudo nano wp-config.php
   ```
   
   Configuramos el archivo con los datos que configuramos en mysql: la base de datos, el usuario, contraseña, etc, tal y como vemos en la imagen. 
   
   ![Imagen 3_4_3](/recursos/tema1/practica/3_4_3.png)

   Ahora tenemos que añadir claves de seguridad ya que WordPress necesita unas claves 'salts'. Para generarlas abrimos otra terminal y ejecutamos:
   ```bash
   curl -s https://api.wordpress.org/secret-key/1.1/salt/
   ```
   
   ![Imagen 3_4_4](/recursos/tema1/practica/3_4_4.png)

   Copiamos todas las líneas que salgan, buscamos en el archivo wp-config.php la sección que dice define( 'AUTH_KEY', '...' ); y las líneas siguientes y la sustituimos por las líneas generadas.

   ![Imagen 3_4_5](/recursos/tema1/practica/3_4_5.png)

4) Configurar los permisos correctos
   Ejecutamos estos comandos: 
   ```bash
   sudo chown -R www-data:www-data /var/www/centro.intranet
   sudo chmod 755 /var/www/
   ```
   
   ![Imagen 3_4_6](/recursos/tema1/practica/3_4_6.png)

  
5) Reiniciar apache


#### 3.5 Terminar de instalar Wordpress 

Abrimos el navegador y entramos en http://centro.intranet donde se nos abrirá WordPress par terminar de instalarlo. 
![Imagen 3_5_1](/recursos/tema1/practica/3_5_1.png)

Una vez instalado accedemos a la página web en http://centro.intranet y nos aparecerá la página web. 
![Imagen 3_5_2](/recursos/tema1/practica/3_5_2.png)
   

### 4. Configurar el sitio departamentos.centro.intranet 
#### 4.1 Activar el módulo WSGI para Python
Lo instalamos con 
```bash
sudo apt install libapache2-mod-wsgi-py3
sudo systemctl restart apache2
```
![Imagen 4_1](/recursos/tema1/practica/4_1.png)


#### 4.2 Crear el directorio para el sitio
Al igual que hicimos con el sitio centro.intranet tenemos que crear un directorio para este sitio
```bash
sudo mkdir -p /var/www/departamentos.centro.intranet
```

#### 4.3 Configurar VirtualHost en Apache
Creamos el archivo de configuración del sitio con 
```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```

![Imagen 4_2](/recursos/tema1/practica/4_2.png)

El contenido del archivo será el siguiente:
```bash
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    DocumentRoot /var/www/departamentos.centro.intranet

    WSGIScriptAlias / /var/www/departamentos.centro.intranet/app.py

    <Directory /var/www/departamentos.centro.intranet>
        Require all granted
    </Directory>
</VirtualHost>
```

![Imagen 4_3](/recursos/tema1/practica/4_3.png)


#### 4.4 Activar el módulo “wsgi” y crear el archivo .py para ejecutar el sitio en Python
1) Crear el archivo app.py
   ```bash
   sudo nano /var/www/departamentos.centro.intranet/app.py
   ```
   ![Imagen 4_4_1](/recursos/tema1/practica/4_4_1.png)
   
2) Contenido del script
   ```bash
   def application(environ, start_response):
    status = '200 OK'
    output = b"Aplicación Python WSGI funcionando desde app.py"

    headers = [('Content-Type', 'text/plain'),
               ('Content-Length', str(len(output)))]

    start_response(status, headers)
    return [output]
   ```

   ![Imagen 4_4_2](/recursos/tema1/practica/4_4_2.png)

3) Activar el módulo wsgi, el sitio y asignar permisos necesarios
   Ejecutamos los siguientes comandos
   ```bash
   sudo a2ensite departamentos.centro.intranet.conf
   sudo a2enmod wsgi
   sudo chown -R www-data:www-data /var/www/departamentos.centro.intranet.conf
   sudo systemctl reload apache2
   ```

   ![Imagen 4_4_3](/recursos/tema1/practica/4_4_3.png)

#### 4.5 Comprobar el funcionamiento del sitio 
Abrimos el navegador y entramos en http://departamentos.centro.intranet. 

![Imagen 4_5](/recursos/tema1/practica/4_5.png)

Como vemos, podemos ver la página con el mensaje que configuramos en el script de python. 


### 5. Proteger el acceso a la aplicación python mediante autenticación
Primero creamos el usuario con su contraseña
```bash
sudo htpasswd -c /etc/apache2/.htpasswd admin
```

![Imagen 5_1](/recursos/tema1/practica/5_1.png)

A continuación editamos el archivo con la configuración VirtualHost departamentos.centro.intranet.conf (que creamos anteriormente).
En la sección Directory agregamos las siguientes líneas:
```bash
AuthType Basic
AuthName "Sitio protegido con Autenticacion"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```

![Imagen 5_2](/recursos/tema1/practica/5_2.png)

Reiniciamos el servidor con <code> sudo systemctl restar apache2 </code> para hacer efectivos los cambios.

Ahora volvemos a entrar en la página y como vemos pide el usuario y la contraseña

![Imagen 5_3](/recursos/tema1/practica/5_3.png)


### 6. Instalar y configurar awstat.
Empezamos instalando awstat con 
```bash
sudo apt install awstats
```

![Imagen 6_1](/recursos/tema1/practica/6_1.png)

Copiamos el archivo awstats.conf que viene por defecto en la carpeta awstats, le ponemos el nombre awstats.centro.intranet.conf y lo editamos
```bash
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
sudo nano /etc/awstats/awstats.centro.intranet.conf
```

En el archivo cambiamos las siguientes líneas: 
```bash
LogFile="/var/log/apache2/access.log"
SiteDomain="centro.intranet"
HostAliases="localhost 127.0.0.1 www.centro.intranet" 
```

![Imagen 6_2](/recursos/tema1/practica/6_2.png)

![Imagen 6_3](/recursos/tema1/practica/6_3.png)

![Imagen 6_4](/recursos/tema1/practica/6_4.png)


Ahora actualizamos las estadísticas:
```bash
sudo /usr/lib/cgi-bin/awstats.pl -config=centro.intranet -update
```

![Imagen 6_5](/recursos/tema1/practica/6_5.png)

Habilitamos el CGI en Apache y reiniciamos el servidor
```bash
sudo a2enmod cgi
sudo systemctl restart apache2
```

![Imagen 6_6](/recursos/tema1/practica/6_6.png)

Abrimos el navegador y entramos en http://centro.intranet/cgi-bin/awstats.pl?config=centro.intranet para ver las estadísticas 

### 7. Instalar un segundo servidor(nginx, lighttpd) bajo el dominio “servidor2.centro.intranet” usando el puerto 8080 con php y phpmyadmin.

#### 7.1 Instalar el servidor Nginx 
En mi caso, usaré el servidor Nginx 
```bash
sudo apt install nginx
```
![Imagen 7_1](/recursos/tema1/practica/7_1.png)

#### 7.2 Crear el directorio para el sitio
Como ya hemos hecho anteriormente tenemos que crear un directorio en /var/www/
```bash
sudo mkdir -p /var/www/servidor2.centro.intranet
```

#### 7.3 Crear el archvivo de configuración para el sitio y configurarlo para que escuche por el puerto 8080
```bash
sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
```

El contenido del archivo es el siguiente:
```bash
server {
    listen 8080;
    server_name servidor2.centro.intranet;

    root /var/www/servidor2;

    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
    }
}
```

![Imagen 7_3](/recursos/tema1/practica/7_3.png)

#### 7.4 Activar el sitio, dar permisos y reiniciar el servidor Nginx
```bash
sudo ln -s /etc/nginx/sites-available/servidor2 /etc/nginx/sites-enabled/
sudo chown -R www-data:www-data /var/www/servidor2.centro.intranet
sudo systemctl restart nginx
```

Al reiniciar el servidor Nginx me dió un error:

![Imagen 7_4_1](/recursos/tema1/practica/7_4_1.png)

Eso era porque en la carpeta /etc/nginx/sites-enabled/ hay un fichero de configuración llamado default. Este escucha también por el puerto 80, que era el error que me daba. 
Lo que hice fue eliminarlo con <code> sudo rm /etc/nginx/sites-enabled </code> aunque también podemos comentar la línea Listen 80 del fichero. 

![Imagen 7_4_2](/recursos/tema1/practica/7_4_2.png)

Volvemos a reinciar el servidor y ya no nos sale error: 

![Imagen 7_4_2](/recursos/tema1/practica/7_4_3.png)


#### 7.5 Instalar PHP-FPM para Nginx
```bash
sudo apt install php-fpm 
sudo systemctl restart php*-fpm
```

![Imagen 7_5_1](/recursos/tema1/practica/7_5_1.png)

![Imagen 7_5_2](/recursos/tema1/practica/7_5_2.png)


#### 7.6 Instalar phpmyadmin para Nginx
```bash
sudo apt install phpmyadmin
```
Cuando se nos abra una ventana donde nos pida seleccionar el servidor elegimos apache2. 

A la hora de configurar o no la base de datos yo puse que no

![Imagen 7_6](/recursos/tema1/practica/7_6.png)

#### 7.7 Configurar phpmyadmin para Nginx y reiniciar el servidor
```bash
sudo ln -s /usr/share/phpmyadmin /var/www/servidor2/phpmyadmin
sudo systemctl restart nginx
```

![Imagen 7_7](/recursos/tema1/practica/7_7.png)

#### 7.8 Entrar a phpmyadmin
En el navegador entramos en http://servidor2.centro.intranet:8080/phpmyadmin

![Imagen 7_8](/recursos/tema1/practica/7_8.png)

Como vemos entramos perfectamente

