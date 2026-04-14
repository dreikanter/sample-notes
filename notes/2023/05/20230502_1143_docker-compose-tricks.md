# Docker Compose — patterns I keep relearning

Documenting these because I look them up every single time.

**Override files**

```bash
docker compose -f docker-compose.yml -f docker-compose.override.yml up
```

By default, Compose looks for `docker-compose.override.yml` automatically. Useful for keeping dev-specific settings (volume mounts, debug ports) out of the base file.

**Environment variable precedence**

1. Values set in the shell environment
2. `.env` file in the project root
3. `environment:` block in `compose.yml`
4. Defaults in the image

**Waiting for dependencies**

`depends_on` only waits for the container to start, not for the service to be ready. For actual readiness, use a healthcheck:

```yaml
services:
  db:
    image: postgres:15
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

**Rebuilding without cache**

```bash
docker compose build --no-cache
```

**One-off command against a service**

```bash
docker compose run --rm app python manage.py migrate
```

**Profiles** (Compose 1.28+)

```yaml
services:
  adminer:
    image: adminer
    profiles: [tools]
```

Only starts with `docker compose --profile tools up`.

Full docs: [docs.docker.com/compose](https://docs.docker.com/compose/)
