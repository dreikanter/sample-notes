---
title: Docker Compose patterns for local dev
slug: docker-compose-patterns-local-dev
tags: [docker, devops, development]
description: Useful Docker Compose patterns for local development environments
---

# Docker Compose patterns for local dev

Notes on patterns I use regularly. Not a tutorial — assuming familiarity with the basics.

## Override files

Keep a `docker-compose.yml` for production-like config and a `docker-compose.override.yml` for local dev additions. The override is automatically merged:

```yaml
# docker-compose.override.yml
services:
  app:
    volumes:
      - .:/app          # live code mounting
    environment:
      - DEBUG=true
    command: npm run dev  # override production command
```

## Wait-for dependencies

Use `depends_on` with condition to properly wait for database readiness:

```yaml
services:
  app:
    depends_on:
      db:
        condition: service_healthy
  db:
    image: postgres:16
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
```

This is more reliable than sleep-based workarounds in entrypoint scripts.

## Named volumes for database persistence

```yaml
volumes:
  postgres_data:

services:
  db:
    volumes:
      - postgres_data:/var/lib/postgresql/data
```

Named volumes survive `docker-compose down` but not `docker-compose down -v`. This distinction matters.

## Profiles for optional services

```yaml
services:
  mailhog:
    image: mailhog/mailhog
    profiles: ["mail"]
```

Start only with: `docker-compose --profile mail up`

[Compose file reference](https://docs.docker.com/compose/compose-file/)
