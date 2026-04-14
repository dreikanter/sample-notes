---
title: Docker Compose patterns I keep reusing
slug: docker-compose-patterns
tags: [docker, devops, reference]
description: Reusable docker-compose patterns for local dev
public: true
---

# Docker Compose patterns I keep reusing

**Health checks with depends_on**

Without health checks, `depends_on` only waits for the container to start, not for the service inside to be ready. This causes race conditions with databases.

```yaml
services:
  db:
    image: postgres:16
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
      interval: 5s
      timeout: 5s
      retries: 5

  app:
    depends_on:
      db:
        condition: service_healthy
```

**Shared environment files**

```yaml
services:
  app:
    env_file:
      - .env
      - .env.local  # overrides .env, gitignored
```

Later files override earlier ones. Keep secrets in `.env.local` which is gitignored; keep defaults in `.env` which is committed.

**Named volumes with explicit driver options**

```yaml
volumes:
  postgres_data:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: ./data/postgres
```

This binds a host directory as a named volume, giving you both the persistence of a bind mount and the compose-friendly volume syntax.

**Override files for dev vs CI**

Keep `docker-compose.yml` as the base, `docker-compose.override.yml` for local dev (hot reloading, debug ports), and `docker-compose.ci.yml` for CI. Compose auto-loads the override file; for CI, specify explicitly:

```bash
docker compose -f docker-compose.yml -f docker-compose.ci.yml up
```

Reference: https://docs.docker.com/compose/compose-file/
