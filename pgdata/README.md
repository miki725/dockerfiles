# PgData

Container to serve as storage for running PostgreSQL containers.

## Why?

Docker containers feel really happy when they are completely stateless. That however is really hard to do with any modern application since it usually requires some sort of persistent data (e.g. database storage). That can be solved using a couple of approaches:

1. **Storing on Host**  
   Allocating storage for container on the host and mounting it inside the container as volume. That will allow the container to storage persistent data which in turn will be stored on host.  
   **PRO**  
   Relatively easy to setup since only a single flag is required when launching container  
   **CON**  
   This approach leaks some implementation details to the host and can make things more challenging when multiples of the same container need to be launched. For example when using multiple PostgreSQL db containers, each of them will need a dedicated place within the host machine to store persistent data.

2. **Storing state on the actual db running container.**  
   **PRO**  
   Super easy to setup since no other dependencies are present.  
   **CON**  
   Does not seem like a good idea since the running containers are often removed for various reasons. For example when upgrading db version will require the old container to be discarded and then new to be launched. Obviously this will complicate the matter of making the data "persistent".

3. **Using another container for storage**  
   **PRO**  
   Still relatively easy to setup. When using `docker-compose`, it simply adds another container for the app. Since this container's purpose is to simply storage data, it by-itself can be really lightweight.  
   **CON**  
   A bit more of moving pieces but I cant really think of any significant cons for this approach.

4. **Using 3rd party service such as RDS**  
   **PRO**  
   No added complexity  
   **CON**  
   More expensive

`pgdata` is a container image which implements the third option (3) from the above list. It is meant to be used a persistent data container for other running containers. Since this container is based on top of `busybox`, it is really small and lightweight.

## Usage

### Vanilla docker

Since in docker, running containers can reference volumes from non-running containers, `pgdata` container does not even have to be running in order to use it. It should be launched once to create the container after which point it can be used offline:

```bash
$ docker run --name=mypgdata miki725/pgdata
$ docker run --name=mypgdb --volumes-from=mypgdata miki725/postgres
```

The above will create a blank `mypgdata` container and immediately exit. After that, that container can be used to as a volume in another running container.

### docker-compose

Alternatively this can also be used in docker-compose:

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
