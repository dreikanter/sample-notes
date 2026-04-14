# Docker Compose reference — common patterns

Notes from setting up local development environments. Assumes Compose V2 syntax (no `version:` key required).

## Basic structure

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgres://user:pass@db:5432/mydb
    depends_on:
      db:
        condition: service_healthy
    volumes:
      - .:/app
      - /app/node_modules  # anonymous volume prevents host overwrite

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "user"]
      interval: 5s
      retries: 5

volumes:
  postgres_data:
```

## Useful commands

```bash
docker compose up -d          # start in background
docker compose logs -f app    # tail app logs
docker compose exec app sh    # shell into running container
docker compose run --rm app npm test  # one-off command
docker compose down -v        # stop and remove volumes
```

## Override files for environments

Base: `docker-compose.yml`
Local dev overrides: `docker-compose.override.yml` (loaded automatically)
CI overrides: `docker-compose.ci.yml` (explicit: `compose -f docker-compose.yml -f docker-compose.ci.yml`)

## Networking

Services communicate by service name within the default network. `db:5432` works from `app` because Compose creates a DNS entry for each service.

To expose a service only internally (not to host), omit the `ports` key.

Full reference: [Docker Compose documentation](https://docs.docker.com/compose/)
