# Scan the container images with Trivy

## Install Trivy

```bash
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sudo sh -s -- -b /usr/local/bin
```

## Scan the Python app with Trivy

Scan the Python app with Trivy:

```bash
trivy image --scanners vuln web:init
```

Review the vulnerabilities.

```none no-copy-button
web:init (debian 13.2)
61 (UNKNOWN: 0, LOW: 51, MEDIUM: 10, HIGH: 0, CRITICAL: 0)
```

## Scan the PostgreSQL image with Trivy

Scan the PostgreSQL image with Trivy:

```bash
trivy image --scanners vuln postgres:18.1
```

Review the vulnerabilities.

```none no-copy-button
postgres:18.1 (debian 13.2)
Total: 111 (UNKNOWN: 1, LOW: 98, MEDIUM: 11, HIGH: 1, CRITICAL: 0)

usr/local/bin/gosu (gobinary)
Total: 12 (UNKNOWN: 0, LOW: 0, MEDIUM: 8, HIGH: 4, CRITICAL: 0)
```

## Resources

- [Comparing Docker Scout results with other scanners](https://docs.docker.com/dhi/how-to/scan/#comparing-docker-scout-results-with-other-scanners)