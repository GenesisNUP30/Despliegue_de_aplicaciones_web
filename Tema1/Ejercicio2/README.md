# EJERCICIO 2.1 - Configuración de Apache

## 1. Hacer que Apache escuche por el puerto 81
Tenemos que editar el archivo de configuración de puertos de Apache **ports.conf**, que especifica los puertos en los que el servidor escuchará las peticiones entrantes. Para ello usamos el siguiente comando:
```bash
sudo nano /etc/apache2/ports.conf
```
El comando **nano** se usa para editar un archivo que tenga permisos de escritura, como es en este caso. 

Al introducir el comando, se nos abrirá el archivo ports.conf, que podremos editar. Por defecto el servidor Apache usa el puerto 80 para escuchar las peticiones http y eso lo sabemos por la línea que viene por defecto **Listen 80**. 
Por lo tanto, para hacer que Apache también use el puerto 81 añadiremos **Listen 81** debajo de Listen 80. 

![Imagen 1](/recursos/tema1/ejercicio2/puerto81.png)


Para guardar los cambios hacemos Ctrl + O y para salir Ctrl + X. 

Ahora, reiniciamos el servidor Apache para que se hagan efectivos los cambios. Para ello usamos el comando 
```bash
sudo service apache2 restart
```
A partir de ahora, podemos usar el puerto 81 escribiendo en el navegador **localhost:81** o **127.0.0.1:81**. Se abrirá la página por defecto de Apache como se ve en la foto con la diferencia de que estaremos el puerto 81. 

![Imagen 2](/recursos/tema1/ejercicio2/puerto81_1.png)


## 2. Añadir el dominio “marisma.intranet” en el fichero “hosts”
Al agregar este dominio podremos acceder a nuestra página escribiendo marisma.intranet en el navegador. Por eso, tenemos que editar el fichero **/etc/hosts**, que se encarga de asociar nombres de dominio con direcciones IP. 

El comando que usamos es: 
```bash
sudo nano /etc/hosts
```

Una vez que se nos abra el fichero veremos un contenido parecido a este:

![Imagen 3](/recursos/tema1/ejercicio2/dominio1.png)

En mi caso, para mi dirección IP 127.0.0.1 (que es el servidor de mi equipo local) tengo asociado el nombre de dominio **locahost** por eso puedo escribir en el navegador tanto 127.0.0.1 como localhost. 

Como queremos asociar el nuevo nombre de dominio marisma.intranet a nuestro servidor vamos a agregar la siguiente línea: 
**127.0.0.1   marisma.intranet**. 

Debería quedar así: 

![Imagen 4](/recursos/tema1/ejercicio2/dominio2.png)

Ahora escribimos en el navegador **http://marisma.intranet** y nos saldrá el servidor Apache: 

![Imagen 5](/recursos/tema1/ejercicio2/dominio3.png)


## 3. Cambia la directiva “ServerTokens” para mostrar el nombre del producto.
Esta directiva controla la cantidad de información sobre el servidor que se incluye en la cabecera de la respuesta HTTP y así aumentar la seguridad. El archivo donde se encuentra es security.conf. 

Para acceder a él introducimos este comando: 
```bash
sudo nano /etc/apache2/conf-available/security.conf
```

Al abrir el archivo vemos esta línea: 

![Imagen 6](/recursos/tema1/ejercicio2/servertokens1.png)


**OS** significa que en la cabecera veremos la versión de Apache y el sistema operativo sobre el que se está ejecutando. Para ver esta información hay 2 formas que podemos usar:
- Desde el navegador:
  1. Abre localhost
  2. Pulsa **F12** y entra en **Red (Network)**
  3. Recarga la página
  4. Pulsa en una petición, ve al apartado **Cabecera de la respuesta (Response header)** y fijate la línea **Server**
 
  ![Imagen 7](/recursos/tema1/ejercicio2/servertokens2.png)

