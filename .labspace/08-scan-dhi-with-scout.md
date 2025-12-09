# Scan the DHI images with Docker Scout

## Scan the Python app image

Scan the Python app image:

```bash
docker scout quickview web:dhi
```

Review the Docker Scout quickview.

```none no-copy-button
0C     0H     1M     0L
```

Get the details of the CVEs:

```bash
docker scout cves web:dhi
```

Review the vulnerabilities.

## Scan the PosgreSQL image

Scan the PosgreSQL image:

```bash
docker scout quickview $$org$$/dhi-postgres:17.7
```

Review the Docker Scout quickview.

```none no-copy-button
0C     0H     0M     1L
```

Get the details of the CVEs:

```bash
docker scout cves postgres:17.7
```

Review the vulnerabilities.

## Compare between the before and after for the Python app

Compare between the before and after for the Python app:

```bash
docker scout compare --ignore-unchanged --to web:init web:dhi
```

```none no-copy-button
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:dhi                                                             │  web:init                                                             
      digest          │  5ad4dc2d90c0                                                        │  ea7557110ca6                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  a19eea2f7b330432612ebefc0d1c44a33a62f5a5                            │  94a72a184ec641823bbe0b517ed77f0253204901                             
      vulnerabilities │    0C     0H     1M     0L                                           │    0C     0H     1M    20L                                            
                      │                        -20                                           │                                                                       
      size            │ 55 MB (-12 MB)                                                       │ 68 MB                                                                 
      packages        │ 86 (-15)                                                             │ 101
```