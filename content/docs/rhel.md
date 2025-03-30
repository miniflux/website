---
title: RHEL Installation
description: Instructions to install Miniflux on RHEL distros
url: docs/rhel.html
---

This document describes how to install Miniflux on any RHEL-compatible Linux distributions such as RedHat, CentOS, RockyLinux, or AlmaLinux.

When you use the RPM package, the Miniflux daemon is managed by systemd.

Make sure to [install and configure PostgreSQL]({{< relref "database" >}}) before installing Miniflux.

- [Install the RPM Package Manually](#rpm-package)
- [Install the RPM Package from the YUM Repo](#rpm-repo)
- [Configure Miniflux](#configuration)

<h2 id="rpm-package">RPM Package Installation <a class="anchor" href="#rpm-package" title="Permalink">¶</a></h2>

1. Download the RPM package from the [GitHub Releases page](https://github.com/miniflux/v2/releases).
2. Install the RPM package: `rpm -ivh miniflux-2.0.13-1.0.x86_64.rpm`.

<h2 id="rpm-repo">How to Configure the RPM Repository <a class="anchor" href="#rpm-repo" title="Permalink">¶</a></h2>

Create the file `/etc/yum.repos.d/miniflux.repo`:

```
[miniflux]
name=Miniflux Repository
baseurl=https://repo.miniflux.app/yum/
enabled=1
gpgcheck=0
```

Then install the package:

```bash
dnf install -y miniflux
```

<div class="warning">
The previous repository URL <code>https://rpm.miniflux.app/x86_64/</code> is deprecated in favor of <code>https://repo.miniflux.app/yum/</code>.
</div>

<h2 id="configuration">Configure Miniflux <a class="anchor" href="#configuration" title="Permalink">¶</a></h2>

1. Define the environment variable `DATABASE_URL` if necessary.
2. Run the SQL migrations: `miniflux -migrate`, or set the variable `RUN_MIGRATIONS=1` in `/etc/miniflux.conf`.
3. Create an admin user: `miniflux -create-admin`.
4. Customize your configuration file `/etc/miniflux.conf` if necessary.
5. Enable the systemd service: `systemctl enable miniflux`.
6. Start the process with systemd: `systemctl start miniflux`.
7. Check the process status: `systemctl status miniflux`.

<p class="info">
Systemd reads the environment variables from the file <code>/etc/miniflux.conf</code>.
You must restart the service to apply any new values.
</p>

Make sure to review the list of [configuration parameters]({{< relref configuration >}}) to customize your installation.
