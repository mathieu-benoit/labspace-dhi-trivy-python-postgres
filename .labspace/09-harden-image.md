# Harden the final container image

## Exec from the Python container

Open the Docker Desktop GUI.

Select the `web-1` container.

Select Exec.

Run these commands to install `curl`:
```bash
apt update
apt upgrade
apt install curl
```

Try to `curl` an external link:
```bash
curl https://www.docker.com
```

Having a final container image with a shell and a package manager could bring a lot of security issues.

Let's fix this!

## Update the Dockerfile to use minimal DHI base image

For that we want to use a Distroless and minimal image approach.

Update the :fileLink[Dockerfile]{path="Dockerfile"} with this content:

```yaml
FROM $$org$$/dhi-python:3.13-debian13-dev AS builder
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
WORKDIR /app
RUN pip install psycopg2-binary --target /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt --target /app

FROM $$org$$/dhi-python:3.13-debian13 AS prod
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
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

Go to the application in the browser: :tabLink[http://localhost:5001]{href="http://localhost:5001" title="Web app"}.

Enter a reservation using the application to confirm that the application is still working.

## Build the new hardened container image version

```bash
docker build -t web:hardened --sbom=true --provenance=mode=max .
```

Verify the image built:

```bash
docker images web
```

```none no-copy-button
IMAGE            ID             DISK USAGE   CONTENT SIZE   EXTRA
web:dhi          bda37908b32e        239MB         57.9MB        
web:hardened     9924f220528d        160MB         35.5MB        
web:init         fc63104c9a8e        262MB         69.8MB
```

## Scan the new hardened container image with Trivy

Scan the new hardened container image with Trivy:

```bash
trivy image --scanners vuln web:hardened
```

Review the vulnerabilities.

```none no-copy-button
web:hardened (debian 13.2)
15 (UNKNOWN: 0, LOW: 13, MEDIUM: 2, HIGH: 0, CRITICAL: 0)
```

## Scan the new hardened container image with Trivy with the DHI VEX

Get the DHI VEX file:

```bash
docker scout vex get web:hardened --output web-hardened-vex.json
```

Scan the new hardened container image with Trivy with the DHI VEX:

```bash
trivy image --scanners vuln --vex ./web-hardened-vex.json web:hardened
```

Review the vulnerabilities.

```none no-copy-button
web:hardened (debian 13.2)
Total: 0
```

## Scan the new hardened container image with Docker Scout

```bash
docker scout quickview web:hardened
```

Review the Docker Scout quickview.

```none no-copy-button
0C     0H     1M     0L
```

Compare the differences between `web:hardened` and `web:dhi`:

```bash
docker scout compare --ignore-unchanged --to web:dhi web:hardened
```

```none no-copy-button
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:hardened                                                        │  web:dhi                                                              
      digest          │  9e25a8ec0b0e                                                        │  5ad4dc2d90c0                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  e6752960d1876bb07739052ba0929f404703bfd1                            │  a19eea2f7b330432612ebefc0d1c44a33a62f5a5                             
      vulnerabilities │    0C     0H     0M     0L                                           │    0C     0H     1M     0L                                            
                      │                  -1                                                  │                                                                       
      size            │ 34 MB (-21 MB)                                                       │ 55 MB                                                                 
      packages        │ 44 (-42)                                                             │ 86
```

Compare the differences between `web:hardened` and `web:init`:

```bash
docker scout compare --ignore-unchanged --to web:init web:hardened
```

```none no-copy-button
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:hardened                                                        │  web:init                                                             
      digest          │  9e25a8ec0b0e                                                        │  ea7557110ca6                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  e6752960d1876bb07739052ba0929f404703bfd1                            │  94a72a184ec641823bbe0b517ed77f0253204901                             
      vulnerabilities │    0C     0H     0M     0L                                           │    0C     0H     1M    20L                                            
                      │                  -1    -20                                           │                                                                       
      size            │ 34 MB (-34 MB)                                                       │ 68 MB                                                                 
      packages        │ 44 (-57)                                                             │ 101
```

## Exec not possible from the hardened Python container

Open the Docker Desktop GUI.

Select the `web-1` container.

Select Exec.

You shouldn't be able to make it anymore.

You have now a more secure Python container image out there!

## Resources

- [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- [Base image hardening](https://docs.docker.com/dhi/core-concepts/hardening/)