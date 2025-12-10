# Docker Hardened Images (DHI), Trivy and Docker Scout with Python & PostgreSQL Labspace

> A repository containing a Restaurant reservation application consisting of a basic Python app talking to a PostgreSQL database to demonstrate the use of Docker Hardened Images (DHI) with Trivy and Docker Scout scanning.

## Run this Labspace

You can run this Labspace in its latest version from anywhere (if you have Docker and Docker Compose installed):

```bash
docker compose -f oci://ghcr.io/mathieu-benoit/labspace-dhi-trivy-python-postgres:latest up
```

## Contribute to this Labspace

After you forked this GitHub repository, you can run this Labspace locally by running this commands:

On Mac/Linux:

```bash
CONTENT_PATH=$PWD docker compose -f oci://dockersamples/labspace-content-dev -f .labspace/compose.override.yaml up
```

On Windows with PowerShell:

```bash
$Env:CONTENT_PATH = (Get-Location).Path; docker compose -f oci://dockersamples/labspace-content-dev -f .labspace/compose.override.yaml up
```
