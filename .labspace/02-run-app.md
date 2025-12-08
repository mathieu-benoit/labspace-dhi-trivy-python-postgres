# Run the Python app with PostgreSQL

## Review the Python app

Open the :fileLink[Dockerfile]{path="Dockerfile"} and review it.

Open the :fileLink[docker-compose.yml]{path="docker-compose.yml"} and review it.

## Run and test the Python app

Start the application:

```bash
docker compose up -d --build
```

Go to the application in the browser: :tabLink[http://localhost:5000]{href="http://localhost:5000" title="Web app"}.

Enter a reservation using the application.

## Build the container image of the Python app

Build the Python app:

```bash
docker build -t web:init --sbom=true --provenance=mode=max .
```

Verify the image built:

```bash
docker images web
```

```none no-copy-button
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA    
web:init       391b8587185a        256MB         65.8MB
```