- Desde la terminal con el comando:
  ```bash
  curl -I http://localhost
  ```
  La información que nos saldrá en **Server** será la misma que en el navegador:

  ![Imagen 8](/recursos/tema1/ejercicio2/servertokens3.png)


Entonces, para mostrar el nombre de producto cambiamos **OS** por **Prod** tal y como se ve en la imagen:

![Imagen 9](/recursos/tema1/ejercicio2/servertokens4.png)

Guardamos y cerramos el archivo y reiniciamos Apache. Si volvemos al navegador ahora solo aparecerá **Apache**: 

![Imagen 10](/recursos/tema1/ejercicio2/servertokens5.png)

Y lo mismo nos aparecerá si lo vemos desde la terminal: 

![Imagen 11](/recursos/tema1/ejercicio2/servertokens6.png)


## 4. Pie de página en páginas generadas por Apache (directiva "ServerSignature")
Esta directiva controla si se añade o no un pie de página con información del servidor, como su nombre y versión, en los documentos que genera automáticamente (por ejemplo, en los mensajes de error). 

Para ver si lo tenemos activado o no vamos a abrir una página que no exista como **http://localhost/holamundo**. Nos saltará una página de error 404 y, si la directiva está activada, nos saldrá el pie de página como en la imagen: 

![Imagen 12](/recursos/tema1/ejercicio2/serversignature1.png)

En mi caso sí está activada. 

El archivo donde se encuentra esta directiva también es security.conf por lo que para abrirlo introducimos:
```bash
sudo nano /etc/apache2/conf-available/security.conf
```

Vemos que pone **ServerSignature On**, lo que corrobora lo que ya sabíamos: que la directiva **ServerSignature** está activada. Si quisiéramos desactivarla cambiaríamos **On** por **Off**. 

Al hacerlo ya no saldría el pie de página en la página de error:

![Imagen 13](/recursos/tema1/ejercicio2/serversignature2.png)

  
## 5. Crear 2 directorios de prueba

Todas las páginas del servidor se alojan en el directorio **/var/www/html/**, que es la carpeta raíz por defecto de estos archivos. Por lo tanto, para crear los 2 directorios vamos a movermos a este directorio con 
```bash
cd /var/www/html/
```
Una vez dentro vamos a crear 2 carpetas, una para cada página, y así las tenemos separadas con sus respectivos archivos. Para ello usamos la orden **mkdir** que es el comando de Ubuntu que se usa para crer directorios o carpetas: 
```bash
sudo mkdir prueba prueba2
```

![Imagen 14](/recursos/tema1/ejercicio2/directorio1.png)

Como se puede ver tenemos los 2 directorios creados

Ahora vamos a crear un index para que cuando se abra la página se muestre un mensaje. Hacemos: 
```bash
sudo nano prueba/index.html
```
Al hacer directamente **nano** crea el archivo si no existe previamente.

Cuando se nos abrá el editor lo único que tenemos que hacer es crear una estructura html sencilla para mostrar un mensaje. Yo he puesto esto:
```bash
<html>
<h1>Página de prueba</h1>
</html>
```

![Imagen 16](/recursos/tema1/ejercicio2/directorio2.png)

Guardamos y cerramos el archivo. 

Lo mismo hacemos con la otra página prueba2, lo único que su contenido algo distinto para poder diferenciarlas

Para poder tener varias páginas dentro de cada una de estos directorios tenemos que crear tantos archivos html como páginas querramos tener. Por ejemplo, ejemplo1.html y ejemplo2.html en cada directorio. 
Esto lo hacemos con el mismo comando de antes: 
```bash
sudo nano prueba/ejemplo1.html
```

Volvemos a ponerle contenido como:
```bash
<html>
  <h1>Esto es ejemplo1 dentro del directorio prueba</h1>
</html>
```

![Imagen 16](/recursos/tema1/ejercicio2/directorio3.png)

Usamos el mismo comando para prueba2 pero con otro nombre de fichero, como ejemplo2.html

Por último vamos a comprobar que nuestras páginas se ven desde el navegador. Para ver prueba:
- **http://localhost/prueba/**
  
  ![Imagen 17](/recursos/tema1/ejercicio2/directorio4.png)

- **http://localhost/prueba/ejemplo1.html**
  
  ![Imagen 18](/recursos/tema1/ejercicio2/directorio5.png)

Probamos la otra página prueba2 haciendo:
- **http://localhost/prueba2/**
  
  ![Imagen 19](/recursos/tema1/ejercicio2/directorio6.png)

- **http://localhost/prueba2/ejemplo2.html**
  
  ![Imagen 20](/recursos/tema1/ejercicio2/directorio7.png)


## 6. Redireccionar el contenido de la carpeta “prueba” hacia “prueba2”
Para eso debemos editar el archivo de configuracion **000-default.conf** que contiene la configuración de nuestro sitio web de Apache. El comando sería: 
```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Una vez dentro vamos a usar la directiva **Redirect**, que como bien indica su nombre, sirve para redireccionar automáticamente una solicitud web de una URL a otra. 
Dentro del bloque <VirtualHost *:80>, se añade:
**Redirect permanent /prueba http://localhost/prueba2**

![Imagen 21](/recursos/tema1/ejercicio2/redirect1.png)

Yo he usado **permanent** porque así indico que la redirección es de forma permanente. Una vez guardado el cambio reiniciamos el servidor. 

Ahora en el navegador escribimos **http://localhost/prueba** como se ve en la foto

![Imagen 22](/recursos/tema1/ejercicio2/redirect2.png)

Al hacer intro podemos observar como nos redirige a prueba2 (también lo podemos ver en la URL que ahora pone **/prueba2**)

