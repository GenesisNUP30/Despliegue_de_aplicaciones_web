# Práctica 1 - Instalar Docker en Ubuntu
Esta práctica se realizará en Ubuntu 24 específicamente.

Todos los comandos se ejecutan poniendo sudo delante para tener privilegios de administrador y porder ejecutarlos. 

Pasos:
1. Actualizar el sistema
   
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
   ![Imagen 1](/recursos/tema5/actividad1/1.png)
   
2. Instalar dependencias necesarias
   
   ```bash
   sudo apt install -y ca-certificates curl gnupg
   ```
   ![Imagen 2](/recursos/tema5/actividad2/2.png)
3. Agregar la clave GPG de Docker

   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```
   ![Imagen 3](/recursos/tema5/actividad1/3.png)
   
4. Agregar el repositorio oficial de Docker
   
   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo
   tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
   
   ```bash
   sudo apt update
   ```
   ![Imagen 4](/recursos/tema5/actividad1/4.png)
   
5. Instalar Docker
   
   ```bash
   sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
   ![Imagen 5](/recursos/tema5/actividad1/5.png)
   
6. Ejecutar Docker sin sudo.
   Por defecto, Docker requiere permisos de superusuario. Para ejecutarlo sin sudo, agregamos nuestro usuario al grupo docker:
   
   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```
   ![Imagen 6](/recursos/tema5/actividad1/6.png)
   
7. Comprobar estado.
   Tras instalar correctamente el paquete Docker CE, el servicio debería iniciarse automáticamente y habilitarse para iniciarse al arrancar el sistema.
   Lo comprobamos con este comando
   ```bash
   sudo systemctl status docker
   ```
   ![Imagen 7](/recursos/tema5/actividad1/7.png)

8. Verificar instalación correcta
   Para ello, ejecutamos la imagen hello-world.
   ```bash
   sudo docker run hello-world
   ```
   ![Imagen 8](/recursos/tema5/actividad1/8.png)

   
