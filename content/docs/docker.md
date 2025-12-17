---
title: Miniflux Installation with Docker
description: Instructions to install Miniflux with Docker
url: docs/docker.html
---

This document describes how to use the Docker images of Miniflux.

- [Container Registries](#registries)
- [How to Run the Container Manually](#docker)
- [Docker Compose](#docker-compose)

<h2 id="registries">Container Registries <a class="anchor" href="#registries" title="Permalink">¶</a></h2>

**Docker Registries:**

Docker images are published to three different container registries:

- [Docker Hub Registry](https://hub.docker.com/r/miniflux/miniflux): `docker.io/miniflux/miniflux`
- [GitHub Container Registry](https://github.com/miniflux/v2/pkgs/container/miniflux): `ghcr.io/miniflux/miniflux`
- [Quay.io RedHat Container Registry](https://quay.io/repository/miniflux/miniflux): `quay.io/miniflux/miniflux`

**Docker Architectures:**

- `amd64`
- `arm64`
- `arm/v7`
- `arm/v6`

**Docker Tags Naming Convention:**

- `latest`: Latest stable version
- `2.0.25`: Specific version
- `nightly`: Development version

It is recommended to use a pinned version to avoid unexpected updates.

<h2 id="docker">How to Run the Container Manually <a class="anchor" href="#docker" title="Permalink">¶</a></h2>

Pull the image and run the container:

```bash
docker run -d \
  -p 80:8080 \
  --name miniflux \
  -e "DATABASE_URL=postgres://miniflux:*password*@*dbhost*/miniflux?sslmode=disable" \
  -e "RUN_MIGRATIONS=1" \
  -e "CREATE_ADMIN=1" \
  -e "ADMIN_USERNAME=*username*" \
  -e "ADMIN_PASSWORD=*password*" \
  docker.io/miniflux/miniflux:latest
```

The command above will run the migrations and set up a new admin account with the chosen username and password.

Once the container is started, you should be able to access the application on the exposed port, which is port 80 in this example.

<h2 id="docker-compose">Docker Compose <a class="anchor" href="#docker-compose" title="Permalink">¶</a></h2>

Here is an example of a Docker Compose file:

```yaml
services:
  miniflux:
    image: miniflux/miniflux:latest
    ports:
      - "80:8080"
    depends_on:
      db:
        condition: service_healthy
    environment:
      - DATABASE_URL=postgres://miniflux:secret@db/miniflux?sslmode=disable
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME=admin
      - ADMIN_PASSWORD=test123
  db:
    image: postgres:18
    environment:
      - POSTGRES_USER=miniflux
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=miniflux
    volumes:
      # You may have to adjust the volume path depending on the version of Postgres
      # Postgres 18 uses /var/lib/postgresql
      # Postgres 17 and below uses /var/lib/postgresql/data
      # See https://hub.docker.com/_/postgres#pgdata
      - miniflux-db:/var/lib/postgresql
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "miniflux"]
      interval: 10s
      start_period: 30s
volumes:
  miniflux-db:
```

- `DATABASE_URL` defines the database connection parameters.
- `RUN_MIGRATIONS=1` runs the SQL migrations automatically.
- `CREATE_ADMIN`, `ADMIN_USERNAME`, and `ADMIN_PASSWORD` allow the creation of the first admin user. These can be removed after the first initialization.

There are more examples in the Git repository with Traefik and Caddy: https://github.com/miniflux/v2/tree/main/contrib/docker-compose

You can also configure an optional health check in your Docker Compose file:

```yaml
miniflux:
  image: miniflux/miniflux:latest
  healthcheck:
    test: ["CMD", "/usr/bin/miniflux", "-healthcheck", "auto"]
```

Make sure to review the list of [configuration parameters]({{< relref configuration >}}) to customize your installation.
