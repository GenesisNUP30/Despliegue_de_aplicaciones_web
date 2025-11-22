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
En el primero niega el acceso a todos y luego permite solo desde 192.168.1.100. En el segundo primero se permite desde 192.168.1.100, luego niega todo (porque Deny from All se evalúa al final).
Esa es la configuración antigua de Apache, combinando Allow con Deny. Con Require sería diferente. 




