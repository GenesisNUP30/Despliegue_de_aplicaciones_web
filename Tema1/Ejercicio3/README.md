# EJERCICIO 3. Directiva Directory

## 1. Crea un directorio llamado "dir1" y otro llamado "dir2"
Eso lo hacemos con **sudo mkdir dir1**. 

![Imagen 1](/recursos/tema1/ejercicio3/act1.png)

## 2. Explica qué diferencia existe entre ambos y muestra su equivalencia con la directiva Require:
```batch
<Directory /var/www/example1>
Order Deny,Allow
Deny from All
Allow from 192.168.1.100
</Directory>

<Directory /var/www/example1>
Order Allow,Deny
Deny from All
Allow from 192.168.1.100
</Directory>
```
En el primero niega el acceso a todos y luego permite solo desde 192.168.1.100. 
En el segundo primero se permite desde 192.168.1.100, luego niega todo (porque Deny from All se evalúa al final).
Esa es la configuración antigua de Apache, que combina Allow con Deny. 

La nueva sería usando Require y quedaría así: 
- Require 192.168.1.100: solo permite a esa IP
- Require all denied: negaría a todos


## 3. Para dir1
a. Permite el acceso de las peticiones provenientes de 10.3.0.100
Para hacer estas configuraciones editaremos el fichero 000-default.conf usando: 
```batch
sudo nano /etc/apache2/sites-available/000-default.conf
```

![Imagen 2](/recursos/tema1/ejercicio3/act3_1.png)

Guardamos el archivo y reiniciamos el servidor apache con **sudo service apache2 restart**. 


b. Permite el acceso desde "marisma.intranet"
Editamos el mismo archivo de antes de la siguiente manera: 

![Imagen 3](/recursos/tema1/ejercicio3/act3_2.png)

Para que no haga conflicto con la regla anterior, la comentamos con #

c. Permite el acceso desde cualquier subdominio de "marisma.intranet"

![Imagen 4](/recursos/tema1/ejercicio3/act3_3.png)

El punto inicial justo antes del nombre indica subdominios, por eso es importante ponerlo. 

d. Permite el acceso de las peticiones provenientes de "10.3.0.100" con máscara "255.255.0.0"

![Imagen 5](/recursos/tema1/ejercicio3/act3_4.png)

## 4. Modifica la configuración de forma que el acceso a dir1:
Se permita a "marisma.intranet" y no se permita desde 10.3.0.101"

Esto lo hacemos en el mismo fichero que hemos editado en los ejercicios anteriores, 000.default-conf.
Borramos o comentamos las otras reglas Requiere que hemos añadido antes y editamos la directiva Directory de esta manera: 

![Imagen 6](/recursos/tema1/ejercicio3/act4.png)


## 5. Modifica la configuración de forma que el acceso a dir2:
Se permita a "10.3.0.100/8" y no a "marisma.intranet"

![Imagen 6](/recursos/tema1/ejercicio3/act5.png)


No olvidemos que después de hacer estos cambios hay que reinciar el servicio de Apache con **sudo service apache2 restart**. 


