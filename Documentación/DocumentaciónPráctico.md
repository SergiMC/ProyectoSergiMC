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

## Comprobación del funcionamiento de Influxdb y Telegraf

* **Telegraf**

Para comprobar que telegraf funciona correctamente pasando las métricas a influxdb, deberemos configurar anteriormente telegraf para que todas las métricas que vayan a ser enviadas a influxdb, sean escritas en un fichero con extension .JSON

```
# OUTPUT FILES
[[outputs.file]]
  ## Files to write to, "stdout" is a specially handled file.
  files = ["stdout", "/tmp/datos.js"]

  ## Data format to output.
  ## Each data format has its own unique set of configuration options, read
  ## more about them here:
  ## https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md
  data_format = "json"

  ## The resolution to use for the metric timestamp.  Must be a duration string
  ## such as "1ns", "1us", "1ms", "10ms", "1s".  Durations are truncated to
  ## the power of 10 less than the specified units.
  json_timestamp_units = "1s"
                               
```

Entraremos en el docker e comprobamos en la ruta que hemos configurado si el fichero recibe las métricas.

```
[sergimc@192 grafana]$ docker exec -it telegraf /bin/bash
```
Datos del fichero:

```
root@telegraf:/# ls /tmp/datos.js 
/tmp/datos.js
root@telegraf:/# more /tmp/datos.js 
{"fields":{"usage_guest":0,"usage_guest_nice":0,"usage_idle":56.09943618657149,"
usage_iowait":21.96309584828384,"usage_irq":0.17939518195797735,"usage_nice":0,"
usage_softirq":0.10251153254741303,"usage_steal":0,"usage_system":1.204510507432
1258,"usage_user":20.451050743209382},"name":"cpu","tags":{"cpu":"cpu-total","ho
st":"telegraf"},"timestamp":1590666150}
{"fields":{"usage_guest":0,"usage_guest_nice":0,"usage_idle":15.794871794871423,
"usage_iowait":8.71794871794866,"usage_irq":0.30769230769231026,"usage_nice":0,"
usage_softirq":0.10256410256410493,"usage_steal":0,"usage_system":1.025641025641
0402,"usage_user":74.05128205128204},"name":"cpu","tags":{"cpu":"cpu3","host":"t
elegraf"},"timestamp":1590666150}
{"fields":{"usage_guest":0,"usage_guest_nice":0,"usage_idle":55.578300921185736,
"usage_iowait":40.020470829068614,"usage_irq":0.1023541453428844,"usage_nice":0,
"usage_softirq":0.10235414534288895,"usage_steal":0,"usage_system":1.43295803480
04178,"usage_user":2.7635619242580423},"name":"cpu","tags":{"cpu":"cpu2","host":
"telegraf"},"timestamp":1590666150}
```

* **InfluxDB**

Para comprobar que influxdb está captando las métricas que le envía telegraf, deberemos entrar en el docker de influx y entrar a la base de datos mostrando las bases y sus measurements.

```
[sergimc@192 grafana]$ docker exec -it influxdb /bin/bash
```

Mostramos las bases de datos y los measurements.

```
root@influxdb:/# influx 
Connected to http://localhost:8086 version 1.8.0
InfluxDB shell version: 1.8.0
> show databases
name: databases
name
----
telegraf
_internal
> use telegraf
Using database telegraf
> show measurements
name: measurements
name
----
cpu
disk
mem
```



## Comprobación del funcionamiento de Grafana

Para comprobar el funcionamiento de grafana, nos conectaremos a grafana a través del navegador: http://ip-del-docker:3000.
Nos conectaremos a grafana con las credenciales que nos proporciona grafana, user: admin , password: admin.

![intro](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/intro.png)

Ya conectados, pasaremos a connectar e implementar nuestra base de datos de influxdb con las métricas que nos proporciona telegraf.

FOTO

Una vez agregada ya la base de datos, crearemos nuestro dashboard con una serie de paneles para visualizar dichos datos que hemos configurado.

FOTO



