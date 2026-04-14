# Docker Compose practical notes

Things I keep forgetting or looking up.

## Service dependencies

```yaml
depends_on:
  db:
    condition: service_healthy
```

The bare `depends_on: [db]` form only waits for the container to start, not for the service inside to be ready. Use `condition: service_healthy` with a `healthcheck` block in the dependency service.

```yaml
db:
  image: postgres:15
  healthcheck:
    test: ["CMD-SHELL", "pg_isready -U postgres"]
    interval: 5s
    timeout: 5s
    retries: 5
```

This pattern has saved the "app starts before database is ready" issue in every project that didn't have it.

## Override files

`docker compose -f compose.yml -f compose.override.yml up` merges the files. The common pattern is to keep secrets and host mounts in the override file, which can be gitignored.

## Useful commands

```bash
docker compose up -d          # detached
docker compose logs -f app    # follow logs for one service
docker compose exec app bash  # shell into running container
docker compose down -v        # remove containers AND volumes
```

The `-v` flag on `down` is destructive — removes named volumes. Don't use it if you want to keep database state between runs.

## Volume bind mounts in development

```yaml
volumes:
  - .:/app
  - /app/node_modules
```

The second entry prevents the host from overwriting the container's `node_modules` with whatever is (or isn't) on the host. A pattern I saw fail spectacularly in a demo before understanding why it was there.

Reference: [Docker Compose file reference](https://docs.docker.com/compose/compose-file/).
