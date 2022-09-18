# Docker

> **Autor:** _Ángel Herce Soto_  
> **Fecha:** _19/09/2022_

_Proyecto donde iré creando varios ficheros `docker-compose.yaml` y `.Dockerfile` con varias herramientas para su futuro uso._

---

## 1- Instalar Docker Engine

> _**[DOC](https://docs.docker.com/engine/install/):** Install Docker Engine_  

### 1.1- Instalación en Ubuntu

> _**[DOC](https://docs.docker.com/engine/install/ubuntu/):** Ubuntu Install_  

1. Las versiones anteriores de Docker se llamaban `docker`, `docker.io` o `docker-engine`. Si están instalados, las desinstalaremos:

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

2. Configuramos el repositorio de Docker.
    
    2.1. Actualizamos `apt` e instalamos los paquetes necesarios para permitir el uso de un repositorio a través de HTTPS:  
   
    ```shell
       sudo apt-get update
    ```
   
    ```shell
    sudo apt-get install \
      ca-certificates \
      curl \
      gnupg \
      lsb-release
    ```

    2.2. Agregamos la clave GPG oficial de Docker:

    ```shell
       sudo mkdir -p /etc/apt/keyrings
    ```

    ```shell
       curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```

    2.3. Usaremos el siguiente comando para configurar el repositorio:

    ```shell
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
   
3. Instalamos Docker Engine.
    
```shell
sudo apt-get update
```    

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

4. Iniciamos el servicio de Docker.

```shell
sudo service docker start
```

5. Verificamos que Docker Engine esta instalado correctamente ejecutando la imagen `hello-world`.

```shell
sudo docker run hello-world
```

6. Configuramos el comando Docker para ejecutarlo sin `sudo`.

    6.1. Agregamos nuestro nombre de usuario al grupo docker:
    
    ```shell
    sudo usermod -aG docker ${USER}
    ```
   
    6.2. Para aplicar la nueva membresía de grupo, cerraremos la sesión del servidor y volveremos a iniciarla, o lanzaremos el siguiente comando:

    ```shell
    su - ${USER}
    ```
   
    6.3. Confirmamos que ahora nuestro usuario se agregó al grupo docker con el siguiente comando:
    
    ```shell
    id -nG 
    ```
   
---

## 2- Instalar Plugin de Docker Compose

> _**[DOC](https://docs.docker.com/compose/install/):** Install Docker Compose_  

### 2.1- Instalar Plugin de Docker Compose en Ubuntu

> _**[DOC](https://docs.docker.com/compose/install/linux/):** Ubuntu Install_   

1. Instalamos el plugin.

```shell
sudo apt-get update
```

```shell
sudo apt-get install docker-compose-plugin
```

2. Verificamos que Docker Compose esta instalado correctamente comprobando la versión.

```shell
docker compose version
```