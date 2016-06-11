# Postgres

Custom docker container build for PostgreSQL.

## Features

* Build on top of official [Ubuntu container](https://hub.docker.com/_/ubuntu/)
* Installs PostgreSQL by using `apt-get` which makes it really easy to add any other extensions (e.g. like PostGIS)
* Implements custom `ENTRYPOINT` which automatically runs PostgreSQL commands as `postgres` user:  
  `docker run -it --rm miki725/postgres psql  # runs under postgres user`
* Designed to make it easy to integrate with data volumes
* The entrypoint automatically initializes the db at the data location if one is not already present. That is really useful when starting a fresh app.
* Exposes port `5432` by default

## Usage

As mentioned above, this container is meant to be used together volumes for for persistent storage:

```bash
$ docker volume create --name pgdata
$ docker run --name=mypgdb \
             --volume=pgdata:/var/lib/postgresql/data \
             miki725/postgres
```

The above creates a volume which is used by `miki725/postgres` to store its database data.

Alternatively `docker-compose` can be used (only after the volume is created):

```yaml
postgres:
  image: miki725/postgres
  expose:
    - 5432
  volumes:
    - pgdata:/var/lib/postgresql/data
```
