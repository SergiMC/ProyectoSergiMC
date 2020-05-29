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
