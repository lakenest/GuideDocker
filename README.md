##docker compose


#ver listado de imagenes
docker images

#clonar imagen de docker hub, con o sin tag.
#puedes buscar ejemplos de imagen en hub docker
docker pull node
docker pull node:18

#remover una imagen de docker local
docker image rm node:16

#crear contenedor indicando la imagen, nos retorna un idext.
docker create mongo /*crear primer contenedor referenciando a la imagen que va a usar no devuelve un id
docker container create mongo /*la linea anterior tambien se puede escribir asi.

#inicializar contenedor indicando el id, retorna el idext.
docker start 364c4f0903e1eb36da83d42df3dab6e2bcbf6d6cf9d60af8c044c50ea6ba3ca3

#confirmar si el contenedor se inicio correctamente, retorna info del contenedor.
docker ps

#detener el contenedor indicando el id, retorna info vacia de contenedores.
docker stop 364c4f0903e1

#listar los contenedores disponibles
docker ps -a

#elimar un contenedor por el nombre
docker rm fervent_wescoff

#crear contenedor con nombre(ctmongo) e imagen(mongo), retorna idext.
docker create --name ctmongo mongo

#crear contenedo indicando puertodockerlocal/puertodockerhub
docker create -p27017:27017 --name ctmongose mongo

#comando unido para descargar imagen, crea contenedor e iniciar (cada ejecucion crea un nuevo contenedor).
docker run --name ctmongose -p27017:27017 -d mongo

#crear imagen en archivo 'Dockerfile' sin extension. (se crea diferente por tipo de aplicacion)
##ejemplo para scripts de bd
# Base image
FROM mysql

# Add all scripts 
COPY ./scripts/ /docker-entrypoint-initdb.d/

##ejemplo para apps node
# Basarse en imagen
FROM node:18

# Ejecutar en directorio linux(mkdir crea el direcotorio)
RUN mkdir -p /home/app

# Copiar los archivos que va  a ejecutar
COPY ./home/app

# Exponer un puerto para conectarnos a este contenededor
EXPOSE 3000

# Comando mas ruta completa, para que ejecute la app.
CMD ["node", "/home/app/index.js"]


#Crear imagen indicando nombreimagen:tag ubicacion ('./' estamos en la carpera donde esta Dockerfile)
docker build -t  basicstore:1.0.0 ./

#configurar un contenedor docker disponible para conexion, todos se hacen de forma diferente.(ejemplo mysql) -e son variables de entorno. databasename es referencias, siempre direcciona a la db que tiene la imagen.
docker run -d -p 3309:3309 --name ctmysqlbasicstore -e MYSQL_ROOT_PASSWORD=456123 -e MYSQL_DATABASE=dbtest dbstore:1.0.0

docker run -d -p puertoanfitrion:puertodocker --name namecontenedor -e MYSQL_ROOT_PASSWORD=456123 -e MYSQL_DATABASE=basicstore nombreimagen:tag


#relacionar contenedores para usar app y bd juntas por ejemplo.
#listar redes de docker
docker network ls

#crear redes de docker
docker network create mired

#eliminar red en docker
docker network rm mired

##cadena de conexion de app con bd, debe apuntar al nombre del contenedor(cambiar localhost o ip por ctmysql)

#crear contenedor dentro de una red (puedes tener varios, caso app y bd o varias bd)
docker create -ppuertolocal:puertodocker --name nombrecontenedor --network nombrered -e MYSQL_ROOT_PASSWORD=password nombreimagen
docker create -p3306:3306 --name ctmysql1 --network mired -e MYSQL_ROOT_PASSWORD=456123 basicstore:1.0.0
docker create -p3306:3306 --name ctmysql2 --network mired -e MYSQL_ROOT_PASSWORD=456123 basic:2.0.0
