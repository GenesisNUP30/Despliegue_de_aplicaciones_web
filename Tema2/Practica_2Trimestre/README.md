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

#### 1.1 Servidor web Apache
Si no lo tenemos instalado lo hacemos con este comando: 
```bash
sudo apt-get install apache2
```

En mi caso, ya estaba instalado

![Imagen 1_2](/recursos/tema2/practica_2trimestre/1_2.png)

Para comprobar su funcionamiento accedemos a la IP del servidor desde nuestro navegador:

![Imagen 1_3](/recursos/tema2/practica_2trimestre/1_3.png)

#### 1.2 Servidor mysql
Seguimos instalando mysql 

```bash
sudo apt mysql-server -y
```

![Imagen 1_4](/recursos/tema2/practica_2trimestre/1_4.png)

#### 1.3 Instalar PHP
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

#### 1.4 Instalar phpMyAdmin

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

#### 1.5 Habilitar SSH

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

#### 1.6 Instalar FTP para la administración de archivos configurando TLS
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

Por último reinciamos el servicio con 
```bash
sudo systemctl restart vsftpd
```

### 2. Configuración de DNS con las resoluciones directa e inversa
Tenemos que tener creado el archivo para zonas DNS antes de realizar el script. Para este apartado usaré la IP 127.0.0.1. 

Para ello primero instalamos Bind9 con 
```bash
sudo apt install bind9 -y
```
![Imagen 2_1](/recursos/tema2/practica_2trimestre/2_1.png)

Ahora editamos el archivo named.conf.local
```bash
sudo nano /etc/bind/named.conf.local
```

Y añadimos las dos zonas:
```bash
zone "ejemplo.local" {
    type master;
    file "/etc/bind/db.ejemplo.local";
};

zone "0.0.127.in-addr.arpa" {
    type master;
    file "/etc/bind/db.127";
};
```
![Imagen 2_2](/recursos/tema2/practica_2trimestre/2_2.png)

Ahora creamos el archivo donde vinculamos los nombres de dominio a una ip en este caso a 127.0.0.1. 
Primeros hacemos la zona directa (NOMBRE -> IP)
```bash
sudo nano /etc/bind/db.ejemplo.com
```

Y le ponemos esta configuración:

![Imagen 2_3](/recursos/tema2/practica_2trimestre/2_3.png)


Ahora creamos el archivo de la zona indirecta (IP → nombre):
```bash
sudo nano /etc/bind/db.127
```

Y lo configuramos así:

![Imagen 2_4](/recursos/tema2/practica_2trimestre/2_4.png)

Por último nos aseguramos de usar nuestro DNS
```bash
sudo nano /etc/resolv.conf
```

Y editando así la línea: nameserver 127.0.0.1

Después de realizar todas estas configuraciones reinciamos el servicio DNS
```bash
sudo systemctl restart bind9
```

### 3. Soporte Python en Apache
Para instalar Python hacemos
```bash
sudo apt install libapache2-mod-wsgi-py3 python3 -y
```

![Imagen 3_1](/recursos/tema2/practica_2trimestre/3_1.png)

Ahora habilitamos el módulo wgsi con
```bash
sudo a2enmod wsgi
```
Y reinciamos el servicio Apache
```bash
sudo systemctl restart apache2
```

### 4. Automatización con script
Ahora crearemos un script con
```bash
sudo nano crear_cliente.sh
```
Este script realizará las siguientes tareas:
- La creación de usuarios y del directorio correspondiente para el alojamiento web
- Host virtual en apache
- Creación de usuario del sistema para acceso a ftp, ssh, smtp, …
- Se creará un subdominio en el servidor DNS con las resolución directa e inversa
- Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)
- Se habilitará la ejecución de aplicaciones Python con el servidor web 

