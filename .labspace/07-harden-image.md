# Harden final container image

## Exec curl from the python image using the Docker Desktop GUI

Open the Docker Desktop GUI.

Select the `web-1` container.

Select Exec.

Enter `wget https://www.docker.com`.
Or.
Enter `curl https://www.docker.com`.

## Update the Dockerfile to use minimal DHI base image

Update the :fileLink[Dockerfile]{path="Dockerfile"} with this content:

```yaml
FROM $$org$$/dhi-python:3.13-alpine3.22-dev AS builder
WORKDIR /app
RUN pip install psycopg2-binary --target /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt --target /app

FROM $$org$$/dhi-python:3.13-alpine3.22 AS prod
COPY --from=builder /app /app
COPY . /app
EXPOSE 5000
CMD ["python", "/app/app.py"]
```

## Run the application

Run the application:
```bash
docker compose up -d --build
```

Go to the application in the browser: :tabLink[http://localhost:5000]{href="http://localhost:5000" title="Web app"}.

Enter a reservation using the application to confirm that the application is still working.

## Build the new hardened container image version

```bash
docker build -t web:hardened --sbom=true --provenance=mode=max .
```

## Scan the container image with Trivy

Collect Vex statements:
```bash
docker scout vex get web:dhi -o vex-python-hardened.json
```

Use Vex statements to scan DHI image:
```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v .:/opt aquasec/trivy image --scanners vuln --vex /opt/vex-python-hardened.json web:hardened
```

Review Trivy output.

## Scan the DHI images using Docker Scout

```bash
docker scout quickview web:hardened
```

Compare the before and after:

```bash
docker scout compare --to web:dhi web:hardened
```

```bash
docker scout compare --to web:init web:hardened
```

## Exec not possible anymore

Open the Docker Desktop GUI.

Select the `web-1` container.

Select Exec.

You shouldn't be able to make it.