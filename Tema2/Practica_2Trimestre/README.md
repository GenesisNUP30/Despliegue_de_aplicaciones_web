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

#### Servidor web (apache y php)
```bash
sudo apt install apache2 php libapache2-mod-php php-mysql -y
```

![Imagen 1_2](/recursos/tema2/practica_2trimestre/1_2.png)

Para comprobar su funcionamiento accedemos a la IP del servidor desde nuestro navegador:

![Imagen 1_3](/recursos/tema2/practica_2trimestre/1_3.png)

#### Servidor mysql

```bash
sudo apt mysql-server -y
```

![Imagen 1_4](/recursos/tema2/practica_2trimestre/1_4.png)

#### Instalar phpMyAdmin

```bash
sudo apt install phpmyadmin -y
```
En la pantalla que nos salta a continuación nos pregutan sobre el servidor web que queremos elegir. Como nosotros hemos instalado apache, marcamos apache2 (lo seleccionamos pulsando la tecla Espacio). Para dar OK nos movemos con Tab

![Imagen 1_5](/recursos/tema2/practica_2trimestre/1_5.png)

Luego nos preguntarán si queremos configurar la base de datos así que nosotros hacemos Intro en yes.

![Imagen 1_6](/recursos/tema2/practica_2trimestre/1_6.png)

Nos pedirá una contraseña para entrar a la administración de phpmyadmin así que la introducimos

![Imagen 1_7](/recursos/tema2/practica_2trimestre/1_7.png)

Tendremos que confirmarla

Para verificar que se ha instalado correctamente accedemos a la dirección http://IP-publica/phpmyadmin. 
El usuario es phpmyadmin

![Imagen 1_8](/recursos/tema2/practica_2trimestre/1_8.png)

#### Instalar FTP para la administración de archivos configurando TLS
```bash
sudo apt install vsftpd
```

Una vez instalado, editamos el archivo de configuración: 
```bash
sudo nano /etc/vsftpd.conf
```