![Imagen 23](/recursos/tema1/ejercicio2/redirect3.png)


## 7. Redireccionar tan solo una página en lugar de toda la carpeta. 

Volvemos a editar el archivo **000-default.conf** como en el ejercicio anterior pero ahora con una ruta distinta para indicar que redirigimos una página

**Redirect permanent /prueba/ejemplo1.html http://localhost/prueba2/ejemplo2.html**

![Imagen 24](/recursos/tema1/ejercicio2/redirect4.png)

Guardamos los cambios, cerramos el archivo y reiniciamos el servidor. Ahora probamos la redirección escribiendo en la URL **http://localhost/prueba2/ejemplo2.html** y como vemos se efectua correctamente: 

![Imagen 25](/recursos/tema1/ejercicio2/redirect5.png)


## 8. Usar la directiva userdir
Esta directiva permite que cada usuario de un sistema pueda tener su propio sitio web alojado en una subcarpeta dentro de su directorio personal como http://localhost/~usuario/.
Para ponerla en práctica debemos:

1. Activar el módulo userdir
   ```bash
   sudo a2enmod userdir
   ```
2. Crear el directorio para el usuario (en mi caso genesis)
   ```bash
   mkdir ~/public_html
   echo "<html><h1>Mi sitio personal usando la directiva userdir</h1></html>" > ~/public_html/index.html
   ```

   ![Imagen 26](/recursos/tema1/ejercicio2/userdir1.png)

   Se tiene que llamar public_html porque al activar el módulo **userdir** Apache espera que los sitios personales estén en una carpeta llamada public_html dentro del directorio de cada usuario.
   El echo sirve para que imprime el mensaje por pantalla y redirige la salida al archivo index.html, que como no existe lo crea en el momento.

3. Reinciar Apache


## 9. Usar la directiva alias para redireccionar a una carpeta dentro del directorio de usuario.
1. Creamos la carperta y su contenido:
   ```bash
   mkdir ~/ejercicio9
   echo "<html><h1>Este es el ejercicio de la directiva alias</h1></html>" > ~/ejercicio9/index.html
   ```
   ![Imagen 27](/recursos/tema1/ejercicio2/alias1.png)
   
