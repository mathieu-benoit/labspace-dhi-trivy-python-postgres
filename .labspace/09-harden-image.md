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

```yaml save-as=Dockerfile
FROM $$org$$/dhi-python:3.14-debian13-dev AS builder
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
WORKDIR /app
RUN pip install psycopg2-binary --target /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt --target /app

FROM $$org$$/dhi-python:3.14-debian13 AS prod
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
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
web:dhi        3d706e8d6465        243MB         58.8MB        
web:hardened   9fbc4b1dc20d        163MB         37.6MB        
web:init       eb3a0f14997a        261MB         69.8MB
```

## Scan the new hardened container image with Trivy

Scan the new hardened container image with Trivy:

```bash
trivy image --scanners vuln web:hardened
```

Review the vulnerabilities.

```none no-copy-button
web:hardened (debian 13.2)
Total: 15 (UNKNOWN: 0, LOW: 13, MEDIUM: 2, HIGH: 0, CRITICAL: 0)
```

## Scan the new hardened container image with Trivy with the DHI VEX

Two options:

1. Get the DHI VEX file:

```bash
docker scout vex get web:hardened --output web-hardened-vex.json
```

Scan the new hardened container image with Trivy with the DHI VEX:

```bash
trivy image --scanners vuln --vex ./web-hardened-vex.json web:hardened
```

2. Scan the new hardened container image with the DHI VEX repository:

```bash
trivy image --scanners vuln --vex repo web:hardened
```

Review the vulnerabilities.

```none no-copy-button
web:hardened (debian 13.2)
Total: 0
```

## Scan the new hardened container image with Docker Scout

```bash
docker scout quickview web:hardened --org $$org$$
```

Review the Docker Scout quickview.

```none no-copy-button
  Target     │  web:hardened                               │    0C     0H     0M    10L   
    digest   │  6cc0194c223b                               │                              
  Base image │  demonstrationorg/dhi-python:3.14-debian13  │                              

Policy status  SUCCESS  (10/10 policies met)

  Status │                       Policy                        │           Results            
─────────┼─────────────────────────────────────────────────────┼──────────────────────────────
  ✓      │ AGPL v3 licenses found                              │    0 packages                
  ✓      │ Default non-root user                               │                              
  ✓      │ No AGPL v3 licenses                                 │    0 packages                
  ✓      │ No embedded secrets                                 │    0 deviations              
  ✓      │ No embedded secrets (Rego)                          │    0 deviations              
  ✓      │ No fixable critical or high vulnerabilities         │    0C     0H     0M     0L   
  ✓      │ No high-profile vulnerabilities                     │    0C     0H     0M     0L   
  ✓      │ No unapproved base images                           │    0 deviations              
  ✓      │ Supply chain attestations                           │    0 deviations              
  ✓      │ Valid Docker Hardened Image (DHI) or DHI base image │    0 deviations
```

- Compare the differences between `web:hardened` and `web:dhi`:

```bash
docker scout compare --ignore-unchanged --to web:dhi web:hardened
```

```none no-copy-button
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:hardened                                                        │  web:dhi                                                              
      digest          │  6cc0194c223b                                                        │  cc8632e0d4fa                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  972f63d35bc8629cc14fa3b6e84d114732772370                            │  972f63d35bc8629cc14fa3b6e84d114732772370                             
      vulnerabilities │    0C     0H     0M     0L                                           │    0C     0H     1M     0L                                            
                      │                  -1                                                  │                                                                       
      size            │ 36 MB (-20 MB)                                                       │ 56 MB                                                                 
      packages        │ 44 (-42)                                                             │ 86                                                                    
                      │                                                                      │
```

Note the -20MB reduction in size, -1 CVE reduction in vulnerabilities count and -42 reduction in packages count.

- Compare the differences between `web:hardened` and `web:init`:

```bash
docker scout compare --ignore-unchanged --to web:init web:hardened
```

```none no-copy-button
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:hardened                                                        │  web:init                                                             
      digest          │  6cc0194c223b                                                        │  839f3fa5e03f                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  972f63d35bc8629cc14fa3b6e84d114732772370                            │  972f63d35bc8629cc14fa3b6e84d114732772370                             
      vulnerabilities │    0C     0H     0M     0L                                           │    0C     0H     1M    20L                                            
                      │                  -1    -20                                           │                                                                       
      size            │ 36 MB (-31 MB)                                                       │ 68 MB                                                                 
      packages        │ 44 (-57)                                                             │ 101                                                                   
                      │                                                                      │
```

In its final version (`web:hardened`) and overall, note the -31MB reduction in size, -21 CVEs reduction in vulnerabilities count and -57 reduction in packages count.

## Exec not possible from the hardened Python container

Open the Docker Desktop GUI.

Select the `web-1` container.

Select Exec.

You shouldn't be able to make it anymore.

You have now a more secure Python container image out there!

## Resources

- [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- [Base image hardening](https://docs.docker.com/dhi/core-concepts/hardening/)