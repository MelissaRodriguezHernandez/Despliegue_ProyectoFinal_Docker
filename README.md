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
  
  ![5](https://user-images.githubusercontent.com/91748294/172960413-41e4cbc1-74e1-48c6-85d8-3a05cf6468da.jpeg)
  
![6](https://user-images.githubusercontent.com/91748294/172960428-33db6ff2-7911-4217-b0bf-214dfa09f514.jpeg)
  
</div>

<div id= 'img'>

## Preparación y subida de la imagen
  
Ahora con nuestro proyecto desplegado procederemos a crear una imagen y contenedor, para luego subirlo en el DockerHub.
  
Empezaremos por crear el Dockerfile

```
FROM tomcat:latest

LABEL maintainer="DavidMuletMelia"

ADD ./target/Gestion_Proyectos.war /usr/local/tomcat/webapps/

EXPOSE 8080

CMD ["catalina.sh", "run"]
  ```
Y ejecutamos el comando para monstar la imagen:

  ```
  docker build -t davidmuletmelia/gestionproyectos:latest
  ```
  ![6 (2)](https://user-images.githubusercontent.com/91748294/172959140-49070098-e48b-4f3a-945d-2dbda0a70cd7.jpeg)

Ahora procederemos a subirlo al dockerHub
  
  ```
docker push davidmuletmelia/gestionproyectos:latest
  ```
![7](https://user-images.githubusercontent.com/91748294/172959310-4a1b6139-e6d9-482b-bf47-82e558267314.jpeg)
  
Si todo ha salido bien nos saldría algo así, con esto ya podriamos bajarnos el contenedor en cualquier ordenador y ejecutarlo:
  
  ![8](https://user-images.githubusercontent.com/91748294/172959404-60e131a5-5749-403e-8786-793927f3c957.jpeg)
  
</div>

<div id= 'con'>

## Conclusiones

Hemos utilizado como bases los archivos de otro proyecto que ya funcionaba y hemos sustitudo el .war y el .sql por los nuestros.

La parte mas complicada ha sido el funcionamiento de la base de datos, ha dado muchos problemas por los puertos y credenciales.
  
</div>

<div id= 'anex'>

## Annexo
  
[Link del contendor en DockerHub](https://hub.docker.com/r/davidmuletmelia/gestionproyectos)
  
</div>



