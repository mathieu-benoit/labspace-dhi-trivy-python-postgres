# Run the Python app with Postgres

Open the :fileLink[Dockerfile]{path="Dockerfile"} and review it.

Open the :fileLink[docker-compose.yml]{path="docker-compose.yml"} and review it.

Start the application:

```bash
docker compose up -d --build
```

Go to the application in the browser: :tabLink[http://localhost:5000]{href="http://localhost:5000" title="Web app"}.

Enter a reservation using the application.