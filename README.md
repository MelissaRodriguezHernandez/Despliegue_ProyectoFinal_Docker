# Despliegue del proyecto final en Docker

1. [Introducción](#intro)
2. [Configuración del archivo docker-compose.yml](#conf)
3. [Pasos para el despliegue de la aplicación](#apli)
4. [Preparación y subida de la imagen](#img)
5. [Conclusiones](#con)
6. [Annexos](#anex)

<div id= 'intro'>

## Introducción
  
> Esta práctica se basara en el despliegue de nuestro proyecto en docker, mediante el docker compose, haciendo uso de : mysql, phpmyadmin y tomcat.
  Finalmente crearemos una imagen a partir del despliegue y lo subiremos a DockerHub.
  Participantes : Melissa Rodriguez, David Mulet y Bernat Diego 
  
</div>

<div id= 'conf'>

## Configuración del archivo docker-compose

El archivo docker compose es un archivo YML donde definiremos los servicios, redes y volúmenes. Lo colocaremos en dentro del directorio junto con todo lo necesario para armar el ambiente. Nuestro docker compose tendra lo necesario para ejecutar el mysql, tomcat y phpmyadmin. Es el siguiente:

  ```
  version: '3.3'
services:
   db:
     image: mysql:5.7
     volumes:
       - db_vol:/var/lib/mysql
       - ./mysql-dump:/docker-entrypoint-initdb.d
     environment:
       MYSQL_ROOT_PASSWORD: root
       MYSQL_DATABASE: proyecto
       MYSQL_USER: testuser
       MYSQL_PASSWORD: root
     ports:
       - 3306:3306
   phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    ports:
      - '8081:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: root
   web:
    build:
      context: .       
    depends_on:
      - db
    image: tomcat
    volumes:
            - ./target/Gestion_Proyectos.war:/usr/local/tomcat/webapps/Gestion_Proyectos.war
    ports:
      - '8080:8080'
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: proyecto
      MYSQL_USER: testuser
      MYSQL_PASSWORD: root
volumes:
    db_vol: 
  ```
  
</div>

<div id= 'apli'>

## Pasos para el despliegue de la aplicación

Una vez hemos creado el docker-compose.yml, en el mismo directorio deberemos tener todos los archivos necesarios. 
  
  - El archivo del proyecto en un .war (el nuestro esta en la carpeta target)
  
  - El archivo de la base de datos .sql (el nuestro esta en mysql-dump)

Con todo esto, vamos a la terminal y nos situamos en el directorio donde se encuentra todo, y ejecutamos el siguiente comando:
  
```
  sudo docker-compose up -d
  ```

Ahora podremos ir a las páginas. Si vamos al localhost:8080 veremos como se despliega la página principal y como podemos registarnos y loguearnos.
  
![1](https://user-images.githubusercontent.com/91748294/172957693-e01ae2d8-261c-4219-b945-562bd981cfc1.jpeg)
  
![3](https://user-images.githubusercontent.com/91748294/172957772-7176b888-cac0-4cd6-b915-20bdbb591b12.jpeg)
  
![2](https://user-images.githubusercontent.com/91748294/172957782-b1f7ccac-fc9a-486e-98ab-b8f865df4d8f.jpeg)
  
![4](https://user-images.githubusercontent.com/91748294/172957788-69747850-68a0-41d0-ac7a-c4b2b20d4182.jpeg)
  
</div>

<div id= 'img'>

## Preparación y subida de la imagen
  
Ahora con nuestro proyecto desplegado procederemos a crear una imagen y contenedor, para luego subirlo en el DockerHub
  
</div>

<div id= 'con'>

## Conclusiones
  
</div>

<div id= 'anex'>

## Annexo
  
</div>



