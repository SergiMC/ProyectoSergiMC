# Docker de Grafana

Implementaremos el servidor grafana en un docker.

Para implementarlo, nos descargaremos el docker creado por grafana con la última versión actualizada.

```
docker pull grafana/grafana:7.0.0-ubuntu
```

Para crear el docker, utilizaremos el comando **docker run** y mapearemos al puerto 3000 que es el que utiliza
grafana, junto a la imagen.

```
docker run --rm --name grafana --hostname grafana -p 3000:3000 docker.io/grafana/grafana:7.0.0-ubuntu
```
Creado el docker, abriremos el navegador y nos conectaremos a grafana con la ip del docker y puerto.

```
http://ip_docker:3000 
```
