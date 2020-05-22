# Docker de InfluxDB

Implementaremos el servidor influxDB en un docker.

Para implementarlo, nos descargaremos el docker creado por influxDB.

```
docker pull influxdb
```

Para crear el docker, utilizaremos el comando **docker run** y mapearemos al puerto 8086 y 8088 que són los que utiliza
influxdb, junto a la imagen.

```
docker run --rm --name influxdb --hostname influxdb -p 8086:8086 -p 8088:8088 influxdb
```
