# InfluxDB

InfluxDB desarrollada por influxdata, es una base de datos basada en series de tiempo (time-series database),guarda una serie de datos indexados por tiempo. En la gran mayoría de los casos, influxdb se utiliza para la visualización de sus datos en dashboards compuesto por paneles. Es una aplicación que fue lanzada el 24 de septiembre de 2013 por la empresa influxdata.

Uno de los puntos positivos que tiene influxdb es que es una base de datos de código abierto (open source) y permite trabajar con una diversidad de datos.

## Características de InfluxDB

InfluxDB está compuesto por una serie de características:

* Si los datos son exactos en todos los campos, los sobreescribe.
* Los datos están compuestos con timestamp y esto hace que se guarden en orden ascendiente para mejorar el rendimiento.
* La base de datos puede gestionar un gran volumen de lecturas y escrituras. Prioriza sobre la vista de los datos.
* La actualización de la base de datos esta restringida.
* El uso de joins no está disponible entre tablas.

Dentro de las características de InfluxDB debemos de tener en cuenta una serie de conceptos que forman influxDB:

* **Database:** Contendor que contiene series temporales,usuarios,políticas de retención...
* **Measurement:** Estructura en la que se almacena los datos.
* **Timestamp:** Es la asociación de la fecha y hora en los datos almacenados.
* **Field:** Es el par clave-valor que almacena los valores de los datos y siempre están asociados a un timestamp.
(Campos no indexados).
* **Tag:** Es el par clave-valor que almacena valores de metadatos. (Campos indexados y almacenados como string) 
* **Point:** Es el conjunto de valores de campos y tags asociados a un timestamp. 
* **Retention polity:** Describe durante cuanto tiempo mantiene influxDB los datos en la infraestructura, cantidad de copias  de seguridad crea y el tiempo asociado a los shard groups.
* **Shard:** Contiene los datos comprimidos y codificados que se guardan en un archivo del disco del servidor. Cada shard pertenece a un solo grupo de shard.

## Estructuración y almacenamiento de los datos en influxDB

* **Line Protocol**

  Es el formato de texto que utiliza InfluxDB para almacenar los datos en la base de datos.
  
  Sintaxis:
  
  ```
  +-----------+--------+-+---------+-+---------+
  |measurement|,tag_set| |field_set| |timestamp|
  +-----------+--------+-+---------+-+---------+
  ```
  Crearemos un ejemplo de datos que más adelante insertaremos en la base de datos.
  
  ```
  host_datos,host=host01 cpu_usage=30,mem_usage=60,disk_usage=50 1456738900000000000
  ```
  
* **Measurement** 

    Indica el nombre del measurement donde se van a guardar los datos.
    
    En nuestro ejemplo:
    
    ```
    host_datos
    ```

* **Tag Set**
    
    Elemento opcional que representa los tags asociados a los datos.Los tags son separados por comas sin espacios.
    
    ```
    <tag_key>=<tag_value>,<tag_key>=<tag_value>
    ```
    
    En nuestro ejemplo solo tenemos 1 tag:
    
    ```
    host=host01
    ```
  

* **Field Set**
    
    Elemento donde se indican los datos seguidos de este formato.
    
    ```
    <field_key>=<field_value>,<field_key>=<field_value> 
    ```
    En nuestro ejemplo:
    
    ```
    cpu_usage=30,mem_usage=60,disk_usage=50
    ```

* **Timestamp**

   Es un campo opcional, en caso de no especificar, InfluxDB utiliza el timestamp del servidor. Puede utilizarse                microsegundos, milisegundos o segundos.
   
   En nuestro ejemplo:
   
   ```
   1456738900000000000
   ```

## InfluxDB (En Terminal)

* Introducción (Uso de influxDB en terminal)

En primer lugar, deberemos instalar influxDB. Una vez instalado, pondremos en marcha la base de datos de influxDB a través de un terminal(consola).

