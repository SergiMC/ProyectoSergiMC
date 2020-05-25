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

