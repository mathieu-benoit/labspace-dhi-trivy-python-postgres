# Use the DHI images

## Modify the Dockerfile to use the `dhi-python` image

Change the base image in the `FROM` in the :fileLink[Dockerfile]{path="Dockerfile" line=1} and save.

```yaml
FROM $$org$$/dhi-python:3.13-debian13-dev
```

```diff no-copy-button
- FROM python:3.13-slim
+ FROM $$org$$/dhi-python:3.13-debian13-dev
```

## Modify the Docker Compose file to use the `dhi-postgres` image

Change the db image in the :fileLink[docker-compose.yml]{path="docker-compose.yml" line=12} and save.

```yaml
image: $$org$$/dhi-postgres:17.7
```

```diff no-copy-button
- image: image: postgres:17.7
+ image: $$org$$/dhi-postgres:17.7
```

## Modify the Docker Compose file to use the volume directory change for DHI PostgreSQL

Discuss the [volume directory change](https://hub.docker.com/orgs/demonstrationorg/hardened-images/catalog/dhi/postgres/guides#pgdata-migration-from-docker-official-images) for postgres: DHI PostgreSQL uses `/var/lib/postgresql/<MAJOR_VERSION>/data` instead of `/var/lib/postgresql/data`.

Update the volume mounts in the :fileLink[docker-compose.yml]{path="docker-compose.yml" line=20} to use the new versioned data directory and save.

```yaml
      - postgres_data:/var/lib/postgresql/17/data  # for PostgreSQL 17
```

```diff no-copy-button
    volumes:
-      - postgres_data:/var/lib/postgresql/data
+      - postgres_data:/var/lib/postgresql/17/data  # for PostgreSQL 17
```

## Run the application

Run the application:
```bash
docker compose up -d --build
```

Go to the application in the browser: :tabLink[http://localhost:5001]{href="http://localhost:5001" title="Web app"}.

Enter a reservation using the application to confirm that the application is still working.

## Build a new container image version

```bash
docker build -t web:dhi --sbom=true --provenance=mode=max .
```

Verify the image built:

```bash
docker images web
```

```none no-copy-button
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
web:dhi        bda37908b32e        239MB         57.9MB        
web:init       391b8587185a        256MB         65.8MB
```