Para poner en marcha la base de datos influxDB, utilizaremos el siguiente comando: **./influxdb**
```
[sergimc@192 influxdb-1.8.0-1]$ ./influxd

 8888888           .d888 888                   8888888b.  888888b.
   888            d88P"  888                   888  "Y88b 888  "88b
   888            888    888                   888    888 888  .88P
   888   88888b.  888888 888 888  888 888  888 888    888 8888888K.
   888   888 "88b 888    888 888  888  Y8bd8P' 888    888 888  "Y88b
   888   888  888 888    888 888  888   X88K   888    888 888    888
   888   888  888 888    888 Y88b 888 .d8""8b. 888  .d88P 888   d88P
 8888888 888  888 888    888  "Y88888 888  888 8888888P"  8888888P"

2020-05-05T16:43:07.873028Z	info	InfluxDB starting	{"log_id": "0MaRMSt0000", "version": "1.8.0", "branch": "1.8", "commit": "781490de48220d7695a05c29e5a36f550a4568f5"}
2020-05-05T16:43:07.873075Z	info	Go runtime	{"log_id": "0MaRMSt0000", "version": "go1.13.8", "maxprocs": 4}
2020-05-05T16:43:08.052000Z	info	Using data dir	{"log_id": "0MaRMSt0000", "service": "store", "path": "/home/sergimc/.influxdb/data"}
2020-05-05T16:43:08.052078Z	info	Compaction settings	{"log_id": "0MaRMSt0000", "service": "store", "max_concurrent_compactions": 2, "throughput_bytes_per_second": 50331648, "throughput_bytes_per_second_burst": 50331648}
2020-05-05T16:43:08.052105Z	info	Open store (start)	{"log_id": "0MaRMSt0000", "service": "store", "trace_id": "0MaRMTa0000", "op_name": "tsdb_open", "op_event": "start"}
2020-05-05T16:43:08.645644Z	info	Reading file	{"log_id": "0MaRMSt0000", "engine": "tsm1", "service": "cacheloader", "path": "/home/sergimc/.influxdb/wal/_internal/monitor/1/_00001.wal", "size": 601115}
2020-05-05T16:43:08.977222Z	info	Reading file	{"log_id": "0MaRMSt0000", "engine": "tsm1", "service": "cacheloader", "path": "/home/sergimc/.influxdb/wal/telegraf/autogen/2/_00001.wal", "size": 268713}
2020-05-05T16:43:09.611475Z	info	Opened shard	{"log_id": "0MaRMSt0000", "service": "store", "trace_id": "0MaRMTa0000", "op_name": "tsdb_open", "index_version": "inmem", "path": "/home/sergimc/.influxdb/data/_internal/monitor/1", "duration": "1162.569ms"}
2020-05-05T16:43:09.611621Z	info	Open store (end)	{"log_id": "0MaRMSt0000", "service": "store", "trace_id": "0MaRMTa0000", "op_name": "tsdb_open", "op_event": "end", "op_elapsed": "1559.515ms"}
2020-05-05T16:43:09.611787Z	info	Opened shard	{"log_id": "0MaRMSt0000", "service": "store", "trace_id": "0MaRMTa0000", "op_name": "tsdb_open", "index_version": "inmem", "path": "/home/sergimc/.influxdb/data/telegraf/autogen/2", "duration": "797.902ms"}
2020-05-05T16:43:09.623889Z	info	Opened service	{"log_id": "0MaRMSt0000", "service": "subscriber"}
2020-05-05T16:43:09.623925Z	info	Starting monitor service	{"log_id": "0MaRMSt0000", "service": "monitor"}
2020-05-05T16:43:09.623938Z	info	Registered diagnostics client	{"log_id": "0MaRMSt0000", "service": "monitor", "name": "build"}
2020-05-05T16:43:09.623951Z	info	Registered diagnostics client	{"log_id": "0MaRMSt0000", "service": "monitor", "name": "runtime"}
2020-05-05T16:43:09.623976Z	info	Registered diagnostics client	{"log_id": "0MaRMSt0000", "service": "monitor", "name": "network"}
2020-05-05T16:43:09.623994Z	info	Registered diagnostics client	{"log_id": "0MaRMSt0000", "service": "monitor", "name": "system"}
2020-05-05T16:43:09.641676Z	info	Storing statistics	{"log_id": "0MaRMSt0000", "service": "monitor", "db_instance": "_internal", "db_rp": "monitor", "interval": "10s"}
2020-05-05T16:43:09.641683Z	info	Starting precreation service	{"log_id": "0MaRMSt0000", "service": "shard-precreation", "check_interval": "10m", "advance_period": "30m"}
2020-05-05T16:43:09.641732Z	info	Starting snapshot service	{"log_id": "0MaRMSt0000", "service": "snapshot"}
2020-05-05T16:43:09.641742Z	info	Starting continuous query service	{"log_id": "0MaRMSt0000", "service": "continuous_querier"}
2020-05-05T16:43:09.642644Z	info	Starting HTTP service	{"log_id": "0MaRMSt0000", "service": "httpd", "authentication": false}
2020-05-05T16:43:09.642655Z	info	opened HTTP access log	{"log_id": "0MaRMSt0000", "service": "httpd", "path": "stderr"}
2020-05-05T16:43:09.642889Z	info	Listening on HTTP	{"log_id": "0MaRMSt0000", "service": "httpd", "addr": "[::]:8086", "https": false}
2020-05-05T16:43:09.655719Z	info	Starting retention policy enforcement service	{"log_id": "0MaRMSt0000", "service": "retention", "check_interval": "30m"}
2020-05-05T16:43:09.663109Z	info	Listening for signals	{"log_id": "0MaRMSt0000"}
2020-05-05T16:43:09.675206Z	info	Sending usage statistics to usage.influxdata.com	{"log_id": "0MaRMSt0000"}
[httpd] ::1 - - [05/May/2020:18:46:43 +0200] "GET /ping HTTP/1.1" 204 0 "-" "InfluxDBShell/1.8.0" 00795960-8ef0-11ea-8001-7427ea641d41 249
2020-05-05T16:53:09.611958Z	info	Cache snapshot (start)	{"log_id": "0MaRMSt0000", "engine": "tsm1", "trace_id": "0MaRwBQl000", "op_name": "tsm1_cache_snapshot", "op_event": "start"}
2020-05-05T16:53:09.611968Z	info	Cache snapshot (start)	{"log_id": "0MaRMSt0000", "engine": "tsm1", "trace_id": "0MaRwBQl001", "op_name": "tsm1_cache_snapshot", "op_event": "start"}
2020-05-05T16:53:09.775332Z	info	Snapshot for path written	{"log_id": "0MaRMSt0000", "engine": "tsm1", "trace_id": "0MaRwBQl000", "op_name": "tsm1_cache_snapshot", "path": "/home/sergimc/.influxdb/data/_internal/monitor/1", "duration": "163.404ms"}
2020-05-05T16:53:09.775374Z	info	Cache snapshot (end)	{"log_id": "0MaRMSt0000", "engine": "tsm1", "trace_id": "0MaRwBQl000", "op_name": "tsm1_cache_snapshot", "op_event": "end", "op_elapsed": "163.434ms"}
2020-05-05T16:53:09.786516Z	info	Snapshot for path written	{"log_id": "0MaRMSt0000", "engine": "tsm1", "trace_id": "0MaRwBQl001", "op_name": "tsm1_cache_snapshot", "path": "/home/sergimc/.influxdb/data/telegraf/autogen/2", "duration": "174.559ms"}
2020-05-05T16:53:09.786546Z	info	Cache snapshot (end)	{"log_id": "0MaRMSt0000", "engine": "tsm1", "trace_id": "0MaRwBQl001", "op_name": "tsm1_cache_snapshot", "op_event": "end", "op_elapsed": "174.576m
```

