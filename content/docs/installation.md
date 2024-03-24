---
title: Installation Instructions
description: Instructions to install Miniflux on your own server
url: docs/installation.html
---

Installing Miniflux is straightforward if you have some basic system administration knowledge.

- [Packages](#packages)
- [Database Configuration]({{< ref "database.md" >}})
- [Manual Installation]({{< ref "binary.md" >}})
- [Debian/Ubuntu/Raspbian Package Installation]({{< ref "debian.md" >}})
- [RPM Package Installation]({{< ref "rhel.md" >}})
- [Alpine Linux Installation]({{< ref "alpine.md" >}})
- [Docker Installation]({{< ref "docker.md" >}})

<h2 id="packages">Packages <a class="anchor" href="#packages" title="Permalink">¶</a></h2>

Platform       |  Type               |  Repository URL
---------------|---------------------|---------------------------------------------------------------------
Debian/Ubuntu  |  Upstream (Binary)  |  https://github.com/miniflux/v2/tree/master/packaging/debian
RHEL/Fedora    |  Upstream (Binary)  |  https://github.com/miniflux/v2/tree/master/packaging/rpm
Alpine Linux   |  Community (Source) |  https://git.alpinelinux.org/aports/tree/community/miniflux
Arch Linux     |  Community (Source) |  https://archlinux.org/packages/extra/x86_64/miniflux/
Debian         |  Community (Binary) |  https://packages.debian.org/trixie/miniflux
FreeBSD Port   |  Community (Source) |  https://svnweb.freebsd.org/ports/head/www/miniflux/
Nix            |  Community (Source) |  https://github.com/NixOS/nixpkgs/tree/master/pkgs/servers/miniflux
Ubuntu         |  Community (Binary) |  https://packages.ubuntu.com/noble/miniflux

You can download precompiled binaries and packages from the [GitHub Releases page](https://github.com/miniflux/v2/releases). You could also [build the application from the source code]({{< ref "development.md" >}}).
