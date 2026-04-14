---
title: Docker compose patterns for local dev
slug: docker-compose-patterns
tags: [docker, devops, reference]
---

# Docker compose patterns for local dev

Notes on patterns I reuse across projects for local development environments.

## Basic multi-service setup

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
      - /app/node_modules

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - db_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d mydb"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  db_data:
```

## Using .env files

Compose automatically reads `.env` in the project root. Reference variables with `${VARIABLE_NAME}`. Keep `.env.example` committed with placeholder values.

## Override files for environment-specific config

```bash
# Base: docker-compose.yml
# Local overrides: docker-compose.override.yml (auto-loaded)
# CI: docker-compose.ci.yml
docker compose -f docker-compose.yml -f docker-compose.ci.yml up
```

## Useful commands

```bash
docker compose up -d          # start detached
docker compose logs -f app    # follow logs for service
docker compose exec app sh    # shell into running container
docker compose down -v        # stop and remove volumes
docker compose build --no-cache  # force rebuild
```

Official reference: [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)