Puesta la base de datos en marcha, abriremos otro terminal para generar y ejecutar consultas:

```
[sergimc@192 influxdb-1.8.0-1]$ ./influx
Connected to http://localhost:8086 version 1.8.0
InfluxDB shell version: 1.8.0
> 
```

Debemos tener en cuenta que influxDB utiliza y escucha a través del puerto **8086**.

## El uso de comandos en influxDB

Los comandos que se utilizan y los más usados en influxDB:

* CREATE DATABASE: Para crear una base de datos

```
> CREATE DATABASE hostnames
```

* show databases : Para mostrar las bases de datos creadas.

```
> show databases
name: databases
name
----
_internal
telegraf
hostnames
```

* Use: Para usar/interaccionar con una base de datos.

```
> use hostnames
Using database hostnames
```

* Insert: Para insertar datos en la base de datos:

```
INSERT host_datos,host=host01 cpu_usage=30,mem_usage=60,disk_usage=50 1456738900000000000
```
* Select: Para mostrar los datos que hemos introducido.

```
> select * from host_datos
name: host_datos
time                cpu_usage disk_usage host   mem_usage
----                --------- ---------- ----   ---------
1456738900000000000 30        50         host01 60
```

## Comandos de los conceptos de los datos

* Show series: Muestra la información de la serie.

```
> show series
key
---
host_datos,host=host01
```

* Show tag keys: Muestra la información de las keys asociadas

