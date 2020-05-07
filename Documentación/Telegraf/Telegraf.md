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

Activado influxDB, configuraremos telegraf creando el fichero de configuración con los plugins en este caso CPU,RAM,DISK:

```
[sergimc@192 Telegraf]$ ./telegraf -sample-config -input-filter cpu:mem:disk -output-filter influxDB > telegraf.conf

```










