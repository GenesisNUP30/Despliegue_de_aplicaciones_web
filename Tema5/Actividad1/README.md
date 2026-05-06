# Práctica 1 - Instalar Docker en Ubuntu
Esta práctica se realizará en Ubuntu 24 específicamente.

Todos los comandos se ejecutan poniendo sudo delante para tener privilegios de administrador y porder ejecutarlos. 

Pasos:
1. Actualizar el sistema
   
   ````bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Instalar dependencias necesarias
   
   ```bash
   sudo apt install -y ca-certificates curl gnupg
   ```
3. Agregar la clave GPG de Docker

   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```
4. Agregar el repositorio oficial de Docker
   
   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo
   tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
   ```bash
   sudo apt update
   ```
5. Instalar Docker
   
   ```bash
   sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
6. Ejecutar Docker sin sudo.
   Por defecto, Docker requiere permisos de superusuario. Para ejecutarlo sin sudo, agregamos nuestro usuario al grupo docker:
   
   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```
Y ya tendríamos la instalación finalizada.
