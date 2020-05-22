# Docker de Grafana

Implementaremos el servidor grafana en un docker.

Para implementarlo, nos descargaremos el docker creado por grafana con la última versión actualizada.

```
docker pull grafana/grafana:7.0.0-ubuntu
```

Para ejecutar la imagen, utilizaremos el comando **docker run** y mapearemos al puerto 3000 que es el que utiliza
grafana.

```
docker run -d --name grafana --hostname grafana -p 3000:3000 grafana/grafana:7.0.0-ubuntu
```
