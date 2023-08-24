/***actualziacion kernel al instalar Docker***/
wsl --update

/***crea contenedor parm puerto, nombreBd, password, tipo, version***/
docker run -d -p 3308:3306 --name platzi-market -e MYSQL_ROOT_PASSWORD=1234 mysql:latest

/***trabajar abriendo terminal en la carpeta contiene archivos scripts y Dockerfile***/

/***crear archivo "Dockerfile"(respetar nombre sin extension)***/
/***buscar ejemplos en el repo de hub.docker.com***/
# Base image
FROM mysql:8.0.1

# Add all scripts 
COPY ./scripts/ /docker-entrypoint-initdb.d/
/***crear archivo Dockerfile(respetar nombre sin extension)***/

/***crear carpeta "scripts", incluir bk .sql de db***/

/***crea imagen para ser compartida o subida al hub***/
docker build -t nombreContendor ./

/***creamos repositorio en hub.docker.com***/

/***crear copia en local tagname version 1***/
docker tag nombreImagen username/nombreImagen:1.0.0

/***push hub.docker***/
docker push username/bd-mysql:tagname

/***ejecutar aplicaciones descargadas***/
docker run -p 8080:8080 myorg/myapp
