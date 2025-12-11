# Scan the DHI images with Trivy

## Scan the Python app image

Scan the Python app image:

```bash
trivy image --scanners vuln web:dhi
```

Review the vulnerabilities.

```none no-copy-button
web:dhi (debian 13.2)
Total: 48 (UNKNOWN: 0, LOW: 41, MEDIUM: 7, HIGH: 0, CRITICAL: 0)
```

## Set up DHI VEX as a Trivy VEX repository

Initialize the VEX repositories config file:

```bash
trivy vex repo init
```

Add the DHI VEX repository in the :fileLink[repository.yaml]{path="~/.trivy/vex/repository.yaml"} file.

```yaml save-as=~/.trivy/vex/repository.yaml
repositories:
  - name: default
    url: https://github.com/aquasecurity/vexhub
    enabled: true
    username: ""
    password: ""
    token: ""
  - name: dhi-vex
    url: https://github.com/docker-hardened-images/advisories
    enabled: true
    username: ""
    password: ""
```

Download the DHI VEX repo in local Trivy cache:

```bash
trivy vex repo download
```

```bash
trivy vex repo list
```

```none no-copy-button
VEX Repositories (config: /home/coder/.trivy/vex/repository.yaml)

- Name: default
  URL: https://github.com/aquasecurity/vexhub
  Status: Enabled

- Name: dhi-vex
  URL: https://github.com/docker-hardened-images/advisories
  Status: Enabled
```

## Scan the Python app image with the DHI VEX

Two options:

1. Get the DHI VEX file:

```bash
docker scout vex get web:dhi --output web-dhi-vex.json
```

Scan the Python app image with the DHI VEX file:

```bash
trivy image --scanners vuln --vex ./web-dhi-vex.json web:dhi
```

2. Scan the Python app image with the DHI VEX repository:

```bash
trivy image --scanners vuln --vex repo web:dhi
```

Review the vulnerabilities.

```none no-copy-button
web:dhi (debian 13.2)
Total: 2 (UNKNOWN: 0, LOW: 2, MEDIUM: 0, HIGH: 0, CRITICAL: 0)
```

## Scan the PostgreSQL image

Scan the PostgreSQL image:

```bash
trivy image --scanners vuln $$org$$/dhi-postgres:17.7
```

Review the vulnerabilities.

```none no-copy-button
$$org$$/dhi-postgres:17.7 (debian 13.2)
59 (UNKNOWN: 0, LOW: 57, MEDIUM: 1, HIGH: 1, CRITICAL: 0)
```

## Scan the PostgreSQL image with the DHI VEX

Two options:

1. Get the DHI VEX file:

```bash
docker scout vex get $$org$$/dhi-postgres:17.7 --output postgres-dhi-vex.json
```

Scan the PostgreSQL image with the DHI VEX file:

```bash
trivy image --scanners vuln --vex ./postgres-dhi-vex.json $$org$$/dhi-postgres:17.7
```

2. Scan the PostgreSQL image with the DHI VEX repository:

```bash
trivy image --scanners vuln --vex repo $$org$$/dhi-postgres:17.7
```

Review the vulnerabilities.

```none no-copy-button
$$org$$/dhi-postgres:17.7 (debian 13.2)
Total: 1 (UNKNOWN: 0, LOW: 1, MEDIUM: 0, HIGH: 0, CRITICAL: 0)
```