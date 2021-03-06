# GRAFANA

Grafana es un software libre basado en la licencia de Apache 2.0 creada por Torkel Ödegaard, en enero de 2014. Está desenvolupada en Lenguaje Go (Google) y Node.js LTS y con Interfaz de Programación de Aplicaciones (API).

Grafana es una herramienta de análisis para métricas, que permite consultar,visualizar,alterar y comprender los datos independientemente de donde estén almacenados. Permite crear dashboards y explorarlos dentro de nuestro equipo. Grafana es conocido por ser una de las mejores herramientas de visualización de tableros.

Respecto a los datos que podemos usar en grafana,existe una gran variedad de información interna de la propia herramienta que podemos consultar:

* Como datos de estado de recursos (CPU, RAM, disco).
* Servicios como MySQL, PostgreSQL, Apache, influxDB, etc.

## Características de Grafana

Grafana está compuesto por una serie de características: 

* Grafana dispone de una serie de dashboards y paneles (mas de 600) para la visualización de los datos y con varias opciones.
* Grafana proporciona el uso de más de un dashboard y paneles que se encuentran en la biblioteca oficial
* Grafana permite la autenticación a través de LDAP,Google Auth,grafana y github.
* Grafana permite el intercambio de datos y dashboards entre equipos.
* Grafana permite crear alertas de seguridad en los casos que necesitemos o nos sea útil.
* Grafana nos ayuda a rastrear el comportamiento del usuario, de la aplicación y de los errores.

## Paneles de Grafana y el dashboard.

![Tablerografana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/TableroGrafanaEj.png)

Un dashboard es un cuadro formado por un conjunto de paneles creados en grafana. Cada dashboard puede estar compuesto por más de un panel personalizado y con distintos datos.

Un panel es el bloque de construcción básico para la visualización de los datos, los paneles extraen los datos de la base de datos que hemos insertado, ya sea de **MySQL**, **PostgresSQL**, **influxDB**, etc.

Estos paneles ontienen una cantidad de opciones de visualización como mapas geográficos, mapas térmicos, histogramas, en general son los tipos de visualización que una empresa requiere para los estudios de los datos.


## Ficheros de Grafana importantes.

Deberemos tener en cuenta los ficheros de grafana y su ubicación una vez terminada la instalación.

* El *fichero binario* se ubica en **/usr/sbin/grafana-server**.
* El *script Init.d* de inicio se ubica en **/etc/init.d/grafana-server**.
* El fichero por defecto (debemos crearlo) se ubica en **/etc/default/grafana-server**.
* El fichero de configuración se ubica en **/etc/grafana/grafana.ini**.
* El fichero de los registros se ubica en **/var/log/grafana/grafana.log**.
* El fichero de la base de datos (db sqlite3) se ubica en **/var/lib/grafana/grafana.db**.

## Grafana (versión gráfica)

* Introducción (Uso de grafana en modo gráfico)

En primer lugar, deberemos descargar grafana para obtener la versión gráfica.
Una vez instalado grafana, pondremos en marcha el servidor y iremos a nuestro navegador y insertaremos en la URL:
**localhost:3000**. Utilizaremos el puerto 3000 ya que grafana escucha y utiliza dicho puerto. Nos registraremos con las credenciales: **admin/admin**.

![portalgrafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/portalGrafana.png)

Iniciado grafana, nos redigrigirá a su pantalla inicial formada por un dashboard que utiliza por defecto, indicando las tareas que debemos seguir para su correcto funcionamiento.

Estas tareas están formadas por : *Instalar Grafana*, *Añadir una fuente de datos*, *Crear nuestro dashboard*, *Añadir a usuarios*, *El uso de plug-ins*.

![iniciografana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/inicioGrafana.png)

Siguiendo la siguiente tarea, añadiremos una fuente de datos o una base de datos, en nuestro caso, haremos un ejemplo con la base de datos influxDB. Una vez dentro de la opción de añadir una base de datos, proporcionaremos los datos que nos pide:

***Nombre de la base de datos:** InfluxDB*.
***URL:** localhost:8086 (InfluxDB utiliza el puerto 8086)*.
***Database:*** telegraf (el nombre de la base de datos creada)*.

![inserciongrafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/inserciondatos.png)

Una vez hayamos añadido la información de la base de datos, guardaremos clicando la opción de **save and test**. Al acabar la configuración, nos saldrán varias opciones: 

* *Data Sources*: Opción donde se reflejan la fuente de datos que hemos configurado y añadido.
* *Users*: Opción donde se refleja lo usuarios.
* *Teams*: Opción que se utiliza para agrupar usuarios.
* *Plugins*: Opción para descargar plugins (tipo panel,fuentes de datos,aplicación).
* *Preferences*: Opción que se utiliza para cambiar la temática y el diseño a nuestro gusto.
* *Api-keys*: Opción Api para conectar e interactuar.

Acabada la configuración de la fuente de datos, haremos la siguiente tarea, crearemos un dashboard. Podemos hacerlo desde el seguimiento de tareas o desde la barra de configuración.

Método desde el seguimiento de tareas:

![dashboard1grafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/creaciondashb.png)

Método desde la barra de configuración:

![dashboard2grafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/crear1.png)

Una vez queramos crear un panel, tendremos 2 opciones, crear una query o elegir un tipo de gráfico. Si elegimos crear una query, directamente utilizaremos el panel predeterminado, si elegimos la opción de que tipo de gráfico elegir, tendremos varias opciones:

![graficosgrafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/tiposgraficos.png)

Elegido y configuado el panel,deberemos hacer una consulta escogiendo los datos y campos que necesitemos. Por ejemplo ver el disco libre que tenemos:

![querygrafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/query.png)

Finalmente con la query añadida, nos saldrá nuestro panel con los datos configurados, en este caso, hemos hecho 3 paneles con 3 querys distinas. Grafana proporicona diferentes medidas para cualquier tipo de dato. 

![finalgrafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/datos.png)


