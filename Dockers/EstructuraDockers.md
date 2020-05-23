## Estructura de Dockers

![Esquema](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/esquema.jpg)

La estructura del esquema está compuesta por 3 dockers:

* Docker de Telegraf
* Docker de InfluxDB
* Docker de Grafana

Externo al docker:
* Navegador web : http://ip-dockergrafana:3000

Con el docker de telegraf, recopilaremos los datos de cpu,ram,disk de nuestro pc y enviaremos las métricas a InfluxDB. 
Enviada las réplicas, utilizaremos el docker grafana para visualizar los datos creando un dashboard y configurando los paneles.
