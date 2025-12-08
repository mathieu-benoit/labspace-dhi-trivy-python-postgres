# Scan DHI images with Docker Scout

Scan the Python app image:

```bash
docker scout quickview web:dhi
```

Review the Docker Scout quickview.

```none no-copy-button
0C     0H     2M     0L
```

Scan the PosgreSQL image:

```bash
docker scout quickview $$org$$/dhi-postgres:17.7
```

Review the Docker Scout quickview.

```none no-copy-button
0C     0H     0M     1L
```

Compare the before and after for the Python app:

```bash
docker scout compare --ignore-unchanged --to web:init web:dhi
```

```none no-copy-button
                      │                            Analyzed Image                            │                           Comparison Image                            
  ────────────────────┼──────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────
    Target            │  web:dhi                                                             │  web:init                                                             
      digest          │  7704ea2aec6f                                                        │  7269e04be765                                                         
      tag             │  latest                                                              │  latest                                                               
      platform        │ linux/amd64                                                          │ linux/amd64                                                           
      provenance      │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres │ https://github.com/mathieu-benoit/labspace-dhi-trivy-python-postgres  
                      │  681452bc84b3d7314ec94fac1ea2eb531520f210                            │  681452bc84b3d7314ec94fac1ea2eb531520f210                             
      vulnerabilities │    0C     0H     2M     0L                                           │    0C     0H     1M    20L                                            
                      │                  +1    -20                                           │                                                                       
      size            │ 55 MB (-8.4 MB)                                                      │ 64 MB                                                                 
      packages        │ 86 (-15)                                                             │ 101                                                                   
                      │                                                                      │                                                                       
    Base image        │  demonstrationorg/dhi-python-bcg:3.13-debian13-dev                   │  python:3.13-slim                                                     
      tags            │ also known as                                                        │ also known as                                                         
                      │                                                                      │   • 3.13-slim-trixie                                                  
                      │                                                                      │   • 3.13.10-slim                                                      
                      │                                                                      │   • 3.13.10-slim-trixie                                               
      vulnerabilities │    0C     0H     1M     0L                                           │    0C     0H     1M    20L 
```