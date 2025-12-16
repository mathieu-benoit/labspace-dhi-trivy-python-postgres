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
docker scout quickview web:init --org $$org$$
```

Review the Docker Scout quickview.

```none no-copy-button
  Target             │  web:init          │    0C     0H     1M    20L   
    digest           │  839f3fa5e03f      │                              
  Base image         │  python:3.13-slim  │    0C     0H     1M    20L   
  Updated base image │  python:3.14-slim  │    0C     0H     1M    20L   
                     │                    │                              

Policy status  FAILED  (7/10 policies met)

  Status │                              Policy                              │           Results            
─────────┼──────────────────────────────────────────────────────────────────┼──────────────────────────────
  ✓      │ AGPL v3 licenses found                                           │    0 packages                
  !      │ No default non-root user found                                   │                              
  ✓      │ No AGPL v3 licenses                                              │    0 packages                
  ✓      │ No embedded secrets                                              │    0 deviations              
  ✓      │ No embedded secrets (Rego)                                       │    0 deviations              
  ✓      │ No fixable critical or high vulnerabilities                      │    0C     0H     0M     0L   
  ✓      │ No high-profile vulnerabilities                                  │    0C     0H     0M     0L   
  !      │ Unapproved base images found                                     │    1 deviation               
  ✓      │ Supply chain attestations                                        │    0 deviations              
  !      │ Invalid or Missing Docker Hardened Image (DHI) or DHI base image │    1 deviation
```

Get the details of the CVEs:

```bash
docker scout cves web:init --org $$org$$
```

Review the vulnerabilities.

## Scan the PostgreSQL image with Docker Scout

Scan the PostgreSQL image with Docker Scout

```bash
docker scout quickview postgres:18.1 --org $$org$$
```

Review the Docker Scout quickview.

```none no-copy-button
  Target     │  postgres:18.1            │    0C     5H     8M    34L   
    digest   │  1f822adace81             │                              
  Base image │  oisupport/staging-amd64  │
```

Get the details of the CVEs:

```bash
docker scout cves postgres:18.1 --org $$org$$
```

Review the vulnerabilities.