title: Upgrading Miniflux
template: doc
uri: docs/upgrade.html
---
<div class="warning">
Please, do not update the software blindly without reading the <a href="https://github.com/miniflux/miniflux/blob/master/ChangeLog">Change Log</a>.
Always check for breaking changes if any.
</div>

<h2 id="instructions">Instructions <a class="anchor" href="#instructions" title="Permalink">¶</a></h2>

1. Export the environment variable `DATABASE_URL` if not already done
2. Disconnect all users by flushing all sessions: `miniflux -flush-sessions`
3. Stop the process
4. Backup your database
5. Check that your backup is really working
6. Run database migrations: `miniflux -migrate`
7. Start the process again

<h2 id="deb">Debian Systems <a class="anchor" href="#deb" title="Permalink">¶</a></h2>

Follow instructions mentioned above and run: `dpkg -i miniflux_2.x.x_amd64.deb`.

<h2 id="rpm">RPM Systems <a class="anchor" href="#rpm" title="Permalink">¶</a></h2>

Follow instructions mentioned above and run: `rpm -Uvh miniflux-2.x.x-1.0.x86_64.rpm`.

<h2 id="docker">Docker Containers <a class="anchor" href="#docker" title="Permalink">¶</a></h2>

- Pull the new image with the new tag: `docker pull miniflux/miniflux:2.x.x`
- Stop and remove the old container: `docker stop <container_name> && docker rm <container_name>`
- Start a new container with the latest tag: `docker run -d -p 80:8080 miniflux/miniflux:2.x.x`

If you use Docker Compose, define the new tag in the YAML file and restart the container.
Do not forget to run the database migrations if necessary.
