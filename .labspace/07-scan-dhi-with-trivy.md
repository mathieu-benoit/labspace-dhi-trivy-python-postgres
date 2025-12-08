# Scan DHI images with Trivy

Scan the Python app image:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v .:/opt aquasec/trivy image --scanners vuln web:dhi
```

Review the vulnerabilities.

```none no-copy-button
web:dhi (debian 13.2)
48 (UNKNOWN: 0, LOW: 41, MEDIUM: 7, HIGH: 0, CRITICAL: 0)
```

Scan the PostgreSQL image:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v .:/opt aquasec/trivy image --scanners vuln $$org$$/dhi-postgres:17.7
```

Review the vulnerabilities.

```none no-copy-button
$$org$$/dhi-postgres:17.7 (debian 13.2)
59 (UNKNOWN: 0, LOW: 57, MEDIUM: 1, HIGH: 1, CRITICAL: 0)
```