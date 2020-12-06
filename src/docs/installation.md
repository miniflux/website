title: Installation Instructions
description: Instructions to install Miniflux on your own server
template: doc
uri: docs/installation.html
---
Installing Miniflux is straightforward if you have some basic system administration knowledge.

- [Packages](#packages)
- [Database Configuration](#database)
- [Manual Installation](#binary)
- [Debian Package Installation](#debian)
- [RPM Package Installation](#rpm)
- [Alpine Linux Installation](#alpine-linux)
- [Docker Installation](#docker)

<h2 id="packages">Packages <a class="anchor" href="#packages" title="Permalink">¶</a></h2>

Platform       |  Type               |  Repository URL
---------------|---------------------|---------------------------------------------------------------------
Debian/Ubuntu  |  Upstream (Binary)  |  https://github.com/miniflux/v2/tree/master/packaging/debian
RHEL/Fedora    |  Upstream (Binary)  |  https://github.com/miniflux/v2/tree/master/packaging/rpm
Alpine Linux   |  Community (Source) |  https://git.alpinelinux.org/aports/tree/community/miniflux
Arch Linux     |  Community (Source) |  https://aur.archlinux.org/packages/miniflux/
FreeBSD Port   |  Community (Source) |  https://svnweb.freebsd.org/ports/head/www/miniflux/
Nix            |  Community (Source) |  https://github.com/NixOS/nixpkgs/tree/master/pkgs/servers/miniflux

You can download precompiled binaries and packages on the [GitHub Releases page](https://github.com/miniflux/v2/releases). You could also [build the application from the source code](development.html).

<h2 id="database">Database Configuration <a class="anchor" href="#database" title="Permalink">¶</a></h2>

### Creating the Database

Here an example from the command line:

```
# Switch to the postgres user
$ su - postgres

# Create a database user for Miniflux
$ createuser -P miniflux
Enter password for new role: ******
Enter it again: ******

# Create a database for miniflux that belongs to our user
$ createdb -O miniflux miniflux

# Create the extension hstore as superuser
$ psql miniflux -c 'create extension hstore'
CREATE EXTENSION
```

### Enabling HSTORE extension for Postgresql

Creating Postgresql extensions requires the ``SUPERUSER`` privilege.
Several solutions are available:

1) Give `SUPERUSER` privileges to the miniflux user only during the schema migration:

```sql
ALTER USER miniflux WITH SUPERUSER;
-- Run the migrations (miniflux -migrate)
ALTER USER miniflux WITH NOSUPERUSER;
```

2) You could [create the hstore extension](https://www.postgresql.org/docs/current/static/sql-createextension.html>) with another user that have the ``SUPERUSER`` privileges before running the migrations.

```
sudo -u postgres psql $MINIFLUX_DATABASE
> CREATE EXTENSION hstore;
```

<h2 id="binary">Manual Installation <a class="anchor" href="#binary" title="Permalink">¶</a></h2>

1. Copy the precompiled binary somewhere on your server, for example in `/usr/local/bin`
2. Make the file executable: `chmod +x miniflux`
3. Define the environment variable `DATABASE_URL` if necessary
4. Run the SQL migrations: `miniflux -migrate`
5. Create an admin user: `miniflux -create-admin`
6. Start the application: `miniflux`

<p class="info">
You should configure a process manager like systemd or supervisord to supervise the Miniflux daemon.
</p>

<h2 id="debian">Debian Package Installation <a class="anchor" href="#debian" title="Permalink">¶</a></h2>

You must have Debian >= 8 or Ubuntu >= 16.04.
When using the Debian package, the Miniflux daemon is supervised by systemd.

1. Install the Debian package: `dpkg -i miniflux_2.0.13_amd64.deb`
2. Define the environment variable `DATABASE_URL` if necessary
3. Run the SQL migrations: `miniflux -migrate`
4. Create an admin user: `miniflux -create-admin`
5. Customize your configuration file `/etc/miniflux.conf` if necessary
6. Restart the process: `systemctl restart miniflux`
7. Check the process status: `systemctl status miniflux`

Note that you could also use the [Miniflux APT repository](howto.html#apt-repo) instead of downloading manually the Debian package.

Since Miniflux v2.0.25, the Debian package is available for multiple architectures: `amd64`, `arm64`, and `armhf`.

If you don't want to run the SQL migrations manually each time you upgrade Miniflux, set the environment variable: `RUN_MIGRATIONS=1` in `/etc/miniflux.conf`.

<p class="info">
Systemd reads the environment variables from the file <code>/etc/miniflux.conf</code>.
You must restart the service to take the new values into consideration.
</p>

<h2 id="rpm">RPM Package Installation <a class="anchor" href="#rpm" title="Permalink">¶</a></h2>

You must have Fedora or Centos/Redhat >= 7.
When you use the RPM package, the Miniflux daemon is supervised by systemd.

1. Install the Miniflux RPM package: `rpm -ivh miniflux-2.0.13-1.0.x86_64.rpm`
2. Define the environment variable `DATABASE_URL` if necessary
3. Run the SQL migrations: `miniflux -migrate`
4. Create an admin user: `miniflux -create-admin`
5. Customize your configuration file `/etc/miniflux.conf` if necessary
6. Enable the systemd service: `systemctl enable miniflux`
7. Start the process with systemd: `systemctl start miniflux`
8. Check the process status: `systemctl status miniflux`

Note that you could also use the [Miniflux RPM repository](howto.html#rpm-repo) instead of downloading manually the RPM package.

<p class="info">
Systemd reads the environment variables from the file <code>/etc/miniflux.conf</code>.
You must restart the service to take the new values into consideration.
</p>

<h2 id="alpine-linux">Alpine Linux Installation <a class="anchor" href="#alpine-linux" title="Permalink">¶</a></h2>

[Alpine Linux](https://alpinelinux.org/) is a lightweight Linux distribution that is perfectly suited for running Miniflux.

An APK package is available from the Edge community repository (it was in testing before).

Edit the file `/etc/apk/repositories` to enable the Edge repository: `http://dl-cdn.alpinelinux.org/alpine/edge/community`. And then run `apk update`.

The Miniflux installation is simple as running:

```bash
apk add miniflux miniflux-openrc miniflux-doc
```

Do not forget to install Postgresql:

```
apk add postgresql postgresql-contrib
```

Configure the database and enable the `HSTORE` extension as [mentioned previously](#database).

On Alpine Linux, the Miniflux process is supervised by `supervise-daemon` from [OpenRC](https://github.com/OpenRC/openrc) (there is no Systemd).
The log file `/var/log/miniflux.log` is rotated with `logrotate`.

In this context, the configuration file `/etc/miniflux.conf` is used instead of environment variables:

```
# /etc/miniflux.conf

LOG_DATE_TIME=yes
LISTEN_ADDR=127.0.0.1:8080
DATABASE_URL=user=postgres password=secret dbname=miniflux sslmode=disable

# Run SQL migrations automatically:
# RUN_MIGRATIONS=1
```

To finalize the installation, create the database schema and a first user:

```bash
miniflux -c /etc/miniflux.conf -migrate
miniflux -c /etc/miniflux.conf -create-admin
```

And finally, start the application:

```bash
service miniflux start
```

<h2 id="docker">Docker Installation <a class="anchor" href="#docker" title="Permalink">¶</a></h2>

**Docker Registries:**

- Docker Hub: `docker.io/miniflux/miniflux`
- GitHub Container Registry: `ghcr.io/miniflux/miniflux`

**Docker Architectures:**

- `amd64`
- `arm64`
- `arm/v7`
- `arm/v6`

**Docker Tags:**

- `latest`: Latest stable version
- `2.0.25`: Specific version
- `nightly`: Development version

Pull the image and run the container:

```bash
docker run -d -p 80:8080 miniflux/miniflux:latest
```

You will probably need to pass some environment variables like the `DATABASE_URL`.

You could also use Docker Compose. Here an example of `docker-compose.yml` file:

```yaml
version: '3'
services:
  miniflux:
    image: miniflux/miniflux:latest
    ports:
      - "80:8080"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgres://miniflux:secret@db/miniflux?sslmode=disable
  db:
    image: postgres:latest
    environment:
      - POSTGRES_USER=miniflux
      - POSTGRES_PASSWORD=secret
    volumes:
      - miniflux-db:/var/lib/postgresql/data
volumes:
  miniflux-db:
```

Start the database first `docker-compose up -d db` and then the application `docker-compose up miniflux`.

Remember that you still need to run the database migrations and create the first user:

```bash
# Run database migrations
docker-compose exec miniflux /usr/bin/miniflux -migrate

# Create the first user
docker-compose exec miniflux /usr/bin/miniflux -create-admin
```

Another way of doing the same thing is to populate the variables `RUN_MIGRATIONS`, `CREATE_ADMIN`, `ADMIN_USERNAME` and `ADMIN_PASSWORD`.

For example:

```yaml
version: '3'
services:
  miniflux:
    image: miniflux/miniflux:latest
    ports:
      - "80:8080"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgres://miniflux:secret@db/miniflux?sslmode=disable
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME=admin
      - ADMIN_PASSWORD=test123
  db:
    image: postgres:latest
    environment:
      - POSTGRES_USER=miniflux
      - POSTGRES_PASSWORD=secret
    volumes:
      - miniflux-db:/var/lib/postgresql/data
volumes:
  miniflux-db:  
```

There are more examples in the Git repo: https://github.com/miniflux/v2/tree/master/contrib/docker-compose
