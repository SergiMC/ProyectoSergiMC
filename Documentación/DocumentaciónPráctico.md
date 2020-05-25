# Documentación Práctico

El caso práctico consistirá en montar una estructura de dockers con **infuxdb** y **telegraf** y visualizar los datos con **grafana**.

## Información de las herramientas

* [Grafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Documentaci%C3%B3n/Grafana.md): Herramienta de análísis que se encarga de visualizar los datos proporcionados por influxDB.
* [InfluxDB](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Documentaci%C3%B3n/influxDB.md): Herramienta que se encarga de almacenar los datos enviados por telegraf. InfluxDB se encarga de proporcionar los datos a grafana.
* [Telegraf](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Documentaci%C3%B3n/Telegraf.md): Herramienta que se encarga de obtener los datos que configuramos y de enviar las métricas a influxDB.

## Estructura de los dockers

![Estructura](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/esquema.jpg)

La estructura del esquema está compuesta por 3 dockers:

* Docker de Telegraf
* Docker de InfluxDB
* Docker de Grafana

## Ejecución de la estructura de los dockers

Para poner en funcionamiento la estructura de lo dockers, crearemos un docker-compose.yml y lo configuraremos con los 3 dockers.

```
version: "2"
services:
  #Servidor Grafana
  grafana:
    image: sergimc/grafana
    container_name: grafana
    hostname: grafana
    ports:
      - "3000:3000"
    volumes:
      - "grafana_v:/usr/share/grafana/data"
    networks:
     - netproyecto
  #Servidor InfluxDB
  influxdb:
    image: sergimc/influxdb
    container_name: influxdb
    hostname: influxdb
    ports:
      - "8086:8086"
      - "8088:8088"
    volumes:
      - "influxdb_v:/var/lib/influxdb"
    networks:
     - netproyecto
  #Servidor Telegraf
  telegraf:
   image: sergimc/telegraf
   container_name: telegraf
   hostname: telegraf
   restart: always
   volumes:
    - ./telegraf.conf:/etc/telegraf/telegraf.conf:ro
    - /var/run/docker.sock:/var/run/docker.sock:ro
   networks:
     - netproyecto

volumes:
  grafana_v:
  influxdb_v:
networks:
  netproyecto:
```

Ejecutamos el docker compose en modo datach.

```
[sergimc@192 grafana]$ docker-compose up -d
Creating telegraf ... 
Creating grafana ... 
Creating influxdb ... 
Creating telegraf
Creating grafana
Creating grafana ... done
```
Comprobamos que los puertos esten mapeados a nuestro host, los puertos que tenemos que validar son:

* Port: 3000 , Service: ppp
* Port: 8086 , Service: d-s-n
* Port: 8088 , Service: radan-http

```
[sergimc@192 grafana]$ nmap localhost

Starting Nmap 7.60 ( https://nmap.org ) at 2020-05-25 14:21 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00025s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 994 closed ports
PORT     STATE SERVICE
631/tcp  open  ipp
3000/tcp open  ppp
3306/tcp open  mysql
5432/tcp open  postgresql
8086/tcp open  d-s-n
8088/tcp open  radan-http

Nmap done: 1 IP address (1 host up) scanned in 0.13 seconds
```
