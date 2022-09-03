# Notes

## Building

```bash
docker build -t="geographica/postgresql-9.1.2-postgis-1.5.8-patch" ./patch
docker build -t="geographica/postgresql-9.1.2-postgis-1.5.8-nopatch" ./patchnone


docker build --no-cache -t="geographica/postgresql-9.1.2-postgis-1.5.8-patch" ./patch
docker build --no-cache -t="geographica/postgresql-9.1.2-postgis-1.5.8-nopatch" ./patchnone
```

### Creating data folder

```bash
mkdir /home/username/works/folder/data

docker run --rm \
  -v /home/username/works/folder/data:/data/ \
  -t -i geographica/postgresql-9.1.2-postgis-1.5.8-nopatch \
  /bin/bash

chown postgres:postgres /data
chmod 700 /data

su postgres -c "initdb --encoding=UTF-8 --locale=es_ES.UTF-8 --lc-collate=es_ES.UTF-8 --lc-monetary=es_ES.UTF-8 --lc-numeric=es_ES.UTF-8 --lc-time=es_ES.UTF-8 -D /data"
```

## pg_hba.conf

On the host cd to data folder

```bash
sudo su
gedit pg_hba.conf
```

Add the following line:

```txt
host    all             all             0.0.0.0/0               trust
```

## postgresql.conf

On the host cd to data folder

```bash
sudo su
gedit postgresql.conf
```

Edit the following line

```txt
listen_addresses = '*'
```

```bash
docker run -i -t \
 --name="pgsql-9.1.2-postgis-1.5.8-nopatch" \
 -v /home/burak/works/iucn/data:/data/ \
 -p 5455:5432 \
 geographica/postgresql-9.1.2-postgis-1.5.8-nopatch
```

## Other Notes

### PostgresSQL Folders inside the Container

```txt
/usr/local/share/postgresql/contrib/
/usr/local/share/postgresql/extension/
```

### Postgis Manual

[Postgis 1.5 Manual](https://postgis.net/docs/manual-1.5/index.html)

### Create Roles and Database

```sql
CREATE ROLE iucn_admin WITH
  SUPERUSER
  CREATEDB
  CREATEROLE
  INHERIT
  LOGIN ENCRYPTED PASSWORD 'iucn_admin'
  REPLICATION
  CONNECTION LIMIT -1;

CREATE ROLE easin_interface WITH
  SUPERUSER
  CREATEDB
  CREATEROLE
  INHERIT
  LOGIN ENCRYPTED PASSWORD 'easin_interface'
  REPLICATION
  CONNECTION LIMIT -1;

CREATE DATABASE iucn_reporting
  OWNER iucn_admin
  TEMPLATE template0;
```

### Restore Backup

Container terminal

```bash
psql -U iucn_admin iucn_reporting < uicp.sql
```
