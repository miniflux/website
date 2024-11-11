---
title: How-To's
description: Examples and Tutorials
url: /docs/howto.html
---

Here are some examples of configuration:

- [How to enable WebAuthn / Passkey authentication?](#webauthn)
- [Use a Unix socket for Postgresql](#pg-unix-socket)
- [How to run Miniflux on port 443 or 80](#privileged-ports)
- [Reverse-Proxy Configuration](#reverse-proxy)
- [Reverse-Proxy with a subfolder](#reverse-proxy-subfolder)
- [Reverse-Proxy with a Unix socket](#reverse-proxy-unix-socket)
- [Systemd Socket Activation](#systemd-socket-activation)
- [Let's Encrypt Configuration](#lets-encrypt)
- [Manual HTTPS Configuration](#https)
- [OAuth2 and OpenID Connect Authentication](#oauth2)
- [Deploy Miniflux on Heroku](#heroku)
- [Deploy Miniflux on Google App Engine](#gae)

<h2 id="webauthn">How to enable WebAuthn / Passkey authentication? <a class="anchor" href="#webauthn" title="Permalink">¶</a></h2>

- Define `BASE_URL` with an appropriate value, for example: `https://miniflux.example.org/`. It won't work with an IP address like `127.0.0.1` in some browsers.
- Enable WebAuthn / Passkey: `WEBAUTHN=1`.

**You must enter the username on the login page if you are using a security key (also known as non-resident key in the WebAuthn protocol).**
This is not required for Passkeys which are discoverable credentials stored in the platform secure enclave, or in a trusted platform module (TPM).

<h2 id="pg-unix-socket">Use a Unix socket for Postgresql <a class="anchor" href="#pg-unix-socket" title="Permalink">¶</a></h2>

If you would like to connect via a Unix socket to Postgresql, set the parameter `host=/path/to/socket/folder`.

Example:

```bash
export DATABASE_URL="user=postgres password=postgres dbname=miniflux2 sslmode=disable host=/path/to/socket/folder"
./miniflux
```

<h2 id="privileged-ports">How to run Miniflux on port 443 or 80 <a class="anchor" href="#privileged-ports" title="Permalink">¶</a></h2>

Ports less than 1024 are reserved for privileged users.
If you have installed Miniflux with the RPM or Debian package, no further action is required. Systemd runs the process as the `miniflux` user with `CAP_NET_BIND_SERVICE` capability.

If you are not using systemd and want to give Miniflux the ability to bind to privileged ports as a non-root user, add the capability `CAP_NET_BIND_SERVICE` to the binary:

```bash
setcap cap_net_bind_service=+ep /usr/bin/miniflux
```

Check that the capability is added:

```bash
getcap /usr/bin/miniflux
/usr/bin/miniflux = cap_net_bind_service+ep
```

Note that you will have to do this operation each time you upgrade Miniflux.

<h2 id="reverse-proxy">Reverse-Proxy Configuration <a class="anchor" href="#reverse-proxy" title="Permalink">¶</a></h2>

You can use the reverse-proxy software of your choice, here an example with Nginx:

```bash
server {
    server_name     my.domain.tld;
    listen          80;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

This example assumes that you are running the Miniflux daemon on `127.0.0.1:8080`.

<h2 id="reverse-proxy-subfolder">Reverse-Proxy with a subfolder <a class="anchor" href="#reverse-proxy-subfolder" title="Permalink">¶</a></h2>

Since version 2.0.2, you can host your Miniflux instance under a subfolder.

You must define the environment variable `BASE_URL` for Miniflux, for example:

```bash
export BASE_URL=http://example.org/rss/
```

### Nginx Example

```bash
server {
    server_name     my.domain.tld;
    listen          80;

    location /rss/ {
        proxy_pass http://127.0.0.1:8080/rss/;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Alternative Nginx Configuration

```bash
server {
    server_name     my.domain.tld;
    listen          80;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

This example assumes that you are running the Miniflux daemon on `127.0.0.1:8080`.

Now you can access your Miniflux instance at `http://example.org/rss/`.
In this configuration, cookies are using the path `/rss`.

### Apache 2.4 Example

This configuration assumes the same base-url as the nginx-example.

```
ProxyRequests Off
<Proxy *>
    Order allow,deny
    Allow from all
</Proxy>

<Location "/rss/">
    ProxyPreserveHost On
    ProxyPass http://127.0.0.1:8080/rss/
    ProxyPassReverse http://127.0.0.1:8080/rss/
</Location>
```

Place this snippet inside your vhosts config, needed modules: `mod_proxy` and `mod_proxy_http`.

<h2 id="reverse-proxy-unix-socket">Reverse-Proxy with a Unix socket <a class="anchor" href="#reverse-proxy-unix-socket" title="Permalink">¶</a></h2>

If you prefer to use a Unix socket, change the environment variable `LISTEN_ADDR` to the path of your socket.

Configure Miniflux to use a Unix socket:

```bash
LISTEN_ADDR=/run/miniflux/miniflux.sock
```

The socket folder must be writeable by the miniflux user:

```bash
sudo mkdir /run/miniflux
sudo chown miniflux: /run/miniflux
```

Example with Nginx as reverse-proxy:

```bash
server {
    server_name     my.domain.tld;
    listen          80;

    location / {
        proxy_pass  http://unix:/run/miniflux/miniflux.sock;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

- By default, the socket has the permissions `0666` to make it accessible from other processes like Nginx or others.
- If you don't set the header `X-Forwarded-For`, Miniflux won't be able to determine the remote IP address.
- Listening on Unix socket is available only since Miniflux v2.0.13.

<h2 id="systemd-socket-activation">Systemd Socket Activation <a class="anchor" href="#systemd-socket-activation" title="Permalink">¶</a></h2>

<div class="warning">
This feature is experimental.
</div>

In this example, we are going to expose Miniflux on port 80 via Systemd.

Miniflux will be started by systemd at boot or on-demand.
The process is running under an unprivileged user `miniflux` and we load the environment variables from `/etc/miniflux.conf`.

Create a file `/etc/systemd/system/miniflux.socket`:

    [Unit]
    Description=Miniflux Socket

    [Socket]
    NoDelay=true

    # Listen on port 80.
    # To use a unix socket, define the absolute path here.
    ListenStream=80

    [Install]
    WantedBy=sockets.target

Create a file `/etc/systemd/system/miniflux.service`:

    [Unit]
    Description=Miniflux Service
    Requires=miniflux.socket

    [Service]
    ExecStart=/usr/bin/miniflux
    EnvironmentFile=/etc/miniflux.conf
    User=miniflux
    NonBlocking=true

    [Install]
    WantedBy=multi-user.target

Enable this:

    sudo systemctl enable miniflux.socket
    sudo systemctl enable miniflux.service

Tell systemd to listen on port 80 for us:

    sudo systemctl start miniflux.socket

If you go to `http://127.0.0.1/`, systemd will start the Miniflux service automatically.

If you watch the logs with `journalctl -u miniflux.service`, you will see `[INFO] Listening on systemd socket`.

<div class="info">
<ul>
    <li>Systemd socket activation is available only since Miniflux v2.0.13.</li>
    <li>When using this feature, Miniflux ignores <code>LISTEN_ADDR</code>.</li>
</ul>
</div>

<h2 id="lets-encrypt">Let's Encrypt Configuration <a class="anchor" href="#lets-encrypt" title="Permalink">¶</a></h2>

You could use Let's Encrypt to manage the SSL certificate automatically and activate HTTP/2.

```bash
export CERT_DOMAIN=my.domain.tld
miniflux
```

- Your server must be reachable publicly on port 443 and port 80 (http-01 challenge)
- In this mode, `LISTEN_ADDR` is automatically set to `:https`

<div class="note">
Miniflux supports http-01 challenge since the version 2.0.2.
</div>

<h2 id="https">Manual HTTPS Configuration <a class="anchor" href="#https" title="Permalink">¶</a></h2>

Here an example to generate a self-signed certificate:

```bash
# Generate the private key:
openssl genrsa -out server.key 2048
openssl ecparam -genkey -name secp384r1 -out server.key

# Generate the certificate:
openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650
```

Start the server like this:

```bash
# Configure the environment variables:
export CERT_FILE=/path/to/server.crt
export KEY_FILE=/path/to/server.key
export LISTEN_ADDR=":https"

# Start the server:
miniflux
```

Then you can access to your server by using an encrypted connection with the HTTP/2 protocol.

<h2 id="oauth2">OAuth2 and OpenID Connect Authentication <a class="anchor" href="#oauth2" title="Permalink">¶</a></h2>

OAuth2 allows you to sign in with an external provider.

### Google

1. Create a new project in Google Console
2. Create a new OAuth2 client
3. Set an authorized redirect URL, for example `https://my.domain.tld/oauth2/google/callback`
4. Define the OAuth2 environment variables and start the process

```
OAUTH2_PROVIDER=google
OAUTH2_CLIENT_ID=replace_me
OAUTH2_CLIENT_SECRET=replace_me
OAUTH2_REDIRECT_URL=https://miniflux.example.org/oauth2/google/callback
```

Now from the settings page, you can link your existing user to your Google account.

If you would like to authorize anyone to create a user account, you must set `OAUTH2_USER_CREATION=1`.
Since Google do not have the concept of username, the email address is used as username.

### OpenID Connect

1. Create a client in your OpenID Connect Provider, for example [Keycloak](https://www.keycloak.org)
2. Set Access Type confidental
3. Set Client ID, for example `miniflux`
4. Set valid Redirect URI, for example `https://miniflux.example.org/oauth2/oidc/callback`
5. Set valid Web Origins, for example `https://miniflux.example.org/oauth2/oidc/redirect`
6. Define the OAuth2 environment variables and start the process

```ini
OAUTH2_PROVIDER=oidc
OAUTH2_CLIENT_ID=replace_me
OAUTH2_CLIENT_SECRET=replace_me
OAUTH2_REDIRECT_URL=https://miniflux.example.org/oauth2/oidc/callback
OAUTH2_OIDC_DISCOVERY_ENDPOINT=https://my.oidc.provider.tld/auth/realms/id
```

You can use the OpenID Connect integration with different providers.

Note that the OIDC library automatically appends the `.well-known/openid-configuration`, this part has to be removed from the URL when setting `OAUTH2_OIDC_DISCOVERY_ENDPOINT`.

For example, Authentik discovery endpoint is `https://authentik.example.org/application/o/miniflux/.well-known/openid-configuration` and the `OAUTH2_OIDC_DISCOVERY_ENDPOINT` config option should be `https://authentik.example.org/application/o/miniflux/`.

- Since Miniflux 2.0.48, [OAuth2 PKCE](https://oauth.net/2/pkce/) is supported.
- Since Miniflux 2.0.48, the `profile` scope will be requested in addition to the `email` scope. If no email address is available, Miniflux will fallback to the `preferred_username` or `name` claims to populate Miniflux's `username` field.

#### Authentik

Example of Miniflux configuration with [Authentik](https://goauthentik.io):

```ini
OAUTH2_PROVIDER=oidc
OAUTH2_CLIENT_ID=replace_me
OAUTH2_CLIENT_SECRET=replace_me
OAUTH2_REDIRECT_URL=https://miniflux.example.org/oauth2/oidc/callback
# Authentik discovery endpoint is https://authentik.example.org/application/o/miniflux/.well-known/openid-configuration
OAUTH2_OIDC_DISCOVERY_ENDPOINT=https://authentik.example.org/application/o/miniflux/
OAUTH2_USER_CREATION=1
```

#### Kanidm

Example of Miniflux configuration with [Kanidm](https://kanidm.com):

```ini
OAUTH2_PROVIDER=oidc
OAUTH2_CLIENT_ID=replace_me
OAUTH2_CLIENT_SECRET=replace_me
OAUTH2_REDIRECT_URL=https://miniflux.example.org/oauth2/oidc/callback
OAUTH2_OIDC_DISCOVERY_ENDPOINT=https://kanidm.example.org/oauth2/openid/miniflux
OAUTH2_USER_CREATION=1
```

#### Zitadel

Example of Miniflux configuration with [Zitadel](https://zitadel.com):

- Type: Web
- Authentication: PKCE
- Redirect URIs:
    - `https://miniflux.example.org/oauth2/oidc/callback`
    - `https://miniflux.example.org/oauth2/oidc/redirect`
- Post Logout URIs:
    - `https://miniflux.example.org`

```ini
OAUTH2_PROVIDER=oidc
OAUTH2_CLIENT_ID=replace_me
OAUTH2_REDIRECT_URL=https://miniflux.example.org/oauth2/oidc/callback
OAUTH2_OIDC_DISCOVERY_ENDPOINT=https://zitadel.example.org
OAUTH2_USER_CREATION=1
```

<h2 id="heroku">Deploy Miniflux on Heroku <a class="anchor" href="#heroku" title="Permalink">¶</a></h2>

Since the version 2.0.6, you can deploy Miniflux on [Heroku](https://www.heroku.com/) in few seconds.

- Clone the repository on your machine: `git clone https://github.com/miniflux/v2.git`
- Switch to latest stable version `git checkout $(git describe --tags --abbrev=0)` (master is the development branch)
- Commit to master `git checkout -B master` (Heroku only deploy master by default)
- Create a new Heroku application: `heroku apps:create`
- Add the Postgresql addon: `heroku addons:create heroku-postgresql:hobby-dev`

Defines the environment variables to configure the application:

```
# Creates all tables in the database.
heroku config:set RUN_MIGRATIONS=1

# Creates the first user.
heroku config:set CREATE_ADMIN=1
heroku config:set ADMIN_USERNAME=admin
heroku config:set ADMIN_PASSWORD=test123
```

Then, deploy the application to Heroku:

```
git push heroku master
```

Once the application is deployed successfully, you don't need these variables anymore:

```
heroku config:unset CREATE_ADMIN
heroku config:unset ADMIN_USERNAME
heroku config:unset ADMIN_PASSWORD
```

- To watch the logs, use `heroku logs`.
- You can also run a one-off container to run the commands manually: `heroku run bash`.
  The Miniflux binary will be located into the `bin` folder.
- To update Miniflux, pull the new version from the repository and push to Heroku again.

<h2 id="gae">Deploy Miniflux on Google App Engine <a class="anchor" href="#gae" title="Permalink">¶</a></h2>

If you have experience with Terraform and want a setup that works out of the box, consider using this [Terraform module](https://github.com/huy-nguyen/terraform-google-miniflux).
Alternatively, you can follow these instructions:

- Create a Postgresql instance via Google Cloud SQL, then create a user and a new database
- Clone the repository and create a `app.yaml` file in the project root directory

```yaml
runtime: go111
env_variables:
    CLOUDSQL_CONNECTION_NAME: INSTANCE_CONNECTION_NAME
    CLOUDSQL_USER: replace-me
    CLOUDSQL_PASSWORD: top-secret

    CREATE_ADMIN: 1
    ADMIN_USERNAME: foobar
    ADMIN_PASSWORD: test123
    RUN_MIGRATIONS: 1
    DATABASE_URL: "user=replace-me password=top-secret host=/cloudsql/INSTANCE_CONNECTION_NAME dbname=miniflux"
```

Replace the values according to your project configuration.
As you can see, the database connection is made over a Unix socket on App Engine.

Last step, deploy your application:

```
gcloud app deploy
```

Refer to Google Cloud documentation for more details:

- <https://cloud.google.com/appengine/docs/standard/go111/building-app/>
- <https://cloud.google.com/appengine/docs/standard/go111/using-cloud-sql>

<div class="warning">
Running Miniflux on Google App Engine should work but it's considered experimental.
</div>
