# Docker Compose quick reference

Things I look up more than I'd like. Based on Compose v2 (the `docker compose` command, not the deprecated `docker-compose`). See [official docs](https://docs.docker.com/compose/compose-file/).

## Common commands

```bash
docker compose up -d           # Start in background
docker compose down            # Stop and remove containers
docker compose down -v         # Also remove volumes
docker compose logs -f web     # Follow logs for 'web' service
docker compose exec web bash   # Shell into running container
docker compose run web pytest  # Run one-off command
docker compose ps              # List running services
docker compose build           # Rebuild images
docker compose pull            # Pull latest images
```

## Minimal compose.yaml

```yaml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db/mydb
    depends_on:
      db:
        condition: service_healthy
    volumes:
      - .:/app

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "user"]
      interval: 5s
      retries: 5

volumes:
  pgdata:
```

## Environment variables

Prefer `.env` file in the same directory — Compose loads it automatically. Reference with `${VAR_NAME}` in the compose file. Never commit the `.env` file.

## Networking

Services within the same Compose project can reach each other by service name. External-facing ports use the `ports` key; internal-only traffic uses `expose`.

## Profiles

Use profiles to define services that don't start by default:
```yaml
services:
  adminer:
    image: adminer
    profiles: [tools]
```
Start with: `docker compose --profile tools up`
