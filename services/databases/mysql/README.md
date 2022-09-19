# MySQL
  
***MySQL***
> Image: _mysql:8.0.30_  
> Source: _[https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)_  
> URL: _localhost:3306_  
> User: _admin_  
> Pass: _123456_  
> Database: _exampledb_  

***PHPmyAdmin***
> Image: _phpmyadmin:5.2.0_  
> Source: _[https://hub.docker.com/_/phpmyadmin](https://hub.docker.com/_/phpmyadmin)_  
> URL: _[http://localhost:8080](http://localhost:8080)_  
> User: _admin_  
> Pass: _123456_  

---

## 1 - Docker Compose

### 1.1 Fichero YAML

```yaml
version: '3'

services:
  mysql:
    image: mysql:8.0.30
    container_name: mysql
    hostname: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_USER: admin
      MYSQL_PASSWORD: 123456
      MYSQL_DATABASE: exampledb
    networks:
      - net-mysql
    volumes:
      - 'vol-mysql:/var/lib/mysql'
    ports:
      - '3306:3306'
    expose:
      - '3306'

  phpmyadmin:
    image: phpmyadmin:5.2.0
    container_name: phpmyadmin
    hostname: phpmyadmin
    restart: always
    depends_on:
      - mysql
    environment:
      PMA_HOST: mysql
      PMA_PORT: '3306'
    networks:
      - net-mysql
    ports:
      - '8080:80'

networks:
  net-mysql:

volumes:
  vol-mysql:
```

### 1.2 - Fichero ENV 

Se pueden establecer las variables de entorno en un fichero externo como el siguiente (`./.env`):

```plaintext
# MySQL
MYSQL_ROOT_PASSWORD = 'root'
MYSQL_USER = 'admin'
MYSQL_PASSWORD = '123456'
MYSQL_DATABASE = 'exampledb'

# PHPMYADMIN
PMA_HOST = 'mysql'
PMA_PORT = '3306'
```

A continuación en el fichero `docker-compose.yaml` remplazaremos las secciones `environment` de los servicios que queramos que establezcan las variables de entorno que hemos especificado en el fichero anterior por la siguiente sección:

```yaml
env_file: './.env'
```

## 2 - Comandos

---

> [2.1 - Iniciar los servicios](#21---iniciar-los-servicios)  
> [2.2 - Iniciar los servicios en segundo plano](#22---iniciar-los-servicios-en-segundo-plano)  
> [2.3 - Ver los logs de los servicios](#23---ver-los-logs-de-los-servicios)  
> [2.4 - Ver los logs de los servicios en tiempo real](#24---ver-los-logs-de-los-servicios-en-tiempo-real)  
> [2.5 - Ver los logs de un contenedor](#25---ver-los-logs-de-un-contenedor)  
> [2.6 - Ver los logs de un contenedor en tiempo real](#26---ver-los-logs-de-un-contenedor-en-tiempo-real)  
> [2.7 - Ejecutar comandos en un contenedor](#27---ejecutar-comandos-en-un-contenedor)  
> [2.8 - Ejecutar comandos en un contenedor de manera interactiva](#28---ejecutar-comandos-en-un-contenedor-de-manera-interactiva)  
> [2.9 - Detener los servicios](#29---detener-los-servicios)  
> [2.10 - Detener los servicios y eliminar los volúmenes](#210---detener-los-servicios-y-eliminar-los-volúmenes)  
> [2.11 - Eliminar todos los contenedores de Docker](#211---eliminar-todos-los-contenedores-de-docker)  
> [2.12 - Eliminar todos los volúmenes de Docker](#212---eliminar-todos-los-volúmenes-de-docker)  
> [2.13 - Eliminar todas las networks de Docker](#213---eliminar-todas-las-networks-de-docker)  
> [2.14 - Eliminar todas las imágenes de Docker](#214---eliminar-todas-las-imágenes-de-docker)  

---

### 2.1 - Iniciar los servicios
> [🔍ÍNDICE](#2---comandos)  

```bash
docker compose up
```

```bash
docker-compose up
```

### 2.2 - Iniciar los servicios en segundo plano
> [🔍ÍNDICE](#2---comandos)  

```bash
docker compose up -d
```

```bash
docker-compose up -d
```

### 2.3 - Ver los logs de los servicios
> [🔍ÍNDICE](#2---comandos)  

```bash
docker compose logs
```

```bash
docker-compose logs
```

### 2.4 - Ver los logs de los servicios en tiempo real
> [🔍ÍNDICE](#2---comandos)  

```bash
docker compose logs -f
```

```bash
docker-compose logs -f
```

### 2.5 - Ver los logs de un contenedor
> [🔍ÍNDICE](#2---comandos)  

```bash
docker logs <CONTAINER_NAME>
```

### 2.6 - Ver los logs de un contenedor en tiempo real
> [🔍ÍNDICE](#2---comandos)  

```bash
docker logs <CONTAINER_NAME> -f
```

### 2.7 - Ejecutar comandos en un contenedor
> [🔍ÍNDICE](#2---comandos)  

```bash
docker exec <CONTAINER_NAME> <COMMAND>
```

### 2.8 - Ejecutar comandos en un contenedor de manera interactiva
> [🔍ÍNDICE](#2---comandos)  

```bash
docker exec -it <CONTAINER_NAME> <COMMAND>
```

Se puede abrir la terminal bash/sh de un contenedor

```bash
docker exec -it <CONTAINER_NAME> bash
```

```bash
docker exec -it <CONTAINER_NAME> sh
```

### 2.9 - Detener los servicios
> [🔍ÍNDICE](#2---comandos)  

```bash
docker compose down
```

```bash
docker-compose down
```

### 2.10 - Detener los servicios y eliminar los volúmenes
> [🔍ÍNDICE](#2---comandos)  

```bash
docker compose down -v
```

```bash
docker-compose down -v
```

### 2.11 - Eliminar todos los contenedores de Docker
> [🔍ÍNDICE](#2---comandos)  

```bash
docker rm -f $(docker ps -aq)
```

```bash
docker ps -aq | xargs docker rm -f
```

### 2.12 - Eliminar todos los volúmenes de Docker
> [🔍ÍNDICE](#2---comandos)  

```bash
docker volume rm -f $(docker volume ls -q)
```

```bash
docker volume ls -q | xargs docker volume rm -f
```

### 2.13 - Eliminar todas las networks de Docker
> [🔍ÍNDICE](#2---comandos)  

```bash
docker network rm $(docker network ls -q)
```

```bash
docker network ls -q | xargs docker network rm
```

### 2.14 - Eliminar todas las imágenes de Docker
> [🔍ÍNDICE](#2---comandos)  

```bash
docker image rm -f $(docker image ls -q)
```

```bash
docker image ls -q | xargs docker image rm -f
```