# Agregar un Data Source en Grafana (Mysql)

Un data source (DSN Data source Name) es la conexión configurada a una base de datos desde un servidor. A continuación,
configuraremos un data source a grafana para crear un dashboard y visualizar los datos en los paneles. Utilizaremos como
data source, **Mysql (Postgresql)**.

## Configuración

Los pasos para la configuración:

  * 1.Crear Rol
  * 2.Crear Usuario
  * 3.Añadir Rol a la base de datos
  * 4.Uso de Grafana

---------------------------------------------------------------------------------------------------------------------------

* **1.Crear Rol**

Para que grafana pueda utilizar nuestra base de datos, es necesario que le asociemos un usuario con rol de solo lectura para las bases de datos que configuremos.

Para crear el rol, es necesario estar dentro de postgres:

```
postgres=# CREATE ROLE rol_readonly;
CREATE ROLE
```
Confirmamos que se ha creado correctamente con el comando \du:

```
postgres=# \du
                                       List of roles
  Role name   |                         Attributes                         |   M
ember of    
--------------+------------------------------------------------------------+----
------------
 isx39449342  | Create DB                                                  | {}
 postgres     | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 rol_readonly | Cannot login                                               | {}
```

* **2.Crear usuario**

Para crear el usuario que usaremos para grafana, deberemos salir del psql y lo crearemos con el comando **createuser**. Le asignaremos el rol de read_only y le daremos un password.

```
[postgres@192 ~]$ createuser -h localhost -p 5432 -U postgres -D -E -g rol_readonly -P -R -S usr_grafana
Enter password for new role: 
Enter it again:
```

Confirmamos que se ha creado correctamente el usuario con el comando \du:

```
postgres=# \du
                                       List of roles
  Role name   |                         Attributes                         |   
Member of    
--------------+------------------------------------------------------------+---
-------------
 isx39449342  | Create DB                                                  | {}
 postgres     | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 rol_readonly | Cannot login                                               | {}
 usr_grafana  |                                                            | {r
ol_readonly}
```
* **3.Añadir rol a la base de datos**

Conectaremos la base de datos que vayamos a utilizar al rol de read_only y luego añadiremos la tabla que usaremos en modo lectura al rol de read_only.

```
postgres=# \c corona_grafana 
You are now connected to database "corona_grafana" as user "postgres".
corona_grafana=# GRANT CONNECT ON DATABASE corona_grafana TO rol_readonly;
GRANT
corona_grafana=# GRANT SELECT ON datos_countries TO rol_readonly;
GRANT
```

* **4.Uso de Grafana**

Una vez hayamos configurado la parte de base de datos, accederemos a grafana e añadiremos un data source, en este caso postgresql. Para configurarlo, requeriremos:

* Usuario que hemos creado y configurado en la base de datos
* Ruta
* Nombre de la base de datos.

![configdatasource](https://github.com/SergiMC/ProyectoSergiMC/blob/master/Fotos/configdatasource.png)
