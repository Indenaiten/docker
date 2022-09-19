# Odoo & PostgreSQL

***Odoo***
> Image: _odoo:15.0_  
> Source: _[https://hub.docker.com/_/odoo](https://hub.docker.com/_/odoo)_  
> URL: _[http://localhost:80](http://localhost:80)_  
  
***PostgreSQL***
> Image: _postgres:13_  
> Source: _[https://hub.docker.com/_/postgres](https://hub.docker.com/_/postgres)_  
> URL: _localhost:5432_  
> User: _odoo_  
> Pass: _odoo_  
> Database: _postgres_  

***PGAdmin***
> Image: _dpage/pgadmin4:6.13_  
> Source: _[https://hub.docker.com/r/dpage/pgadmin4](https://hub.docker.com/r/dpage/pgadmin4)_  
> URL: _[http://localhost:8080](http://localhost:8080)_  
> User: _odoo@admin.com_  
> Pass: _odoo_  

---

## 1 - Docker Compose

### 1.1 Fichero YAML

```yaml
version: '3'

services:
  database:
    image: postgres:13
    container_name: database
    hostname: database
    environment:
      POSTGRES_DB: postgres
      POSTGRES_PASSWORD: odoo
      POSTGRES_USER: odoo
    networks:
      - net-database
      - net-app
    volumes:
      - 'vol-database:/var/lib/postgresql/data'
    ports:
      - '5432:5432'
    expose:
      - '5432'

  pgadmin:
    image: dpage/pgadmin4:6.13
    container_name: pgadmin
    hostname: pgadmin
    restart: always
    depends_on:
      - database
    environment:
      PGADMIN_DEFAULT_EMAIL: odoo@admin.com
      PGADMIN_DEFAULT_PASSWORD: odoo
    networks:
      - net-database
    volumes:
      - 'vol-pgadmin:/var/lib/pgadmin' 
      - './docker-data/pgadmin/config/servers.json:/pgadmin4/servers.json'
    ports:
      - '8080:80'
  
  app:
    image: odoo:15.0
    container_name: app
    hostname: app
    depends_on:
      - database
    environment:
      HOST: database
      USER: odoo
      PASSWORD: odoo
    networks:
      - net-app
    volumes:
      - 'vol-app:/var/lib/odoo'
      - './app/config:/etc/odoo'
      - './app/addons:/mnt/extra-addons'
    ports:
      - "80:8069"

networks:
  net-database:
  net-app:
  
volumes:
  vol-database:
  vol-pgadmin:
  vol-app:
```

### 1.2 - Fichero ENV 

Se pueden establecer las variables de entorno en un fichero externo como el siguiente (`./.env`):

```plaintext
# PostgreSQL
POSTGRES_DB = 'postgres'
POSTGRES_USER = 'odoo'
POSTGRES_PASSWORD = 'odoo'

# PgAdmin
PGADMIN_DEFAULT_EMAIL = 'odoo@admin.com'
PGADMIN_DEFAULT_PASSWORD = 'odoo'

# Odoo
HOST = 'database'
USER = ${POSTGRES_USER}
PASSWORD = ${POSTGRES_PASSWORD}
```

A continuación en el fichero `docker-compose.yaml` remplazaremos las secciones `environment` de los servicios que queramos que establezcan las variables de entorno que hemos especificado en el fichero anterior por la siguiente sección:

```yaml
env_file: './.env'
```

### 1.3 - Configuración de PgAdmin

La configuración de la base de datos se encuentra en el fichero `./docker-data/pgadmin/config/servers.json`.

```json
{
    "Servers": {
        "test": {
            "Name": "odoo",
            "Group": "Servers",
            "Port": 5432,
            "Username": "odoo",
            "Password": "odoo",
            "Host": "database",
            "SSLMode": "prefer",
            "MaintenanceDB": "postgres"
        }
    }
}
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