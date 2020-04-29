# GRAFANA

Grafana es un software libre basado en la licencia de Apache 2.0 creada por Torkel Ödegaard, en enero de 2014. Está desenvolupada en Lenguaje Go (Google) y Node.js LTS y con Interfaz de Programación de Aplicaciones (API).

Grafana es una herramienta de análisis para métricas, que permite consultar,visualizar,altertar y comprender los datos independientemente de donde estén almacenados. Permite crear tableros y explorarlos dentro de nuestro equipo. Grafana es conocido por ser una de las mejores herramientas de visualización de tableros.

Respecto a los datos que podemos usar en grafana,existe una gran variedad de información interna de la propia herramienta que podemos consultar:

* Como datos de estado de recursos (CPU, RAM, disco).
* Servicios como MySQL, PostgreSQL, Apache, influxDB, etc.

## Características de Grafana

Grafana está compuesto por una serie de características: 

* Grafana dispone de una serie de tableros y paneles (mas de 600) para la visualización de los datos y con varias opciones.
* Grafana proporciona el uso de más de un tablero y paneles que se encuentran en la biblioteca oficial
* Grafana permite la autenticación a través de LDAP,Google Auth,grafana y github.
* Grafana permite el intercambio de datos y tableros entre equipos.
* Grafana permite crear alertas de seguridad en los casos que necesitemos o nos sea útil.
* Grafana nos ayuda a rastrear el comportamiento del usuario, de la aplicación y de los errores.

## Tableros de Grafana

![grafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/TableroGrafanaEj.png)

Los tableros extraen los datos de la base de datos que hemos insertado, ya sea de **MySQL**, **PostgresSQL**, **influxDB**, etc.

Los paneles contienen una cantidad de opciones de visualización como mapas geográficos, mapas térmicos, histogramas, en general son los tipos de visualización que una empresa requiere para los estudios de los datos.


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

![grafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/portalGrafana.png)

Iniciado grafana, nos redigrigirá a su pantalla inicial formada por un dashboard que utiliza por defecto, indicando las tareas que debemos seguir para su correcto funcionamiento.

![grafana](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/inicioGrafana.png)


