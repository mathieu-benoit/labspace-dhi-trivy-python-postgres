# Scan the container images

## Scan the base Python app

```bash
docker build -t web:init --sbom=true --provenance=mode=max .
```

Using Trivy:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image web:init
```

Review the vulnerabilities.

```none no-copy-button
52 (UNKNOWN: 0, LOW: 51, MEDIUM: 1, HIGH: 0, CRITICAL: 0)
```

Using Docker Scout:

```bash
docker scout quickview web:init
```

```bash
docker scout cves web:init
```

Review the vulnerabilities.

```none no-copy-button
21 (0C     0H     1M    20L)
```

## Scan the Postgres image

Using Trivy:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image docker.io/postgres:17.7
```

Review the vulnerabilities.

```none no-copy-button
106 (UNKNOWN: 1, LOW: 98, MEDIUM: 4, HIGH: 3, CRITICAL: 0)
```

Using Docker Scout:

```bash
docker scout quickview docker.io/postgres:17.7
```

Review the vulnerabilities.

```none no-copy-button
47 (0C     5H     8M    34L)
```