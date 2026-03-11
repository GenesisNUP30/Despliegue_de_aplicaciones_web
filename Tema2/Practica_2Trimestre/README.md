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
   
   ![Imagen 1_1](/recursos/tema2/practica_2trimestre/1_1.png)
   
3. Hacemos clic en el botón Lanzar una instancia para abrir el asistente de creación de instancias.
   ![Imagen 2](/recursos/tema2/practica_2trimestre/2.png)
   
   A continuación tenemos que configurar:
   - El nombre
     
     ![Imagen 1_2_1](/recursos/tema2/practica_2trimestre/1_2_1.png)
     
   - La imagen de Amazon Machine (AMI):
     Elegimos Ubuntu y dejamos la que viene por defecto: Ubuntu Server 24.04 LTS (HVM), SSD Volume Type
     
     ![Imagen 1_2_2](/recursos/tema2/practica_2trimestre/1_2_2.png)
     
   - El tipo de instancia EC2. Un tipo de instancia es una configuración particular de CPU, memoria (RAM), almacenamiento y capacidad de red.
     Elegimos t3.micro que es la que viene por defecto
     
     ![Imagen 1_2_3](/recursos/tema2/practica_2trimestre/1_2_3.png)

   - Un par de claves nuevas para la instancia y le asignamos un nombre. Es muy importante no perderlas pues la usaremos para poder iniciar la conexión SSH.
     Elegimos la extensión .pem y la guardamos en un lugar seguro. 

     ![Imagen 1_2_4](/recursos/tema2/practica_2trimestre/1_2_4png)

   - Un grupo de Seguridad y configurar las siguientes reglas de entrada:
     | Tráfico | Protocolo | Puerto | Origen |
     | ------------- | ------------- |
     | SSH | TCP | 22 | Nuestra IP |
     | HTTP | TCP | 80 | 0.0.0.0/0 |
     | HTTPS | TCP | 443 | 0.0.0.0/0 |

     ![Imagen 2_5](/recursos/tema2/practica_2trimestre/2_5.png)
   
   - El almacenamiento:
     Dejaremos el que viene por defecto: 1 volumen de 8GiB

     ![Imagen 1_2_6](/recursos/tema2/practica_2trimestre/1_2_6.png)

4. Lanzamos la instancia

![Imagen 1_3](/recursos/tema2/practica_2trimestre/1_3.png)

Una vez creada nos conectamos a la instancia pulsando en el botón Conectarse a la instancia:

![Imagen 1_4](/recursos/tema2/practica_2trimestre/1_4.png)

En la página que se nos muestra a continuación pulsamos en el botón Conectar

![Imagen 1_5](/recursos/tema2/practica_2trimestre/1_5.png)

Al intentar conectarme estaba dando error la conexión SSH así que edité la regla de entrada para SSH que configuré al principio. 
Lo que hice fue cambiar la IP de privada a 0.0.0.0/0, cómo los demás. 

![Imagen 1_6](/recursos/tema2/practica_2trimestre/1_6.png)

Y ahora sí se conectó a la instancia: 

![Imagen 1_7](/recursos/tema2/practica_2trimestre/1_7.png)

Si queremos conectarnos desde nuestro equipo lo hacemos de la siguiente manera:
1. Obtenemos la IP pública de nuestra instancia
   La podemos encontrar en la ventana donde se muestra el resumen de nuestra instancia antes de conectarnos:
   
   ![Imagen 1_8](/recursos/tema2/practica_2trimestre/1_8.png)
   
2. Abrimos nuestra terminal y nos ubicamos en la carpeta donde tenemos nuestro par de claves.pem (en mi caso es en C:\Users\ushca).
   Ejecutamos el siguiente comando:
   ```bash
   ssh -i "tu-clave.pem" ubuntu@IP_PUBLICA
   ```
   Una vez ejecutamos el comando nos preguntará si queremos continuar y escribimos yes
   
3. Ya estaremos conectados a nuestra instancia
   
   ![Imagen 1_9](/recursos/tema2/practica_2trimestre/1_9.png)

   Si nos da problemas de permisos con la clave, es porque tenemos que asignarle solo lectura para el propietario.
   Para ello, abrimos PowerShell y ejecutamos los siguientes comandos:
   ```bash
   # Quita permisos heredados y da solo lectura al usuario actual
   icacls.exe "tu_clave.pem" /reset
   icacls.exe "tu_clave.pem" /grant:r "$($env:username):(r)"
   icacls.exe "tu_clave.pem" /inheritance:r
   ```

   ![Imagen 1_10](/recursos/tema2/practica_2trimestre/1_10.png)
   

### 2. Instalación de servicios
Antes que nada, actualizamos el sistema con los siguientes comandos:
```bash
sudo apt update
sudo apt upgrade -y
```

Recuerda que todos los comandos tienen que empezar por sudo, para poder tener privilegios de administrador y porder ejecutarlos. 

![Imagen 2_1](/recursos/tema2/practica_2trimestre/2_1.png)

Empezamos instalando el servidor web (apache y php)

![Imagen 2_2](/recursos/tema2/practica_2trimestre/2_2.png)



