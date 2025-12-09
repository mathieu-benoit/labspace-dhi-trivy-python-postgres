# Harden the final container image

## Exec curl from the Python image using the Docker Desktop GUI

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

## Update the Dockerfile to use minimal DHI base image

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

## Scan the DHI images using Docker Scout

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
      digest          │  c0ee26e90195                                                        │  7704ea2aec6f                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  681452bc84b3d7314ec94fac1ea2eb531520f210                            │  681452bc84b3d7314ec94fac1ea2eb531520f210                             
      vulnerabilities │    0C     0H     1M     0L                                           │    0C     0H     2M     0L                                            
                      │                  -1                                                  │                                                                       
      size            │ 36 MB (-20 MB)                                                       │ 55 MB                                                                 
      packages        │ 44 (-42)                                                             │ 86                                                                    
                      │                                                                      │                                                                       
    Base image        │  demonstrationorg/dhi-python-nick:3.13-debian13                      │  demonstrationorg/dhi-python-bcg:3.13-debian13-dev                    
      tags            │ also known as                                                        │ also known as                                                         
                      │                                                                      │                                                                       
      vulnerabilities │    0C     0H     1M     0L                                           │    0C     0H     1M     0L 
```

Compare the differences between `web:hardened` and `web:init`:

```bash
docker scout compare --ignore-unchanged --to web:init web:hardened
```

```none no-copy-button
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:hardened                                                        │  web:init                                                             
      digest          │  c0ee26e90195                                                        │  7269e04be765                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  681452bc84b3d7314ec94fac1ea2eb531520f210                            │  681452bc84b3d7314ec94fac1ea2eb531520f210                             
      vulnerabilities │    0C     0H     1M     0L                                           │    0C     0H     1M    20L                                            
                      │                        -20                                           │                                                                       
      size            │ 36 MB (-28 MB)                                                       │ 64 MB                                                                 
      packages        │ 44 (-57)                                                             │ 101                                                                   
                      │                                                                      │                                                                       
    Base image        │  demonstrationorg/dhi-python-nick:3.13-debian13                      │  python:3.13-slim                                                     
      tags            │ also known as                                                        │ also known as                                                         
                      │                                                                      │   • 3.13-slim-trixie                                                  
                      │                                                                      │   • 3.13.10-slim                                                      
                      │                                                                      │   • 3.13.10-slim-trixie                                               
      vulnerabilities │    0C     0H     1M     0L                                           │    0C     0H     1M    20L
```

## Exec not possible into the new hardened image

Open the Docker Desktop GUI.

Select the `web-1` container.

Select Exec.

You shouldn't be able to make it anymore.

## Resources

- [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- [Base image hardening](https://docs.docker.com/dhi/core-concepts/hardening/)