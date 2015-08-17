# Postgres

Custom docker container build for PostgreSQL.

## Features

* Build on top of official [Ubuntu container](https://hub.docker.com/_/ubuntu/)
* Installs PostgreSQL by using `apt-get` which makes it really easy to add any other extensions (e.g. like PostGIS)
* Implements custom `ENTRYPOINT` which automatically runs PostgreSQL commands as `postgres` user:  
  `docker run -it --rm miki725/postgres psql  # runs under postgres user`
* Designed to make it easy to integrate with [`miki725/pgdata`](https://hub.docker.com/r/miki725/pgdata/) to use as persistent data volume
* The entrypoint automatically initializes the db at the data location if one is not already present. That is really useful when starting a fresh app.
* Exposes port `5432` by default

## Usage

As mentioned above, this container is meant to be used together with [`miki725/pgdata`](https://hub.docker.com/r/miki725/pgdata/) for persistent storage:

```bash
$ docker run --name=mypgdata miki725/pgdata
$ docker run --name=mypgdb --volumes-from=mypgdata miki725/postgres
```

The above creates a dummy container based on top of `miki725/pgdata` which is used by `miki725/postgres` as volume where PostgreSQL will store its database.

Alternatively `docker-compose` can be used:

```yaml
pgdata:
  image: miki725/pgdata

postgres:
  image: miki725/postgres
  expose:
    - 5432
  volumes_from:
    - pgdata
```
