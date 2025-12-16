# Scan the DHI images with Docker Scout

## Scan the Python app image

Scan the Python app image:

```bash
docker scout quickview web:dhi --org $$org$$
```

Review the Docker Scout quickview.

```none no-copy-button
  Target     │  web:dhi                                        │    0C     0H     1M     0L   
    digest   │  cc8632e0d4fa                                   │                              
  Base image │  demonstrationorg/dhi-python:3.14-debian13-dev  │                              

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

Get the details of the CVEs:

```bash
docker scout cves web:dhi --org $$org$$
```

Review the vulnerabilities.

## Scan the PosgreSQL image

Scan the PosgreSQL image:

```bash
docker scout quickview $$org$$/dhi-postgres:18.1 --org $$org$$
```

Review the Docker Scout quickview.

```none no-copy-button
  Target   │  demonstrationorg/dhi-postgres:18.1  │    0C     0H     0M     1L   
    digest │  26c2a1b1ccf7                        │                              

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

Get the details of the CVEs:

```bash
docker scout cves postgres:18.1
```

Review the vulnerabilities.

## Compare between the before and after for the Python app

Compare between the before and after for the Python app:

```bash
docker scout compare --ignore-unchanged --to web:init web:dhi --org $$org$$
```

```none no-copy-button
  ## Overview
  
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:dhi                                                             │  web:init                                                             
      digest          │  cc8632e0d4fa                                                        │  839f3fa5e03f                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  972f63d35bc8629cc14fa3b6e84d114732772370                            │  972f63d35bc8629cc14fa3b6e84d114732772370                             
      vulnerabilities │    0C     0H     1M     0L                                           │    0C     0H     1M    20L                                            
                      │                        -20                                           │                                                                       
      size            │ 56 MB (-11 MB)                                                       │ 68 MB                                                                 
      packages        │ 86 (-15)                                                             │ 101                                                                   
                      │                                                                      │                                                                       
```