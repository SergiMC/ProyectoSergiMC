# Docker en Telegraf

Implementaremos el servidor telegraf en un docker.

Para implementarlo, utilizaremos el docker creado por telegraf.

```
docker pull telegraf
```

## Configuración del docker

Para configurar telegraf, debemos crear y editar el fichero de configuración *telegraf.conf* con los plugins que necesitemos, utilizaremos el siguiente comando:

```
./telegraf -sample-config -input-filter cpu:mem:disk -output-filter influxDB > telegraf.conf
```
Una vez hemos creado el fichero, deberemos configurar el apartado de output para remarcar hacia donde queremos
que telegraf envie las métricas, en nuestro caso utilizaremos influxdb. 

* Debemos poner la ip del docker o el nombre que le hemos configurado al docker.

```
# Configuration for sending metrics to InfluxDB
[[outputs.influxdb]]
  ## The full HTTP or UDP URL for your InfluxDB instance.
  ##
  ## Multiple URLs can be specified for a single cluster, only ONE of the
  ## urls will be written to each interval.
  # urls = ["unix:///var/run/influxdb.sock"]
  # urls = ["udp://127.0.0.1:8089"]
  # urls = ["http://127.0.0.1:8086"]
    urls = ["http://influxdb:8086"]

```

## Crear el docker de telegraf.

Deberemos añadir los volumenes que necesitamos para que el docker telegraf se configure a través del fichero que hemos creado  y configurado, también añadiremos el socket del docker, tendremos que añadir la opción add-host para que utilize la ip del docker influxdb ya que si no se la indicamos, utilizará la ip de nuestro propio docker.

```
docker run -d --add-host "influxdb:ip-docker" --hostname telegraf -v $(pwd)/telegraf/telegraf.conf:/etc/telegraf/telegraf.conf:ro -v /var/run/docker.sock:/var/run/docker.sock:ro telegraf

```
