# Práctica 2º Trimestre - Servidor alojamiento web
## Servidor alojamiento web
Se pide la instalación, configuración y puesta en marcha de un servidor que ofrezca servicio de alojamiento web configurable: 

Se dará alojamiento a páginas web tanto estáticas como dinámicas con “php”
- Los clientes dispondrán de un directorio de usuario con una página web por defecto. 
- Además contarán con una base de datos sql que podrán administrar con phpmyadmin
- Los clientes podrán acceder mediante ftp para la administración de archivos configurando adecuadamente TLS
- Se habilitará el acceso mediante ssh y sftp. 
- Se automatizará mediante el uso de scripts:
  - La creación de usuarios y del directorio correspondiente para el alojamiento web
  - Host virtual en apache
  - Creación de usuario del sistema para acceso a ftp, ssh, smtp, …
  - Se creará un subdominio en el servidor DNS con las resolución directa e inversa
  - Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)
  - Se habilitará la ejecución de aplicaciones Python con el servidor web 

Adicionalmente se podrá incluir:
- Creación mediante mediante Docker de un contenedor DNS y al menos un contenedor que actuará como servidor (web, mysql, ssh,...) Se configurará la red, volúmenes y scripts necesarios para ponerlos en marcha. Este apartado se valorará con hasta el 10% de la nota de la práctica.

Para esta práctica haremos uso de una máquina con Ubuntu 24 pero se puede realizar perfectamente en Ubuntu Server 24 o en una instancia de AWS. 

### 1. Instalación de servicios
Antes que nada, actualizamos el sistema con los siguientes comandos:
```bash
sudo apt update && sudo apt upgrade -y
```

Recuerda que todos los comandos tienen que empezar por sudo, para poder tener privilegios de administrador y porder ejecutarlos. 

![Imagen 1_1](/recursos/tema2/practica_2trimestre/1_1.png)

#### Servidor web Apache
Si no lo tenemos instalado lo hacemos con este comando: 
```bash
sudo apt-get install apache2
```

En mi caso, ya estaba instalado

![Imagen 1_2](/recursos/tema2/practica_2trimestre/1_2.png)

Para comprobar su funcionamiento accedemos a la IP del servidor desde nuestro navegador:

![Imagen 1_3](/recursos/tema2/practica_2trimestre/1_3.png)

#### Servidor mysql
Seguimos instalando mysql 

```bash
sudo apt mysql-server -y
```

![Imagen 1_4](/recursos/tema2/practica_2trimestre/1_4.png)

#### Instalar PHP
PHP interpreta el código de nuestra aplicación, lo que le permite mostrarse en el navegador.

```bash
sudo apt install php libapache2-mod-php php-mcrypt php-mysql php-cgi php-curl php-json
```

![Imagen 1_5](/recursos/tema2/practica_2trimestre/1_5.png)

Para comprobar que PHP funciona correctamente ejecutamos el siguiente comando

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

Con esto creamos un archivo de prueba que nos muestra la versión de PHP. Para verlo abrimos el navegador y accedemos a nuestra IP más el nombre del archivo: 
http://127.0.0.1/info.php

![Imagen 1_6](/recursos/tema2/practica_2trimestre/1_6.png)

#### Instalar phpMyAdmin

```bash
sudo apt install phpmyadmin -y
```
En la pantalla que nos salta a continuación nos preguntan sobre el servidor web que queremos elegir. Como nosotros hemos instalado apache, marcamos apache2 (lo seleccionamos pulsando la tecla Espacio). Para dar Aceptar nos movemos con Tab

![Imagen 1_5](/recursos/tema2/practica_2trimestre/1_7.png)

Luego nos preguntarán si queremos configurar la base de datos así que nosotros hacemos Intro en Sí.

![Imagen 1_6](/recursos/tema2/practica_2trimestre/1_7_1.png)

Nos pedirá una contraseña para entrar a la administración de phpmyadmin así que la introducimos. Luego tendremos que confirmarla. 

![Imagen 1_7](/recursos/tema2/practica_2trimestre/1_7_2.png)

Para verificar que se ha instalado correctamente accedemos a la dirección http://IP-publica/phpmyadmin. 
El usuario es phpmyadmin

![Imagen 1_8](/recursos/tema2/practica_2trimestre/1_8.png)

#### Habilitar SSH

```bash
sudo apt install openssh-server
```

![Imagen 1_9](/recursos/tema2/practica_2trimestre/1_9.png)

Una vez instalado activamos el servicio con 

```bash
sudo systemctl start ssh
```

Y verificamos su estado activo con 
```bash
sudo systemctl status ssh
```

![Imagen 1_9_1](/recursos/tema2/practica_2trimestre/1_9_1.png)

#### Instalar FTP para la administración de archivos configurando TLS
```bash
sudo apt install vsftpd
```

![Imagen 1_10](/recursos/tema2/practica_2trimestre/1_10.png)

Ahora generamos un certificado para cifrar la conexión y la transferencia de archivos. 
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem
```

![Imagen 1_10_1](/recursos/tema2/practica_2trimestre/1_10_1.png)

Al ejecutarlo nos pedirán unos datos que podemos dejar en blanco menos el Common Name donde tenemos que poner la IP de nuestro servidor. 
Editamos el archivo de configuración: 
```bash
sudo nano /etc/vsftpd.conf
```

Y ponemos estas líneas o las descomentamos:
```bash
write_enable=YES
local_enable=YES
chroot_local_user=YES
ssl_enable=YES
allow_writeable_chroot=YES
```

![Imagen 1_10_2](/recursos/tema2/practica_2trimestre/1_10_2.png)

- local_enable: permite que los usuarios que tienen una cuenta real en el sistema Linux
- write_enable: habilita los comandos de escritura como subir archivos, crear carpetas o borrar archivos.
- chroot_local_user: controla que el usuario solo navegue en su propio directorio personal.
- ssl_enable: activa el soporte para cifrado TLS/SSL







