# Telegraf

Telegraf es un servicio que tiene la función de recopilar,enviar métricas y datos de diferentes sistemas. Telegraf nos permite recopila datos del sistema el cual se está usando, datos como CPU,RAM,DISK. Puede recopilar datos tanto como INPUTS o OUTPUTS, un ejemplo son las base de datos o servicios. Telegraf proporciona una gran lista de plugins de entrada, un ejemplo sería Apache, dockers.

## Características de Telegraf

* Escrito en Go (lenguaje de programación). Se compila en un único binario sin dependencias externas.
* Consumo mínimo de memoria.
* Sistema de plugin que permite una fácil inserción de nuevos inputs y outputs.
* Gran numero de plugins para la mayoría de los servicios mas populares y APIs.

## Uso de telegraf

* Utilizaremos telegraf con los plugins CPU,RAM,DISK para la visualización de los datos.

Debemos tener en cuenta que para el uso de telegraf, necesitaremos tener configurado y usando al mismo tiempo la base de datos influxDB por tal que recopile los datos del servicio telegraf y los pueda mostrar como (OUTPUT).

En primer lugar deberemos tener instalado tanto influxDB como telegraf. Una vez hayamos instalado y configurado influxDB, pasaremos a activarlo, para todos estos pasos podemos consultar la documentación de influxDB: (link).

Activado influxDB, configuraremos telegraf creando el fichero de configuración con los plugins en este caso CPU,RAM,DISK el cual pasará los datos a influxDB.

```
[sergimc@192 Telegraf]$ ./telegraf -sample-config -input-filter cpu:mem:disk -output-filter influxDB > telegraf.conf

```
Ejecutamos telegraf con el fichero de configuración que hemos creado en /usr/bin/ de la carpeta telegraf:

```
[sergimc@192 bin]$ ./telegraf --config telegraf.conf
2020-05-07T15:44:30Z I! Starting Telegraf 1.14.2
2020-05-07T15:44:30Z I! Loaded inputs: cpu disk mem
2020-05-07T15:44:30Z I! Loaded aggregators: 
2020-05-07T15:44:30Z I! Loaded processors: 
2020-05-07T15:44:30Z I! Loaded outputs: influxdb
2020-05-07T15:44:30Z I! Tags enabled: host=192.168.1.138
2020-05-07T15:44:30Z I! [agent] Config: Interval:10s, Quiet:false, Hostname:"192.168.1.138", Flush Interval:10s

```
Entramos en influxDB y confirmamos si se ha creado correctamente la base de datos.

```
[sergimc@192 influxdb-1.8.0-1]$ ./influx 
Connected to http://localhost:8086 version 1.8.0
InfluxDB shell version: 1.8.0
> show databases
name: databases
name
----
_internal
telegraf <--- base creada con nombre telegraf
```
Entramos dentro de la base telegraf y confirmamos si estan los measurements que hemos configurado.

```
> show measurements
name: measurements
name
----
cpu
disk
mem

```







