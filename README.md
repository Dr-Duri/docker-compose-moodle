# docker-compose-moodle
[![Build Status](https://travis-ci.org/jobcespedes/docker-compose-moodle.svg?branch=master)](https://travis-ci.org/jobcespedes/docker-compose-moodle)
![Moodle](https://img.shields.io/badge/Moodle-3.5-blue.svg?colorB=f98012)
![Apache2](https://img.shields.io/badge/Apache2-2.4-blue.svg?colorB=557697)
![PHP](https://img.shields.io/badge/PHP-7-blue.svg?colorB=8892BF)
![Postgres](https://img.shields.io/badge/Postgres-9.6-blue.svg?colorB=0085B0)
[![Buy me a coffee](https://img.shields.io/badge/$-BuyMeACoffee-blue.svg)](https://www.buymeacoffee.com/jobcespedes)
[![Software License](https://img.shields.io/badge/License-APACHE-black.svg?style=flat-square&colorB=585ac2)](LICENSE)

>Leer en [**Español**](#español)

This project quickly builds a local workspace for Moodle  (Apache2, PHP-FPM with XDEBUG y Postgres) using containers for each of its main components. The local workspace is build and manage by Docker Compose

## Quickstart:
1. Docker installed. Check out how to install [Docker](https://docs.docker.com/install/)
2. Docker Compose installed. Check out how to install [Docker Compose](https://docs.docker.com/compose/install/)
3. Download this repo: `git clone https://github.com/jobcespedes/docker-compose-moodle.git && cd docker-compose-moodle`
4. Clone Moodle repo: `git clone --branch MOODLE_35_STABLE --depth 1 git://github.com/moodle/moodle html`
5. Deploy with: `docker-compose up -d`

## Contents
1. [Environment Variables](#Environment-variables)
2. [Docker Compose Resources](#Docker-Compose-resources)
3. [Workspace Operations](#Project-management-with-Docker-Compose)
4. [Debugging with XDEBUG](#XDEBUG)
5. [Database management with Pgadmin4](#Pgadmin4)
6. [Notes](#Notes)
7. [Install Docker](https://docs.docker.com/install/)
8. [Install Docker Compose](https://docs.docker.com/compose/install/)

## Environment variables
The following table describes environment variables set in [**.env**](.env). The defaults work for a initial setup. Change them if needed.

| Variable | Default value | Use |
| :--- |:--- |:--- |
| **REPO_FOLDER** | html | Default relative path for Moodle repo |
| **DOCUMENT_ROOT** | /var/www/html | Mount point inside containers for volume **REPO_FOLDER** |
| **MY_TZ** | America/Costa_Rica | Containers timezone |
| **PG_LOCALE** | es_CR | Containers locale |
| **PG_PORT** | 5432 | Postregres port to expose  |
| **POSTGRES_DB** | moodle | Postgres DB for Moodle |
| **POSTGRES_USER** | user | DB user for Moodle |
| **POSTGRES_PASSWORD** | password | DB password for Moodle |
| **PHP_SOCKET** | 9000 | PHP-FPM socket to connect apache2  and php-fpm services |
| **ALIAS_DOMAIN** | localhost | Domain Alias |
| **WWW_PORT** | 80 | Web port to be bound |
| **MOODLE_DATA** | /var/moodledata | Mount point inside containers for Moodle data folder  |
| **WWWROOT** | localhost | Host part to set in Moodle file 'config.php' for config option 'wwwroot' |

## Docker Compose resources
The following table sums up Docker Compose resources.

| Component | Type | Responsability | Content | Config |
| :--- |:--- | :--- | :---| :---|
| **apache2** | Container | Web server | Debian9, Apache2 | [Apache2](http://dockerfile.readthedocs.io/en/latest/content/DockerImages/dockerfiles/php-apache.html#web-environment-variables) server and [modules for Moodle](https://docs.moodle.org/35/en/Apache) |
| **cron** | Container | Cron task for Moodle | Debian9, Cron | [Moodle cron task](https://docs.moodle.org/35/en/Cron) and its frequency |
| **php-fpm** | Container | Process manager for PHP | Debian9, PHP-FPM, XDEBUG | PHP, its modules and Moodle dependencies |
| **postgres** | Container | DBMS  | Debian9, Postgres | [User and DB](https://hub.docker.com/_/postgres/) |
| **db_dumps** | Volume | Restore db when built | Dump files for DB to restore | To restore a initial database when build using the dump file called dump-init.sql.gz |
| **moodledata** | Volume | Moodle data store | Data generated by moodle | [Moodle data dir ](https://docs.moodle.org/35/en/Installing_Moodle#Create_the_.28moodledata.29_data_directory) |
| ***REPO_FOLDER*** | Volume | Moodle code  | Moodle code  | It is set to 'html/' by deafult (check out [**.env**](.env)) |

## Project management with Docker Compose
> **Inside project folder**
1. Run project
``` bash
docker-compose up -d
# Different project name
# docker-compose -p my-proj up -d
```
2. Stop project
``` bash
docker-compose stop
# docker-compose stop <service>
```
3. Start project
``` bash
docker-compose start
# docker-compose start <service>
```
4. Remove project
``` bash
docker-compose down
# Remove volumes too
# docker-compose --volumes
# With different project name:
# docker-compose -p my-proj down
```
5. Logs
``` bash
docker-compose logs
# docker-compose logs -f --tail="20" <service>
```

## XDEBUG
> Use idekey `PHPTEST`

### PHPStorm
Debug config for IDE PHPStorm:

1. Add server:
    * Settings -> Languages -> PHP -> Servers
2. Add PHP remote debug
    * Run / Debug Configurations -> PHP remote debug
    * Use server created in step #1 and set idekey `PHPTEST`
3. Enable `Start listening for PHP Debug Connections`

## Pgadmin4
Config pgadmin4
1. Go to http://localhost:5050
2. In ```File -> Preferences -> Binary paths``` set ```usr/bin```
3. Add new server:
    * Tab ```General```
        * Name: Any name you want
    * Tab ```Connection```
        * Host name/address: ```postgres```
        * Host Username: ```user```
        * Host Password: ```password```
    * Save

## Notes
> Restore a specific database when postgres service starts

A database can be automatically restored when postgres service starts. By placing a dump file inside 'db_dumps' folder and naming it "dump-init.sql.gz", postgres container will try to restore that file as initial database.
> **IMPORTANT**: Depending of dump-init.sql.gz size, database initial availability could be delayed

# Español
Este es un repositorio para crear rápidamente un entorno de trabajo con Moodle (Apache2, PHP-FPM con XDEBUG y Postgres) usando contenedores para cada uno sus principales componentes. El entorno de trabajo se crea y gestiona con Docker Compose.

## Pasos rápidos para crear proyecto:
1. Tener Docker. Ver como instalar [Docker](https://docs.docker.com/install/)
2. Tener Docker Compose. Ver como instalar [Docker Compose](https://docs.docker.com/compose/install/)
3. Descargar este repo y acceder a él: ```git clone https://github.com/jobcespedes/docker-compose-moodle.git && cd docker-compose-moodle```
4. Copiar repositorio de código de Moodle: ```git clone --branch MOODLE_35_STABLE --depth 1 git://github.com/moodle/moodle html```
5. Desplegar con: ```docker-compose up -d```

## Variables de ambiente
La siguiente tabla contiene las variables utilizadas en el archivo [**.env**](.env) para Docker Compose. Los valores por defecto funcionan para una configuración inicial. Cámbielos de ser necesario.

| Variable | Valor por defecto | Utilidad |
| :--- |:--- |:--- |
| **REPO_FOLDER** | html | Ruta relativa para el código de Moodle |
| **DOCUMENT_ROOT** | /var/www/html | Punto de montaje para **REPO_FOLDER** dentro de contenedores |
| **MY_TZ** | America/Costa_Rica | Zona horaria para los contenedores |
| **PG_LOCALE** | es_CR | Configuración de lugar |
| **PG_PORT** | 5432 | Puerto de base de datos postgres a publicar  |
| **POSTGRES_DB** | moodle | Nombre de la base de datos postgres de Moodle |
| **POSTGRES_USER** | user | Nombre de usuario de la base de datos postgres de Moodle |
| **POSTGRES_PASSWORD** | password | Contraseña de la base de datos postgres de Moodle |
| **PHP_SOCKET** | 9000 | Socket para conectar apache2 con php-fpm |
| **ALIAS_DOMAIN** | localhost | Alias del Dominio |
| **WWW_PORT** | 80 | Puerto web a publicar |
| **MOODLE_DATA** | /var/moodledata | Carpeta de datos de Moodle a montar en los contenedores |
| **WWWROOT** | localhost | Para de nombre de host en la url de config.php de Moodle |

## Estructura de Docker Compose
A continuación se incluye una tabla que resume la estructura del archivo de Docker Compose:

| Componente | Tipo | Responsabilidad | Contenido | Configuración |
| :--- |:--- | :--- | :---| :---|
| **apache2** | Contenedor | Servidor web | Debian9, Apache2 | El mínimo de módulos de [Apache2](http://dockerfile.readthedocs.io/en/latest/content/DockerImages/dockerfiles/php-apache.html#web-environment-variables) |
| **cron** | Contenedor|Tarea de cron de Moodle | Debian9, Cron | Frecuencia de ejecución de tarea [cron de Moodle](https://docs.moodle.org/35/es/Cron) |
| **php-fpm** | Contenedor | Interprete y manejador de procesos para PHP | Debian9, PHP-FPM, XDEBUG | Modulos de php y paquetes adicionales para Moodle  |
| **postgres** | Contenedor | Gestor de base de datos  | Debian9, Postgres | [Usuario y base de datos](https://hub.docker.com/_/postgres/) |
| **db_dumps** | Volumen | Restaurar una base de datos inicial | Archivos de respaldo de base de datos. | Para restaurar al iniciar, nombre el archivo sql de respaldo como dump-init.sql.gz |
| **moodledata** | Volumen | Almacén de datos de Moodle | Archivos generados por Moodle | [Moodle data dir ](https://docs.moodle.org/all/es/Gu%C3%ADa_r%C3%A1pida_de_Instalaci%C3%B3n#Crea_el_directorio_de_datos) |
| ***REPO_FOLDER*** | Volumen | Código de aplicación  | Código de Moodle  | Por defecto es './html' (ver archivo .env) |

## Gestión del proyecto con Docker Compose
> **Dentro de la carpeta del proyecto**
1. Correr proyecto
``` bash
docker-compose up -d
# Nombrar el proyecto diferente a la carpeta:
# docker-compose -p mi-proy up -d
```
2. Detener el proyecto
``` bash
docker-compose stop
# docker-compose stop <servicio>
```
3. Iniciar el proyecto
``` bash
docker-compose start
# docker-compose start <servicio>
```
4. Eliminar proyecto
``` bash
docker-compose down
# Eliminar los volumenes también:
# docker-compose --volumes
# Eliminar con un nombre de proyecto especifico:
# docker-compose -p mi-proy down
```
5. Logs
``` bash
docker-compose logs
# docker-compose logs -f --tail="20" <service>
```

### XDEBUG
> Se utiliza el idekey `PHPTEST`

#### PHPStorm
Configuración para depurar con IDE PHPStorm:

1. Agregar servidor:
    * Settings -> Languages -> PHP -> Servers
2. Agregar PHP remote debug
    * Run / Debug Configurations -> PHP remote debug
    * Utilizar servidor previamente agregado y establecer como idekey el valor `PHPTEST`
3. Activar botón `Start listening for PHP Debug Connections`

### Pgadmin4
Pasos para usar pgadmin4
1. Ingresar a http://localhost:5050
2. En ```File -> Preferences -> Binary paths``` establecer en ```usr/bin```
3. Agregar nuevo servidor:
    * Pestaña ```General```
        * Name: Un nombre para el servidor
    * Pestaña ```Connection```
        * Host name/address: ```postgres```
        * Host Username: ```user```
        * Host Password: ```password```
    * Guardar

## Notas
> Restaurar una base de datos al inicio

Se puede restaurar una base de datos, agregando el script sql generador (comprimido como gzip) a la caperta db_dumps y nombrando el archivo como dump-init.sql.gz
> **IMPORTANTE**: Dependiendo del tamaño, la ejecución de este sql podría demorar la disponibilidad inicial de la base de datos.
