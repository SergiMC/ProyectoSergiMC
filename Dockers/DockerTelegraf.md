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

