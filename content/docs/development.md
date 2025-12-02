---
title: Development
description: Working with Miniflux's source code
url: docs/development.html
---
Working with Miniflux's codebase is straightforward:

- [Requirements](#requirements)
- [Check Out the Source Code](#source-code)
- [Compilation](#compilation)
- [Remove Precompiled Binaries](#cleanup)
- [Run the Software Locally](#run)
- [Linter](#linter)
- [Unit Tests](#unit-tests)
- [Integration Tests](#integration-tests)
- [Build Docker Image](#docker-image)
- [Create RPM Package](#rpm)
- [Create Debian Package](#debian)
- [GitHub Codespaces](#github-codespaces)
- [Run a Local PostgreSQL Database with Docker](#postgresql-docker)

<h2 id="requirements">Requirements <a class="anchor" href="#requirements" title="Permalink">¶</a></h2>

- Git
- Go >= 1.24

<h2 id="source-code">Check Out the Source Code <a class="anchor" href="#source-code" title="Permalink">¶</a></h2>

Fork the project and clone the repository locally.

Miniflux uses [Go Modules](https://github.com/golang/go/wiki/Modules) to manage dependencies.

<h2 id="compilation">Compilation <a class="anchor" href="#compilation" title="Permalink">¶</a></h2>

Build the application for the current platform:

```bash
make miniflux
```

To specify a version number:

```bash
make miniflux VERSION=2.0.29
```

Cross-compilation:

```bash
# Build binaries for all supported platforms
make build

# Build Linux binary for amd64 architecture
make linux-amd64

# ARM 64-bit (arm64v8)
make linux-arm64

# ARM 32-bit variant 7 (arm32v7)
make linux-armv7

# ARM 32-bit variant 6 (arm32v6)
make linux-armv6

# ARM 32-bit variant 5 (arm32v5)
make linux-armv5

# macOS (amd64)
make darwin-amd64

# macOS (arm64 / Apple Silicon)
make darwin-arm64

# FreeBSD (amd64)
make freebsd-amd64

# OpenBSD (amd64)
make openbsd-amd64

# Windows (amd64)
make windows-amd64
```

<h2 id="cleanup">Remove Precompiled Binaries <a class="anchor" href="#cleanup" title="Permalink">¶</a></h2>

```bash
make clean
```

<h2 id="run">Run the Software Locally <a class="anchor" href="#run" title="Permalink">¶</a></h2>

```bash
make run
```

This command runs the software in debug mode. If needed, you can [run a local PostgreSQL database with Docker](#postgresql-docker).

<h2 id="linter">Linter <a class="anchor" href="#linter" title="Permalink">¶</a></h2>

```bash
make lint
```

<h2 id="unit-tests">Unit Tests <a class="anchor" href="#unit-tests" title="Permalink">¶</a></h2>

```bash
make test
```

<h2 id="integration-tests">Integration Tests <a class="anchor" href="#integration-tests" title="Permalink">¶</a></h2>

Integration tests validate API endpoints with a real database.

You need to have PostgreSQL installed locally, preconfigured with the user "postgres" and the password "postgres".
Alternatively, you can [run a local PostgreSQL database with Docker](#postgresql-docker).

To run integration tests, execute the following command:

```bash
make integration-test ; make clean-integration-test
```

If the test suite fails, you will see the logs of Miniflux.

<h2 id="docker-image">Build Docker Image <a class="anchor" href="#docker-image" title="Permalink">¶</a></h2>

Miniflux supports different architectures for Docker images: `amd64`, `arm32v6`, `arm32v7`, and `arm64v8`.

Here is an example to build only the `amd64` image:

```bash
make docker-image
```

Build all images and override the image name:

```bash
make docker-images DOCKER_IMAGE=your-namespace/miniflux
```

Override the build version:

```bash
make docker-images DOCKER_IMAGE=your-namespace/miniflux VERSION=42
```

Note that you need to enable Docker experimental features to build multi-platform images.
Miniflux uses [buildx](https://docs.docker.com/buildx/working-with-buildx/).

<h2 id="rpm">Build RPM Package <a class="anchor" href="#rpm" title="Permalink">¶</a></h2>

You can build your own RPM package using this command:

```bash
make rpm
```

Note that Docker is required to generate the RPM package.
All build operations are performed inside a container.

<h2 id="debian">Build Debian Package <a class="anchor" href="#debian" title="Permalink">¶</a></h2>

You can build your own Debian package using this command:

```bash
make debian
```

Use the following command to build packages for all supported architectures (`amd64`, `arm64`, and `armhf`):

```bash
make debian-packages
```

Note that Docker is required to generate the Debian packages.
All build operations are performed inside a container.

<h2 id="github-codespaces">GitHub Codespaces <a class="anchor" href="#github-codespaces" title="Permalink">¶</a></h2>

The Miniflux development environment is preconfigured for GitHub Codespaces.
It is useful for small contributions.

Click the "Create codespace" button in the GitHub web UI to create a new development environment in the cloud. Once it's ready, you can use Visual Studio Code to edit the source code.

<h2 id="postgresql-docker">Run a Local PostgreSQL Database with Docker <a class="anchor" href="#postgresql-docker" title="Permalink">¶</a></h2>

Based on the [PostgreSQL Docker image](https://hub.docker.com/_/postgres).

You can create a local PostgreSQL database for testing easily with the following command:

```bash
docker run --rm --name local-miniflux2-db -p 5432:5432 -e POSTGRES_DB=miniflux2 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres postgres
```

To persist data:

```bash
docker volume create local-miniflux2-data
docker run --rm --name local-miniflux2-db -p 5432:5432 -v local-miniflux2-data:/var/lib/postgresql -e POSTGRES_DB=miniflux2 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres postgres
```

To delete data:

```bash
docker volume rm local-miniflux2-data
```