2. Añadimos la directiva **Alias** en la configuración de Apache
   ```bash
   sudo nano /etc/apache2/sites-available/000-default.conf
   ```

   Dentro del archivo añadimos:
   ```bash
   Alias /misarchivos /home/genesis/ejercicio9
   <Directory /home/genesis/ejercicio9>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
   </Directory>
   ```

   Con **Alias /misarchivos /home/genesis/ejercicio9** consigo redireccionar una URL a una carpeta de mi sistema. 

   ![Imagen 27](/recursos/tema1/ejercicio2/alias2.png)

   
3. Reiniciamos el servidor apache

Si hacemos **localhost/misarchivos** nos sale el contenido de ejercicio9: 

![Imagen 28](/recursos/tema1/ejercicio2/alias3.png)
   

## 10. ¿Para qué sirve la directiva Options y dónde aparece? Comprueba si apache indexa los directorios. Si es así, ¿cómo lo desactivamos?
- **Options**: controla qué características están habilitadas en un directorio.
  Aparece en archivos de configuración del servidor web de Apache dentro de directivas como Directory o Files o en ficheros **.htaccess**.
- Para saber si Apache indexa o no, accedemos a una URL de un directorio que no contenga un archivo de índice (como index.html).
  En mi caso, hice un directorio con 2 archivos .txt dentro. Si está activado, nos debería salir una lista con los archivos que hay en ese directorio.

  ![Imagen 29](/recursos/tema1/ejercicio2/indexado1.png)

  Como vemos, está activado. Para quitarlo entraríamos en la configuración del sitio que queremos editar y modificaríamos la línea Directory de esta manera:
  ```bash
  <Directory /var/www/html>
    Options -Indexes FollowSymLinks
  </Directory>
  ```

  -Indexes quita el indexado.

  Después de los cambios debemos reiniciar Apache. 
  
# EJERCICIO 2.2 - Trabajando con scripts
Para hacer los scripts he creado una carptea en mi directorio personal llamada **scripts-ej2**. Todos los scripts deben tener extensión .sh. 

## 1. Crea un script que añada un puerto de escucha en el fichero de configuración de Apache. El puerto se recibirá como parámetro en la llamada y se comprobará que no esté ya presente en el fichero de configuración.
Dentro de la carpeta creamos el script haciendo
```bash
nano script1.sh
```
El script será el siguiente: 

![Imagen 30](/recursos/tema1/ejercicio2/script1.png)

Guardamos con Ctrl+O y antes de ejecutarlo le damos permisos de ejecución con **chmod 755 script1.sh**. Lo haremos con todos los scripts para asegurarnos de poder ejecutarlos.
Para ejecutar el script hacemos **./nombre_script argumento**. 

![Imagen 30](/recursos/tema1/ejercicio2/script1_2.png)

Como vemos la comprobación funciona pues el puerto 81 ya estaba añadido de la otra práctica pero el puerto 82 si se añadió perfectamente.

## 2. Crea un script que añada un nombre de dominio y una ip al fichero hosts. Debemos comprobar que no existe dicho dominio en el fichero hosts

![Imagen 31](/recursos/tema1/ejercicio2/script2.png)

Le ponemos los permisos de ejecución y ejecutamos. 

![Imagen 31](/recursos/tema1/ejercicio2/script2_2.png)

Como vemos ha añadido correctamente la IP. Hacemos **cat /etc/hosts/** para ver que el contenido del fichero.

## 3. Crea un script que nos permita crear una página web con un título, una cabecera y un mensaje

Primero: 
```bash
nano script3.sh
```
El contenido del script será: 

![Imagen 32](/recursos/tema1/ejercicio2/script3.png)

No olvidemos de darle los permisos de ejecucion antes. 
   
![Imagen 32](/recursos/tema1/ejercicio2/script3_2.png)

Como se puede ver, se ha creado el sitio index.html en la carpeta scripts-ej2. 