```
> show tag keys
name: host_datos
tagKey
------
host
```

* Show field keys: Muestra la información de los tipos de campo que tienen las keys.

```
> show field keys
name: host_datos
fieldKey   fieldType
--------   ---------
cpu_usage  float
disk_usage float
mem_usage  float
```

## Método de consulta con interfaz HTTP API

El HTTP API es uno de los métodos de acceso y el medio principal para consultar y escribir datos en InfluxDB.
El método de consulta con HTTP API suele realizarse con *CURL*, una herramienta de línea de comando que transfiere
datos mediante **URL**.

* Uso de Comandos

    * Crear una base de datos
      ```
      curl -i -XPOST http://localhost:8086/query --data-urlencode "q=CREATE DATABASE hosts_curl"
      ```
      Resultado:
      ```
      HTTP/1.1 200 OK
      Content-Type: application/json
      Request-Id: 1a71ba0e-976b-11ea-8001-7427ea641d41
      X-Influxdb-Build: OSS
      X-Influxdb-Version: 1.8.0
      X-Request-Id: 1a71ba0e-976b-11ea-8001-7427ea641d41
      Date: Sat, 16 May 2020 11:48:04 GMT
      Transfer-Encoding: chunked

      {"results":[{"statement_id":0}]}
      ```
    

    * Insertar datos mediante comando
    
      ```
      curl -i -XPOST 'http://localhost:8086/write?db=hosts_curl' --data-binary 'host_datos,host=host01
      cpu_usage=30,mem_usage=60,disk_usage=50 1456738900000000000'
      ```
    
    * Insertar datos mediante un fichero
    
      Datos del fichero:
      
      ```
      cat inserthosts.txt 
      host_datos,host=host02 cpu_usage=60,mem_usage=50,disk_usage=60 1456738900000000000
      host_datos,host=host03 cpu_usage=50,mem_usage=30,disk_usage=30 1456738900000000000
      host_datos,host=host04 cpu_usage=30,mem_usage=30,disk_usage=40 1456738900000000000
      ```
      Insert:
      
      ```
      curl -i -XPOST 'http://localhost:8086/write?db=hosts_curl' --data-binary @inserthosts.txt
      ```
      Resultado de los dos inserts:
      
      ```
      HTTP/1.1 204 No Content
      Content-Type: application/json
      Request-Id: 2d6f16a7-9770-11ea-8023-7427ea641d41
      X-Influxdb-Build: OSS
      X-Influxdb-Version: 1.8.0
      X-Request-Id: 2d6f16a7-9770-11ea-8023-7427ea641d41
      Date: Sat, 16 May 2020 12:24:24 GMT 
      ```

## Resultado de respuestas HTTP

InfluxDB proporciona tres respuestas HTTP:

* 2xx: Si recibió su solicitud de escritura HTTP 204 No Content, ¡Todo Ok!
* 4xx: InfluxDB no pudo entender la solicitud.
* 5xx: el sistema está sobrecargado o tiene problemas importantes.

## Diferencias entre InfluxDB y base de datos SQL

InfluxDB está especialmente diseñado para datos de serie temporales, respecto a SQL, pueden trabajar con
datos de serie temporal, pero no están optimizadas para trabajar con una gran carga de datos de serie temporal.
En cambio, influxDB está diseñado para guardar grandes volúmenes de datos de serie temporal y realizar análisis 
rápidos en tiempo real.

* Terminología

  SQL proporciona el estilo de tablas:
  
  ```
    +---------+---------+---------------------+--------------+
    | park_id | planet  | time                | #_foodships  |
    +---------+---------+---------------------+--------------+
    |       1 | Earth   | 1429185600000000000 |            0 |
    |       1 | Earth   | 1429185601000000000 |            3 |
    |       1 | Earth   | 1429185602000000000 |           15 |
    |       1 | Earth   | 1429185603000000000 |           15 |
    |       2 | Saturn  | 1429185600000000000 |            5 |
    |       2 | Saturn  | 1429185601000000000 |            9 |
  
  ```
  En influxDB, se proporciona de diferente manera:
  
  ```
    name: foodships
    tags: park_id=1, planet=Earth
    time			               #_foodships
    ----			               ------------
    2015-04-16T12:00:00Z	 0
    2015-04-16T12:00:01Z	 3
    2015-04-16T12:00:02Z	 15
    2015-04-16T12:00:03Z	 15  
  ```
