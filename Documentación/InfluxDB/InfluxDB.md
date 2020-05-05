# InfluxDB

InfluxDB desarrollada por influxdata, es una base de datos basada en series de tiempo (time-series database),guarda una serie de datos indexados por tiempo. En la gran mayoría de los casos, influxdb se utiliza para la visualización de sus datos en gráficas (dashboards). Es una aplicación que fue lanzada el 24 de septiembre de 2013 por la empresa influxdata.

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

## INFLUXDB (En Terminal)

* Introducción (Uso de influxDB en terminal)

En primer lugar, deberemos instalar influxDB. Una vez instalado, pondremos en marcha la base de datos de influxDB a través de un terminal(consola).

Para poner en marcha la base de datos influxDB, utilizaremos el siguiente comando:

//imagen//

Puesta la base de datos en marcha, abriremos otro terminal para generar y ejecutar consultas:

//imagen//

Debemos tener en cuenta que influxDB utiliza y escucha a través del puerto **8086**.