El contenido de este archivo será el siguiente: 
```bash
#!/bin/bash

# =========================================
# USO: ./crear_cliente.sh usuario [IP]
# =========================================

if [ $# -lt 1 ]; then
    echo "Uso: $0 usuario [IP]"
    exit 1
fi

# =========================================
# VARIABLES
# =========================================

USUARIO=$1
IP=${2:-127.0.0.1}   # si no se pasa IP usa localhost
DOMINIO="ejemplo.com"

SUBDOMINIO="${USUARIO}.${DOMINIO}"
WEB_DIR="/var/www/html/${USUARIO}"

ZONA_DIRECTA="/etc/bind/db.ejemplo.com"
ZONA_INVERSA="/etc/bind/db.127"

APACHE_CONF="/etc/apache2/sites-available/${USUARIO}.conf"

DB_NAME="${USUARIO}_db"
DB_USER="${USUARIO}"
DB_PASS=$(openssl rand -base64 10)

# =========================================
# 1. CREAR USUARIO DEL SISTEMA
# =========================================

echo "Creando usuario del sistema..."

if id "$USUARIO" &>/dev/null; then
    echo "❌ El usuario ya existe"
    exit 1
fi

sudo adduser --disabled-password --gecos "" $USUARIO

# =========================================
# 2. DIRECTORIO WEB
# =========================================

echo "Creando directorio web..."

sudo mkdir -p $WEB_DIR
sudo chown -R $USUARIO:$USUARIO $WEB_DIR
sudo chmod 755 $WEB_DIR

echo "<h1>Bienvenido $USUARIO</h1>" | sudo tee $WEB_DIR/index.html > /dev/null

# =========================================
# 3. DNS - ZONA DIRECTA
# =========================================

echo "Configurando DNS directo..."

if ! grep -q "^$USUARIO" $ZONA_DIRECTA; then
    echo "$USUARIO    IN  A   $IP" | sudo tee -a $ZONA_DIRECTA > /dev/null
fi

# =========================================
# 4. DNS - ZONA INVERSA
# =========================================

echo "Configurando DNS inverso..."

# Eliminamos PTR antiguos (evita duplicados)
sudo sed -i "/IN PTR/d" $ZONA_INVERSA

echo "1    IN PTR $SUBDOMINIO." | sudo tee -a $ZONA_INVERSA > /dev/null

# =========================================
# 5. VIRTUALHOST APACHE
# =========================================

echo "Creando VirtualHost..."

sudo bash -c "cat > $APACHE_CONF" <<EOF
<VirtualHost *:80>
    ServerName $SUBDOMINIO
    DocumentRoot $WEB_DIR

    <Directory $WEB_DIR>
        Require all granted
        AllowOverride All
    </Directory>

    # Python WSGI
    WSGIScriptAlias /app $WEB_DIR/app.wsgi

    ErrorLog \${APACHE_LOG_DIR}/${USUARIO}_error.log
    CustomLog \${APACHE_LOG_DIR}/${USUARIO}_access.log combined
</VirtualHost>
EOF

sudo a2ensite ${USUARIO}.conf > /dev/null

# =========================================
# 6. PYTHON (WSGI)
# =========================================

echo "Configurando aplicación Python..."

cat <<EOF | sudo tee $WEB_DIR/app.wsgi > /dev/null
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b"<h1>Python funcionando para $USUARIO</h1>"]
EOF

sudo chown $USUARIO:$USUARIO $WEB_DIR/app.wsgi

# =========================================
# 7. BASE DE DATOS MYSQL
# =========================================

echo "Creando base de datos..."

sudo mysql <<EOF
CREATE DATABASE $DB_NAME;
CREATE USER '$DB_USER'@'localhost' IDENTIFIED BY '$DB_PASS';
GRANT ALL PRIVILEGES ON $DB_NAME.* TO '$DB_USER'@'localhost';
FLUSH PRIVILEGES;
EOF

# =========================================
# 8. REINICIO SERVICIOS
# =========================================

echo "Reiniciando servicios..."

sudo systemctl reload apache2
sudo systemctl restart bind9

# =========================================
# RESULTADO
# =========================================

echo ""
echo "Cliente creado correctamente"
echo "Web: http://$SUBDOMINIO"
echo "Python: http://$SUBDOMINIO/app"
echo "Base de datos: $DB_NAME"
echo "Usuario DB: $DB_USER"
echo "Password DB: $DB_PASS"
echo ""
```

Una vez guardado el archivo tenemos que darle permisos de ejecución con 
```bash
sudo chmod +x crear_cliente.sh
```
Y ahora probamos la ejecución del script sin especificar IP con 
```bash
sudo ./crear_cliente.sh cliente1
```

Y como podemos se ha creado correctamente:

![Imagen 4_1](/recursos/tema2/practica_2trimestre/4_1.png)

Si queremos ip solo la añadiríamos así:
```bash
sudo ./crear_cliente.sh cliente2 x.x.x.x
```

### 5. Creación de entorno completo usando Docker
Primero tenemos que crear este directorio de carpetas y archivos que usará docker al levantarlo:
```bash
docker-practica/
│
├── docker-compose.yml
├── dns/
│   ├── named.conf.local
│   ├── db.ejemplo.local
│   └── db.127
│
└── web/
    ├── index.php
    └── app.wsgi
```

Como vamos a montar el mismo entorno de servicios que hicimos en Ubuntu también necesitamos los mismos archivos de dns para zona directa e inversa, y los archivos para python. Podemos usar el mismo contenido que los archivos que configuramos antes cambiando el nombre de los dominios. 
Para crear los archivos entramos a la carpeta que los contiene y hacemos *sudo nano nombre_archivo* :

- dns/named.conf.local:
```bash
zone "ejemplo.local" {
    type master;
    file "/etc/bind/db.ejemplo.local";
};

zone "0.0.127.in-addr.arpa" {
    type master;
    file "/etc/bind/db.127";
};
```
- dns/db.ejemplo.local
```bash
$TTL 604800
@   IN  SOA ns.ejemplo.local. admin.ejemplo.local. (
        2
        604800
        86400
        2419200
        604800 )

@   IN  NS  ns.ejemplo.local.
ns  IN  A   127.0.0.1

@   IN  A   127.0.0.1
web IN  A   127.0.0.1
```
- dns/db.127
```bash
$TTL 604800
@   IN  SOA ns.ejemplo.local. admin.ejemplo.local. (
        2
        604800
        86400
        2419200
        604800 )

@   IN  NS  ns.ejemplo.local.

1   IN  PTR web.ejemplo.local.
```
- 


