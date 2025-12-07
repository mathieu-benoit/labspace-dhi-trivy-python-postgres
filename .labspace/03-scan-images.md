# Scan the container images

## First thing to get started, please provide your Docker org name

::variableDefinition[org]{prompt="What is your Docker Organization?"}

## Configure Docker Scout organization 

If your account is part of an paid organization, you may have additional output that reflects policy alignment.
```bash
docker scout config organization $$org$$
```

## Login with docker

In order to use Docker Scout to analyze the image during this lab, you will need to be logged in. Make sure that you are logged in with Docker:
```bash
docker login
```

You should see the following message:
```bash no-run-button no-copy-button
Login Succeeded
```

If not, follow the instructions to complete login.

## Scan the Python app

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