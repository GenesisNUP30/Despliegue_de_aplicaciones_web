# EJERCICIO 2 - Configuración básica de Apache

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


  

  

  
  






