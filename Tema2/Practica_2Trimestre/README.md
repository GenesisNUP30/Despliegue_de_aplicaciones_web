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

### 1. Crear la instancia EC2 en AWS
Para ello seguiremos los siguientes pasos:
1. Entramos en la página de AWS
2. En la barra de búsqueda superior escribimos EC2 y pulsamos en la primera opción que dice Servidores virtuales en la nube. Podemos añadirla a Favoritos pulsando en la estrella y así no tendremos que buscarla la próxima vez
   
   ![Imagen 1](/recursos/tema2/practica_2trimestre/1.png)
   
3. Hacemos clic en el botón Lanzar una instancia para abrir el asistente de creación de instancias.
   ![Imagen 2](/recursos/tema2/practica_2trimestre/2.png)
   
   A continuación tenemos que configurar:
   - El nombre
     
     ![Imagen 2_1](/recursos/tema2/practica_2trimestre/2_1.png)
     
   - La imagen de Amazon Machine (AMI):
     Dejamos la que está por defecto: AMI de Amazon Linux 2023 kernel-6.1
     
     ![Imagen 2_2](/recursos/tema2/practica_2trimestre/2_2.png)
     
   - El tipo de instancia EC2. Un tipo de instancia es una configuración particular de CPU, memoria (RAM), almacenamiento y capacidad de red.
     Elegimos t3.micro que es la que viene por defecto
     
     ![Imagen 2_3](/recursos/tema2/practica_2trimestre/2_3png)

   - Creamos un par de claves para la instancia y el asignamos un nombre. Es muy importante no perderlas pues la usaremos para poder iniciar la conexión SSH

     ![Imagen 2_4](/recursos/tema2/practica_2trimestre/2_4png)

     Cuando se genere la descarguemos con extensión .pem y la guardamos en un lugar seguro.

   - 

