# Use DHI images

## Build a new container image version

```bash
docker build -t web:dhi --sbom=true --provenance=mode=max .
```

## Scan the DHI images using Trivy with Vex statements

## Python

Part 1: Collect Vex statements:
```bash
docker scout vex get web:dhi -o vex-python-dhi.json
```

Part 2: Use Vex statements to scan DHI image:
```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v .:/opt aquasec/trivy image --scanners vuln --vex /opt/vex-python-dhi.json web:dhi
```

Review Trivy output.

## Postgres

Part 1: Collect Vex statements:
```bash
docker scout vex get registry://docker.io/$$org$$/dhi-postgres:17.7 -o vex-postgres.json
```

Part 2: Use Vex statements to scan DHI image:
```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v .:/opt aquasec/trivy image --scanners vuln --vex /opt/vex-postgres.json docker.io/$$org$$/dhi-postgres:17.7
```

Review Trivy output for the hardened images.

## Scan the DHI images using Docker Scout

```bash
docker scout quickview web:dhi
```

```bash
docker scout quickview docker.io/$$org$$/dhi-postgres:17.7
```

Compare the before and after:
```bash
docker scout compare --to web:init web:dhi
```