# Notes

```bash
docker build -t="geographica/postgresql-9.1.2-postgis-1.5.8-patch" ./full
docker build -t="geographica/postgresql-9.1.2-postgis-1.5.8-nopatch" ./no-patch


docker build --no-cache -t="geographica/postgresql-9.1.2-postgis-1.5.8-patch" ./full
docker build --no-cache -t="geographica/postgresql-9.1.2-postgis-1.5.8-nopatch" ./no-patch
```

mkdir /home/burak/works/iucn/data

docker run --rm -v /home/burak/works/iucn/data:/data/ -t -i geographica/postgresql-9.1.2-postgis-1.5.8-nopatch /bin/bash

chown postgres:postgres /data
chmod 700 /data

su postgres -c "initdb --encoding=UTF-8 --locale=es_ES.UTF-8 --lc-collate=es_ES.UTF-8 --lc-monetary=es_ES.UTF-8 --lc-numeric=es_ES.UTF-8 --lc-time=es_ES.UTF-8 -D /data"

gedit /data/pg_hba.conf
host    all             all             0.0.0.0/0               trust

gedit /data/postgresql.conf
listen_addresses = '*'

docker run -i -t --name="pgsql-9.1.2-postgis-1.5.8-nopatch" -v /home/burak/works/iucn/data:/data/ -p 5455:5432 geographica/postgresql-9.1.2-postgis-1.5.8-nopatch



/usr/local/share/postgresql/contrib/
/usr/local/share/postgresql/extension/

https://postgis.net/docs/manual-1.5/index.html

CREATE ROLE iucn_admin WITH
	SUPERUSER
	CREATEDB
	CREATEROLE
	INHERIT
	LOGIN
	REPLICATION
	CONNECTION LIMIT -1;

CREATE ROLE easin_interface WITH
	SUPERUSER
	CREATEDB
	CREATEROLE
	INHERIT
	LOGIN
	REPLICATION
	CONNECTION LIMIT -1;

  psql -U iucn_admin iucn_reporting < uicp.sql
