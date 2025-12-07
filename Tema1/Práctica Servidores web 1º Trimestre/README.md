# Práctica 1º Trimestre. Servidores Web
## Instalar un servidor web interno para un instituto


### 1. Creamos 2 dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. 
#### El primero servirá el contenido mediante wordpress y el segundo una aplicación en python
Para eso editamos el archivo /etc/hosts: 
```bash
sudo nano /etc/hosts
```
Recordemos que para estas operaciones necesitamos permisos de admministrador por eso usamos el usuarios sudo. 
Dentro del archivo añadimos las siguientes líneas:

- 127.0.0.1 centro.intranet
- 127.0.0.1 departamentos.centro.intranet
- 127.0.0.1 servidor2.centro.intranet

![Imagen 1](/recursos/tema1/practica/1.png)

### 2. Instalamos Apache, PHP y MySQL y activamos los módulos necesarios

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
  - <code> php-mysql </code>, un módulo PHP que permite que este se comunique con bases de datos basadas en MySQL. 
  - <code> libapache2-mod-php </code> para permitir que Apache gestione archivos PHP.

Después de hacer todas las configuraciones hacemos:
```bash
sudo systemctl restart apache2
```
Para comprobar que el servidor apache está activo hacemos
```bash
sudo systemctl status apache2
```

![Imagen 1](/recursos/tema1/practica/2.png)



### 3. Instala y configura wordpress





### 4. Activar el módulo “wsgi” para permitir la ejecución de aplicaciones Python





### 5. Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente.





### 6. Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación





### 7. Instala y configura awstat.






### 8. Instala un segundo servidor de tu elección (nginx, lighttpd) bajo el dominio “servidor2.centro.intranet”. Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar php. Instala phpmyadmin.





