# Docker Compose patterns for local development

## Basic service with volume and health check

```yaml
services:
  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d myapp"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  pgdata:
```

## Depends on with condition

```yaml
  app:
    build: .
    depends_on:
      db:
        condition: service_healthy
```

This waits for the health check to pass before starting the app. Critical for apps that try to connect at startup.

## Override files

```
docker-compose.yml          # base config
docker-compose.override.yml # local overrides (gitignored)
docker-compose.prod.yml     # production overrides
```

Run with: `docker compose -f docker-compose.yml -f docker-compose.prod.yml up`

## Useful commands

```bash
docker compose up -d              # start detached
docker compose logs -f app        # follow logs for one service
docker compose exec db psql -U user myapp  # shell into running container
docker compose down -v            # stop and remove volumes
docker compose build --no-cache   # force rebuild
```

## Networking

By default, all services in a compose file can reach each other by service name. Your app connects to `db:5432`, not `localhost:5432`.

Reference: https://docs.docker.com/compose/
