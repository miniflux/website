title: Development
description: Working with Miniflux's source code
template: doc
uri: docs/development.html
---
Working with Miniflux's code base is pretty simple:

- [Requirements](#requirements)
- [Checkout the Source Code](#source-code)
- [Compilation](#compilation)
- [Remove Precompiled Binaries](#cleanup)
- [Run the Software Locally](#run)
- [Regenerate Embedded Files](#generate)
- [Linter](#linter)
- [Unit Tests](#unit-tests)
- [Integration Tests](#integration-tests)
- [Build Docker Image](#docker-image)

<h2 id="requirements">Requirements <a class="anchor" href="#requirements" title="Permalink">¶</a></h2>

- Git
- Go >= 1.14

<h2 id="source-code">Checkout the Source Code <a class="anchor" href="#source-code" title="Permalink">¶</a></h2>

Fork the project and clone the repository locally.

Miniflux uses [Go Modules](https://github.com/golang/go/wiki/Modules) to manage dependencies.

<h2 id="compilation">Compilation <a class="anchor" href="#compilation" title="Permalink">¶</a></h2>

Build the application for the actual platform:

```bash
make miniflux
```

To define a specific version number:

```bash
make miniflux VERSION=2.0.13
```

Cross compilation:

```bash
# Build all binaries for all supported platforms
make build

# Build Linux binary for amd64 architecture
make linux-amd64

# ARM 64 bits (arm64v8)
make linux-armv8

# ARM 32 bits variant 7 (arm32v7)
make linux-armv7

# ARM 32 bits variant 6 (arm32v6)
make linux-armv6

# ARM 32 bits variant 5 (arm32v5)
make linux-armv5

# Mac OS (amd64)
make darwin-amd64

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

This command executes `go generate` and `go run main.go`.

<h2 id="generate">Regenerate Embedded Files <a class="anchor" href="#generate" title="Permalink">¶</a></h2>

To avoid any dependencies, all assets (Javascript, CSS, images, translations) are automatically included in the source code.

```bash
make generate
```

<h2 id="linter">Linter <a class="anchor" href="#linter" title="Permalink">¶</a></h2>

```bash
make lint
```

<h2 id="unit-tests">Unit Tests <a class="anchor" href="#unit-tests" title="Permalink">¶</a></h2>

```bash
make test
```

<h2 id="integration-tests">Integration Tests <a class="anchor" href="#integration-tests" title="Permalink">¶</a></h2>

Integration tests are testing API endpoints with a real database.

You need to have Postgresql installed locally preconfigured with the user "postgres" and the password "postgres".

To run integration tests, execute the following command:

```bash
make integration-test ; make clean-integration-test
```

If the test suite fail, you will see the logs of Miniflux.

<h2 id="docker-image">Build Docker Image <a class="anchor" href="#docker-image" title="Permalink">¶</a></h2>

Miniflux supports three different architectures for Docker containers: `amd64`, `arm32v6`, `arm32v7` and `arm64v8`.
There is one image for each architecture and a manifest.

Here an example to build only the `amd64` image:

```bash
make docker-image
```

To build all images and override the image name:

```bash
make docker-images DOCKER_IMAGE=your-namespace/miniflux
```

To override the build version:

```bash
make docker-images DOCKER_IMAGE=your-namespace/miniflux VERSION=42
```

To create the manifest and push the images:

```bash
make docker-manifest DOCKER_IMAGE=your-namespace/miniflux
```
