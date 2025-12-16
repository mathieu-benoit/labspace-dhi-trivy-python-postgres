# Use the DHI images

## Modify the Dockerfile to use the `dhi-python` image

Change the base image in the `FROM` in the :fileLink[Dockerfile]{path="Dockerfile" line=1} and save.

```yaml
FROM $$org$$/dhi-python:3.14-debian13-dev
```

```diff no-copy-button
- FROM python:3.14-slim
+ FROM $$org$$/dhi-python:3.14-debian13-dev
```

## Modify the Docker Compose file to use the `dhi-postgres` image

Change the db image in the :fileLink[docker-compose.yml]{path="docker-compose.yml" line=12} and save.

```yaml
image: $$org$$/dhi-postgres:18.1
```

```diff no-copy-button
- image: image: postgres:18.1
+ image: $$org$$/dhi-postgres:18.1
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
web:dhi        3d706e8d6465        243MB         58.8MB      
web:init       eb3a0f14997a        261MB         69.8MB
```

## Resources

- [Minimal or distroless images](https://docs.docker.com/dhi/core-concepts/distroless/)