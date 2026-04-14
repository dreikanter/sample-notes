# Docker compose patterns

Recurring patterns I keep reusing or looking up.

## Environment variables from file

```yaml
services:
  app:
    env_file:
      - .env
      - .env.local   # overrides .env, gitignored
```

## Health checks and dependency ordering

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

Without `condition: service_healthy`, `depends_on` only waits for the container to start, not for the service inside to be ready. This catches people out constantly.

## Volume mounts for development

```yaml
services:
  app:
    volumes:
      - .:/app              # mount source
      - /app/node_modules   # anonymous volume to avoid overwriting
```

The anonymous volume for `node_modules` prevents the host directory (which may have different platform binaries) from overwriting the container's modules.

## Multi-stage with override

Use `docker-compose.override.yml` for dev-specific settings — it's merged automatically:

```yaml
# docker-compose.override.yml
services:
  app:
    command: npm run dev   # overrides prod command
    ports:
      - "9229:9229"        # debug port
```

## Useful commands

```bash
docker compose up -d --build     # rebuild and start detached
docker compose logs -f app       # follow specific service
docker compose exec app sh        # shell into running container
docker compose down -v            # remove volumes too
```

Reference: https://docs.docker.com/compose/compose-file/
