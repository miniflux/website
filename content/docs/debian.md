---
title: Debian Installation
description: Instructions to install Miniflux on Debian GNU/Linux
url: docs/debian.html
---

- [Install the Debian Package Manually](#debian-package)
- [Install the Debian Package from the APT Repo](#apt-repo)
- [Configure Miniflux](#configuration)

You must have Debian >= 8 or Ubuntu >= 16.04.

When using the Debian package, the Miniflux daemon is supervised by systemd.

Make sure to [install and configure Postgresql]({{< ref "database.md" >}}) before installing Miniflux.

<h2 id="debian-package">Install Miniflux with the Debian package <a class="anchor" href="#debian-package" title="Permalink">¶</a></h2>

1. Download the Debian package from the [GitHub Releases page](https://github.com/miniflux/v2/releases).
2. Install the Debian package: `dpkg -i miniflux_2.0.13_amd64.deb`

<h2 id="apt-repo">Install Miniflux from the APT Repository <a class="anchor" href="#apt-repo" title="Permalink">¶</a></h2>

You can configure APT to use Miniflux repository.
To start, create a `miniflux.list` file in the `/etc/apt/sources.list.d` directory.
You will need sudo access to make these changes:

Here is a basic template for `/etc/apt/sources.list.d/miniflux.list`:

```
deb [trusted=yes] https://repo.miniflux.app/apt/ * *
```

Or run this one-liner:

```bash
echo "deb [trusted=yes] https://repo.miniflux.app/apt/ * *" | sudo tee /etc/apt/sources.list.d/miniflux.list > /dev/null
```

Update the list of packages:

```bash
apt update
```

Then install the package:

```bash
apt install miniflux
```

To upgrade Miniflux, run `apt upgrade miniflux`, and don't forget to run the database migrations.

<div class="warning">
The previous repository URL <code>https://apt.miniflux.app/</code> is deprecated in favor of <code>https://repo.miniflux.app/apt/</code>.
</div>

<h2 id="configuration">Configure Miniflux <a class="anchor" href="#configuration" title="Permalink">¶</a></h2>

1. Define the environment variable `DATABASE_URL` in `/etc/miniflux.conf`
2. Run the SQL migrations manually: `miniflux -migrate`, or set the variable `RUN_MIGRATIONS=1` in `/etc/miniflux.conf`
3. Create an admin user: `miniflux -create-admin`
4. Restart the process: `systemctl restart miniflux`
5. Check the process status: `systemctl status miniflux`

Since Miniflux v2.0.25, the Debian package is available for multiple architectures: `amd64`, `arm64`, and `armhf`.
This way, it's very easy to install Miniflux on a Raspberry Pi.

<p class="info">
Systemd reads the environment variables from the file <code>/etc/miniflux.conf</code>.
You must restart the service to take the new values into consideration.
</p>

Make sure to take a look at the list of [configuration parameters]({{< relref configuration >}}) to customize your installation.
