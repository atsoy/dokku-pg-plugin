PostgreSQL plugin for Dokku
---------------------------

Project: https://github.com/progrium/dokku

PostgreSQL version 9.4
Removed dump + restore.
Added mount options.

**Warning: This plugin is under development and still only tested with the below dependencies**

Requirements
------------
* Docker version `0.7.2` or higher
* Dokku version `0.2.1` or higher

Installation
------------
```
cd /var/lib/dokku/plugins
git clone https://github.com/atsoy/dokku-pg-plugin postgresql
dokku plugins-install
```


Commands
--------
```
$ dokku help
    postgresql:console <db>                        Open a PostgreSQL console
    postgresql:create <db> <mount-point>                        Create a PostgreSQL container with optional mount point.
    postgresql:delete <db>                         Delete specified PostgreSQL container
    postgresql:info <db>                           Display database informations
    postgresql:link <app> <db>                     Link an app to a PostgreSQL database
    postgresql:list                                Display list of PostgreSQL containers
    postgresql:logs <db>                           Display last logs from PostgreSQL container
```

Simple usage
------------

Create a new DB:
```
$ dokku postgresql:create foo </mnt/foo>            # Server side
$ ssh dokku@server postgresql:create foo </mnt/foo> # Client side

-----> PostgreSQL container created: postgresql/foo

       Host: 172.17.42.1
       User: 'root'
       Password: 'RDSBYlUrOYMtndKb'
       Database: 'db'
       Public port: 49187
```
- `</mnt/foo>` - optional mount point for your `/opt/postgresql` folder inside container. (You have to create your mount point folder before).
- without `<mount point>` it will use standard path `/var/lib/docker/vfs/dir/%container_id%`


Deploy your app with the same name (client side):
```
$ git remote add dokku git@server:foo
$ git push dokku master

```

Link your app to the database
```bash
dokku postgresql:link app_name database_name
```


Advanced usage
--------------

Inititalize the database with SQL statements:
```
cat init.sql | dokku postgresql:create foo
```

Open a PostgreSQL console for specified database:
```
dokku postgresql:console foo
```

Deleting databases:
```
dokku postgresql:delete foo
```

Linking an app to a specific database:
```
dokku postgresql:link foo bar
```

PostgreSQL logs (per database):
```
dokku postgresql:logs foo
```

Database information:
```
dokku postgresql:info foo
```

List of containers:
```
dokku postgresql:list
```
