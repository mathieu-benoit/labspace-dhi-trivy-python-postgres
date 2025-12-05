# Use DHI images

## Modify Dockerfile

Change the image in the `FROM` in the `Dockerfile` and save.

```yaml
FROM $$org$$/dhi-python:3.13-alpine3.22-dev
```

```diff no-copy-button
- FROM python:3.13-slim
+ FROM $$org$$/dhi-python:3.13-alpine3.22-dev_curl
```

## Modify Docker Compose file

Change the db image in the Docker Compose file and save.

```yaml
image: $$org$$/dhi-postgres:17.7
```

```diff no-copy-button
- image: image: postgres:17.7
+ image: $$org$$/dhi-postgres:17.7
```

## Run the application

Run the application:
```bash
docker compose up -d --build
```

Go to the application in the browser: :tabLink[http://localhost:5000]{href="http://localhost:5000" title="Web app"}.

Enter a reservation using the application to confirm that the application is still working.

## Discuss the volume directory change for postgres

FIXME - explain what and why

Discuss the volume directory change for postgres.