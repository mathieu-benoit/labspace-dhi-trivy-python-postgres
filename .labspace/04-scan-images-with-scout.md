# Scan the container images with Docker Scout

First thing to get started, please provide your Docker org name

::variableDefinition[org]{prompt="What is your Docker Organization?"}

## Configure Docker Scout organization 

If your account is part of a paid organization, you may have additional output that reflects policy alignment.
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

## Scan the Python app with Docker Scout

Scan the Python app with Docker Scout:

```bash
docker scout quickview web:init
```

Review the Docker Scout quickview.

```none no-copy-button
0C     0H     1M    20L
```

Get the details of the CVEs:

```bash
docker scout cves web:init
```

Review the vulnerabilities.

## Scan the PostgreSQL image with Docker Scout

Scan the PostgreSQL image with Docker Scout

```bash
docker scout quickview postgres:17.7
```

Review the Docker Scout quickview.

```none no-copy-button
47 (0C     5H     8M    34L)
```

Get the details of the CVEs:

```bash
docker scout cves postgres:17.7
```

Review the vulnerabilities.