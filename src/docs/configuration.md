title: Configuration Parameters
description: List of configuration parameters for Miniflux
template: doc
uri: docs/configuration.html
---
Miniflux doesn’t use any configuration file, **only environment variables**.

<h2 id="env-variables">Environment Variables <a class="anchor" href="#env-variables" title="Permalink">¶</a></h2>

| Variable Name               | Description                                                                      | Default Value                                                      |
| --------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `DEBUG`                     | Set the value to `1` to enable debug logs                                        | None                                                               |
| `WORKER_POOL_SIZE`          | Number of background workers                                                     | 5                                                                  |
| `POLLING_FREQUENCY`         | Refresh interval in minutes for feeds                                            | 60 (minutes)                                                       |
| `BATCH_SIZE`                | Number of feeds to send to the queue for each interval                           | 10                                                                 |
| `DATABASE_URL`              | Postgresql connection parameters                                                 | `user=postgres password=postgres dbname=miniflux2 sslmode=disable` |
| `DATABASE_MAX_CONNS`        | Maximum number of database connections                                           | 20                                                                 |
| `DATABASE_MIN_CONNS`        | Minimum number of database connections                                           | 1                                                                  |
| `ARCHIVE_READ_DAYS`         | Number of days after which marking read items as removed                         | `60`                                                               |
| `LISTEN_ADDR`               | Address to listen on (use absolute path for Unix socket)                         | `127.0.0.1:8080`                                                   |
| `PORT`                      | Override `LISTEN_ADDR` to `0.0.0.0:$PORT` (PaaS)                                 | None                                                               |
| `BASE_URL`                  | Base URL to generate HTML links and base path for cookies                        | `http://localhost/`                                                |
| `CLEANUP_FREQUENCY`         | Cleanup job frequency, remove old sessions and archive read entries              | 24 (hours)                                                         |
| `HTTPS`                     | Forces cookies to use secure flag and send HSTS headers                          | None                                                               |
| `DISABLE_HSTS`              | Disable HTTP Strict Transport Security header if HTTPS is set                    | None                                                               |
| `DISABLE_HTTP_SERVICE`      | Disable HTTP service                                                             | None                                                               |
| `DISABLE_SCHEDULER_SERVICE` | Disable scheduler service                                                        | None                                                               |
| `CERT_FILE`                 | Path to SSL certificate                                                          | None                                                               |
| `KEY_FILE`                  | Path to SSL private key                                                          | None                                                               |
| `CERT_DOMAIN`               | Use Let's Encrypt to get automatically a certificate for this domain             | None                                                               |
| `CERT_CACHE`                | Let's Encrypt cache directory                                                    | `/tmp/cert_cache`                                                  |
| `OAUTH2_PROVIDER`           | OAuth2 provider to use, at this time only `google` is supported                  | None                                                               |
| `OAUTH2_CLIENT_ID`          | OAuth2 client ID                                                                 | None                                                               |
| `OAUTH2_CLIENT_SECRET`      | OAuth2 client secret                                                             | None                                                               |
| `OAUTH2_REDIRECT_URL`       | OAuth2 redirect URL                                                              | None                                                               |
| `OAUTH2_USER_CREATION`      | Set to `1` to authorize OAuth2 user creation                                     | None                                                               |
| `RUN_MIGRATIONS`            | Set to `1` to run database migrations                                            | None                                                               |
| `CREATE_ADMIN`              | Set to `1` to create an admin user from environment variables                    | None                                                               |
| `ADMIN_USERNAME`            | Admin user login, used only if `CREATE_ADMIN` is enabled                         | None                                                               |
| `ADMIN_PASSWORD`            | Admin user password, used only if `CREATE_ADMIN` is enabled                      | None                                                               |
| `POCKET_CONSUMER_KEY`       | Pocket consumer API key for all users                                            | None                                                               |
| `PROXY_IMAGES`              | Avoids mixed content warnings for external images: `http-only`, `all`, or `none` | `http-only`                                                        |

<p class="info">
Systemd uses the file `/etc/miniflux.conf` to populate environment variables.
</p>

<h2 id="dsn">Database Connection Parameters <a class="anchor" href="#dsn" title="Permalink">¶</a></h2>

Miniflux uses the Golang library [pq](https://github.com/lib/pq) to communicate with PostgreSQL.
The list of connection parameters are available on [this page](https://godoc.org/github.com/lib/pq#hdr-Connection_String_Parameters).

The default value for `DATABASE_URL` is `user=postgres password=postgres dbname=miniflux2 sslmode=disable`.

You could also use the URL format `postgres://postgres:postgres@localhost/miniflux2?sslmode=disable`.

<div class="warning">
Password that contains special characters like ^ might be rejected since Miniflux 2.0.3. <a href="https://go-review.googlesource.com/c/go/+/87038">Golang v1.10 is now validating the password</a> and will return this error: <code>net/url: invalid userinfo</code>.
To avoid this issue, do not use the URL format for `DATABASE_URL` or make sure the password is URL encoded.
</div>
