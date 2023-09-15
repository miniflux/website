---
title: Configuration Parameters
description: List of configuration parameters for Miniflux
url: /docs/configuration.html
---
Miniflux can use a configuration file and environment variables.

The configuration file is loaded first if specified. Environment variables takes precedence over the options defined in the configuration file.

- [Configuration Options](#options)
- [Configuration File](#config-file)
- [Database Connection Parameters](#dsn)

<h2 id="options">Configuration Options <a class="anchor" href="#options" title="Permalink">¶</a></h2>

<dl class="configs">
    <dt id="debug"><a href="#debug"><code>DEBUG</code></a></dt>
    <dd>
        <p>Toggle debug mode (increase log level).</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="fetch-odysee-watch-time"><a href="#fetch-odysee-watch-time"><code>FETCH_ODYSEE_WATCH_TIME</code></a></dt>
    <dd>
        <p>Set the value to 1 to scrape video duration from Odysee website and use it as a reading time.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="fetch-youtube-watch-time"><a href="#fetch-youtube-watch-time"><code>FETCH_YOUTUBE_WATCH_TIME</code></a></dt>
    <dd>
        <p>Set the value to 1 to scrape video duration from YouTube website and use it as a reading time.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="youtube-embed-url-override"><a href="#youtube-embed-url-override"><code>YOUTUBE_EMBED_URL_OVERRIDE</code></a></dt>
    <dd>
        <p>YouTube URL which will be used for embeds.</p>
        <p><em>Default is https://www.youtube-nocookie.com/embed/</em></p>
    </dd>
    <dt id="log-date-time"><a href="#log-date-time"><code>LOG_DATE_TIME</code></a></dt>
    <dd>
        <p>Show date and time in log messages.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="worker-pool-size"><a href="#worker-pool-size"><code>WORKER_POOL_SIZE</code></a></dt>
    <dd>
        <p>Number of background workers to refresh feeds.</p>
        <p><em>Default is 5 workers.</em></p>
    </dd>
    <dt id="polling-frequency"><a href="#polling-frequency"><code>POLLING_FREQUENCY</code></a></dt>
    <dd>
        <p>Refresh interval in minutes for feeds.</p>
        <p><em>Default is 60 minutes.</em></p>
    </dd>
    <dt id="polling-parsing-error-limit"><a href="#polling-parsing-error-limit"><code>POLLING_PARSING_ERROR_LIMIT</code></a></dt>
    <dd>
        <p>
            The maximum number of parsing errors that the program will try before stopping polling a feed.
            Once the limit is reached, the user must refresh the feed manually. Set to 0 for unlimited.
        </p>
        <p><em>Default is 3.</em></p>
    </dd>
    <dt id="batch-size"><a href="#batch-size"><code>BATCH_SIZE</code></a></dt>
    <dd>
        <p>Number of feeds to send to the queue for each interval.</p>
        <p><em>Default is 100 feeds.</em></p>
    </dd>
    <dt id="polling-scheduler"><a href="#polling-scheduler"><code>POLLING_SCHEDULER</code></a></dt>
    <dd>
        <p>
            Scheduler used for polling feeds.
            Possible values are <code>round_robin</code> or <code>entry_frequency</code>.
        </p>
        <p>
            The maximum number of feeds polled for a given period is subject to <code>POLLING_FREQUENCY</code> and <code>BATCH_SIZE</code>.
        </p>
        <p>
            When <code>entry_frequency</code> is selected, the refresh interval for a given feed is equal to the average updating interval of the last week of the feed.
            The actual number of feeds polled will not exceed the maximum number of feeds that could be polled for a given period.
        </p>
        <p><em>Default is <code>round_robin</code>.</em></p>
    </dd>
    <dt id="scheduler-entry-frequency-factor"><a href="#scheduler-entry-frequency-factor"><code>SCHEDULER_ENTRY_FREQUENCY_FACTOR</code></a></dt>
    <dd>
        <p>Factor to increase refresh frequency for the entry frequency scheduler.</p>
        <p><em>Default is 1.</em></p>
    </dd>
    <dt id="scheduler-entry-frequency-max-interval"><a href="#scheduler-entry-frequency-max-interval"><code>SCHEDULER_ENTRY_FREQUENCY_MAX_INTERVAL</code></a></dt>
    <dd>
        <p>Maximum interval in minutes for the entry frequency scheduler.</p>
        <p><em>Default is 24 hours.</em></p>
    </dd>
    <dt id="scheduler-entry-frequency-min-interval"><a href="#scheduler-entry-frequency-min-interval"><code>SCHEDULER_ENTRY_FREQUENCY_MIN_INTERVAL</code></a></dt>
    <dd>
        <p>Minimum interval in minutes for the entry frequency scheduler.</p>
        <p><em>Default is 5 minutes.</em></p>
    </dd>
    <dt id="database-url"><a href="#database-url"><code>DATABASE_URL</code></a></dt>
    <dd>
        <p>Postgresql connection parameters. See <a href="https://pkg.go.dev/github.com/lib/pq#hdr-Connection_String_Parameters">lib/pq</a> for more details.</p>
        <p><em>Default is <code>user=postgres password=postgres dbname=miniflux2 sslmode=disable</code></em></p>
    </dd>
    <dt id="database-max-conns"><a href="#database-max-conns"><code>DATABASE_MAX_CONNS</code></a></dt>
    <dd>
        <p>Maximum number of database connections.</p>
        <p><em>Default is <code>20</code></em></p>
    </dd>
    <dt id="database-min-conns"><a href="#database-min-conns"><code>DATABASE_MIN_CONNS</code></a></dt>
    <dd>
        <p>Minimum number of database connections.</p>
        <p><em>Default is <code>1</code></em></p>
    </dd>
    <dt id="database-connection-lifetime"><a href="#database-connection-lifetime"><code>DATABASE_CONNECTION_LIFETIME</code></a></dt>
    <dd>
        <p>Set the maximum amount of time a connection may be reused.</p>
        <p><em>Default is 5 minutes</em></p>
    </dd>
    <dt id="listen-addr"><a href="#listen-addr"><code>LISTEN_ADDR</code></a></dt>
    <dd>
        <p>Address to listen on. Use absolute path for a Unix socket.</p>
        <p><em>Default is <code>127.0.0.1:8080</code>.</em></p>
    </dd>
    <dt id="port"><a href="#port"><code>PORT</code></a></dt>
    <dd>
        <p>Override <code>LISTEN_ADDR</code> to <code>0.0.0.0:$PORT</code> (Automatic configuration for <abbr title="Platform as a service">PaaS</abbr>).</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="base-url"><a href="#base-url"><code>BASE_URL</code></a></dt>
    <dd>
        <p>Base URL to generate HTML links and base path for cookies.</p>
        <p><em>Default is <code>http://localhost/</code>.</em></p>
    </dd>
    <dt id="cleanup-frequency-hours"><a href="#cleanup-frequency-hours"><code>CLEANUP_FREQUENCY_HOURS</code></a></dt>
    <dd>
        <p>Cleanup job frequency to remove old sessions and archive entries.</p>
        <p><em>Default is 24 hours.</em></p>
    </dd>
    <dt id="cleanup-archive-unread-days"><a href="#cleanup-archive-unread-days"><code>CLEANUP_ARCHIVE_UNREAD_DAYS</code></a></dt>
    <dd>
        <p>Number of days after marking unread items as removed. Use <code>-1</code> to disable this feature.</p>
        <p><em>Default is 180 days.</em></p>
    </dd>
    <dt id="cleanup-archive-read-days"><a href="#cleanup-archive-read-days"><code>CLEANUP_ARCHIVE_READ_DAYS</code></a></dt>
    <dd>
        <p>Number of days after which marking read items as removed. Use <code>-1</code> to disable this feature.</p>
        <p><em>Default is 60 days.</em></p>
    </dd>
    <dt id="cleanup-archive-batch-size"><a href="#cleanup-archive-batch-size"><code>CLEANUP_ARCHIVE_BATCH_SIZE</code></a></dt>
    <dd>
        <p>Number of entries to archive for each job interval.</p>
        <p><em>Default is 10000 entries.</em></p>
    </dd>
    <dt id="cleanup-remove-sessions-days"><a href="#cleanup-remove-sessions-days"><code>CLEANUP_REMOVE_SESSIONS_DAYS</code></a></dt>
    <dd>
        <p>Number of days after removing old user sessions from the database.</p>
        <p><em>Default is 30 days.</em></p>
    </dd>
    <dt id="https"><a href="#https"><code>HTTPS</code></a></dt>
    <dd>
        <p>Forces cookies to use secure flag. Send <abbr title="HTTP Strict Transport Security">HSTS</abbr> HTTP header. Enabled automatically if the HTTP header <code>X-Forwarded-Proto</code> is set to <code>https</code>.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="disable-hsts"><a href="#disable-hsts"><code>DISABLE_HSTS</code></a></dt>
    <dd>
        <p>Disable HTTP Strict Transport Security header if <code>$HTTPS</code> is set.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="disable-http-service"><a href="#disable-http-service"><code>DISABLE_HTTP_SERVICE</code></a></dt>
    <dd>
        <p>Disable HTTP service.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="disable-scheduler-service"><a href="#disable-scheduler-service"><code>DISABLE_SCHEDULER_SERVICE</code></a></dt>
    <dd>
        <p>Disable scheduler service.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="cert-file"><a href="#cert-file"><code>CERT_FILE</code></a></dt>
    <dd>
        <p>Path to SSL certificate.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="key-file"><a href="#key-file"><code>KEY_FILE</code></a></dt>
    <dd>
        <p>Path to SSL private key.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="cert-domain"><a href="#cert-domain"><code>CERT_DOMAIN</code></a></dt>
    <dd>
        <p>Use <a href="https://letsencrypt.org/">Let's Encrypt</a> to get automatically a certificate for the domain specified in <code>$CERT_DOMAIN</code>.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="metrics-collector"><a href="#metrics-collector"><code>METRICS_COLLECTOR</code></a></dt>
    <dd>
        <p>Set to <code>1</code> to enable metrics collection. It exposes a <code>/metrics</code> endpoint that can be used with <a href="https://prometheus.io/">Prometheus Monitoring software</a>.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="metrics-refresh-interval"><a href="#metrics-refresh-interval"><code>METRICS_REFRESH_INTERVAL</code></a></dt>
    <dd>
        <p>Refresh interval to collect database metrics.</p>
        <p><em>Default is 60 seconds.</em></p>
    </dd>
    <dt id="metrics-allowed-networks"><a href="#metrics-allowed-networks"><code>METRICS_ALLOWED_NETWORKS</code></a></dt>
    <dd>
        <p>List of networks allowed to access the <code>/metrics</code> endpoint (comma-separated values).</p>
        <p><em>Default is <code>127.0.0.1/8</code>.</em></p>
    </dd>
    <dt id="metrics-username"><a href="#metrics-username"><code>METRICS_USERNAME</code></a></dt>
    <dd>
        <p>Metrics endpoint username for basic HTTP authentication.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="metrics-username-file"><a href="#metrics-username-file"><code>METRICS_USERNAME_FILE</code></a></dt>
    <dd>
        <p>Path to a file that contains the username for the metrics endpoint HTTP authentication.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="metrics-password"><a href="#metrics-password"><code>METRICS_PASSWORD</code></a></dt>
    <dd>
        <p>Metrics endpoint password for basic HTTP authentication.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="metrics-password-file"><a href="#metrics-password-file"><code>METRICS_PASSWORD_FILE</code></a></dt>
    <dd>
        <p>Path to a file that contains the password for the metrics endpoint HTTP authentication.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-provider"><a href="#oauth2-provider"><code>OAUTH2_PROVIDER</code></a></dt>
    <dd>
        <p>OAuth2 provider. Possible values are <code>google</code> or <code>oidc</code> for a generic OpenID Connect provider.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-client-id"><a href="#oauth2-client-id"><code>OAUTH2_CLIENT_ID</code></a></dt>
    <dd>
        <p>OAuth2 client ID.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-client-id-file"><a href="#oauth2-client-id-file"><code>OAUTH2_CLIENT_ID_FILE</code></a></dt>
    <dd>
        <p>Path to a client ID exposed as a file, it should contain <code>$OAUTH2_CLIENT_ID</code> value.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-client-secret"><a href="#oauth2-client-secret"><code>OAUTH2_CLIENT_SECRET</code></a></dt>
    <dd>
        <p>OAuth2 client secret.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-client-secret-file"><a href="#oauth2-client-secret-file"><code>OAUTH2_CLIENT_SECRET_FILE</code></a></dt>
    <dd>
        <p>Path to a secret key exposed as a file, it should contain <code>$OAUTH2_CLIENT_SECRET</code> value.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-redirect-url"><a href="#oauth2-redirect-url"><code>OAUTH2_REDIRECT_URL</code></a></dt>
    <dd>
        <p>OAuth2 redirect URL. This URL must be registered with the provider and is something like <code>https://miniflux.example.org/oauth2/oidc/callback</code></p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-oidc-discovery-endpoint"><a href="#oauth2-oidc-discovery-endpoint"><code>OAUTH2_OIDC_DISCOVERY_ENDPOINT</code></a></dt>
    <dd>
        <p>OpenID Connect discovery endpoint.</p>
        <p>Note that the OIDC library automatically appends the <code>.well-known/openid-configuration</code>, this part has to be removed from the URL when setting <code>OAUTH2_OIDC_DISCOVERY_ENDPOINT</code>.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-user-creation"><a href="#oauth2-user-creation"><code>OAUTH2_USER_CREATION</code></a></dt>
    <dd>
        <p>Set to <code>1</code> to authorize OAuth2 user creation.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="run-migrations"><a href="#run-migrations"><code>RUN_MIGRATIONS</code></a></dt>
    <dd>
        <p>Set to <code>1</code> to run database migrations during application startup.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="create-admin"><a href="#create-admin"><code>CREATE_ADMIN</code></a></dt>
    <dd>
        <p>Set to <code>1</code> to create an admin user from environment variables.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="admin-username"><a href="#admin-username"><code>ADMIN_USERNAME</code></a></dt>
    <dd>
        <p>Admin user login, it's used only if <code>CREATE_ADMIN</code> is enabled.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="admin-username-file"><a href="#admin-username-file"><code>ADMIN_USERNAME_FILE</code></a></dt>
    <dd>
        <p>Path to a secret key exposed as a file, it should contain <code>$ADMIN_USERNAME</code> value.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="admin-password"><a href="#admin-password"><code>ADMIN_PASSWORD</code></a></dt>
    <dd>
        <p>Admin user password, it's used only if <code>CREATE_ADMIN</code> is enabled.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="admin-password-file"><a href="#admin-password-file"><code>ADMIN_PASSWORD_FILE</code></a></dt>
    <dd>
        <p>Path to a secret key exposed as a file, it should contain <code>$ADMIN_PASSWORD</code> value.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="pocket-consumer-key"><a href="#pocket-consumer-key"><code>POCKET_CONSUMER_KEY</code></a></dt>
    <dd>
        <p>Pocket consumer API key for all users.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="pocket-consumer-key-file"><a href="#pocket-consumer-key-file"><code>POCKET_CONSUMER_KEY_FILE</code></a></dt>
    <dd>
        <p>Path to a secret key exposed as a file, it should contain <code>$POCKET_CONSUMER_KEY</code> value.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="proxy-option"><a href="#proxy-option"><code>PROXY_OPTION</code></a></dt>
    <dd>
        <p>Avoids mixed content warnings for external media: <code>http-only</code>, <code>all</code>, or <code>none</code>.</p>
        <p><em>Default is <code>http-only</code>.</em></p>
    </dd>
    <dt id="proxy-media-types"><a href="#proxy-media-types"><code>PROXY_MEDIA_TYPES</code></a></dt>
    <dd>
        <p>A list of media types to proxify (comma-separated values): <code>image</code>, <code>audio</code>, <code>video</code>.</p>
        <p><em>Default is <code>image</code>.</em></p>
    </dd>
    <dt id="proxy-http-client-timeout"><a href="#proxy-http-client-timeout"><code>PROXY_HTTP_CLIENT_TIMEOUT</code></a></dt>
    <dd>
        <p>Time limit in seconds before the proxy HTTP client cancel the request.</p>
        <p>Default is 120 seconds.</p>
    </dd>
    <dt id="proxy-url"><a href="#proxy-url"><code>PROXY_URL</code></a></dt>
    <dd>
        <p>Sets a server to proxy media through.</p>
        <p>Default is empty, miniflux does the proxying.</p>
    </dd>
    <dt id="http-client-timeout"><a href="#http-client-timeout"><code>HTTP_CLIENT_TIMEOUT</code></a></dt>
    <dd>
        <p>Time limit in seconds before the HTTP client cancel the request.</p>
        <p><em>Default is 20 seconds.</em></p>
    </dd>
    <dt id="http-client-max-body-size"><a href="#http-client-max-body-size"><code>HTTP_CLIENT_MAX_BODY_SIZE</code></a></dt>
    <dd>
        <p>Maximum body size for HTTP requests in Mebibyte (MiB).</p>
        <p><em>Default is 15 MiB.</em></p>
    </dd>
    <dt id="http-client-proxy"><a href="#http-client-proxy"><code>HTTP_CLIENT_PROXY</code></a></dt>
    <dd>
        <p>Proxy URL for the HTTP client. For example: <code>http://127.0.0.1:8888</code>.</p>
        <p>This proxy is used only when the feed has the option "Fetch via proxy" enabled.</p>
        <p>If you prefer to have a proxy for all outgoing requests, use the environment variables <code>HTTP_PROXY</code> or <code>HTTPS_PROXY</code>, look at the official <a href="https://golang.org/pkg/net/http/#ProxyFromEnvironment">Golang documentation</a> for more details.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="http-client-user-agent"><a href="#http-client-user-agent"><code>HTTP_CLIENT_USER_AGENT</code></a></dt>
    <dd>
        <p>
            The default User-Agent header to use for the HTTP client. Can be overridden in per-feed settings.
            When empty, Miniflux uses a default User-Agent that includes the Miniflux version.
        </p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="auth-proxy-header"><a href="#auth-proxy-header"><code>AUTH_PROXY_HEADER</code></a></dt>
    <dd>
        <p>Proxy authentication HTTP header.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="auth-proxy-user-creation"><a href="#auth-proxy-user-creation"><code>AUTH_PROXY_USER_CREATION</code></a></dt>
    <dd>
        <p>Enable user creation based on proxy authentication information.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="maintenance-mode"><a href="#maintenance-mode"><code>MAINTENANCE_MODE</code></a></dt>
    <dd>
        <p>
            Set to 1 to enable maintenance mode.
            Maintenance mode disables the web ui and show a text message to the users.
        </p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="maintenance-message"><a href="#maintenance-message"><code>MAINTENANCE_MESSAGE</code></a></dt>
    <dd>
        <p>Define a custom maintenance message.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="server-timing-header"><a href="#server-timing-header"><code>SERVER_TIMING_HEADER</code></a></dt>
    <dd>
        <p>Set the value to 1 to enable server-timing headers.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="watchdog"><a href="#watchdog"><code>WATCHDOG</code></a></dt>
    <dd>
        <p>Enable or disable Systemd watchdog.</p>
        <p><em>Enabled by default.</em></p>
    </dd>
    <dt id="invidious-instance"><a href="#invidious-instance"><code>INVIDIOUS_INSTANCE</code></a></dt>
    <dd>
        <p>Set a custom invidious instance to use.</p>
        <p><em>Default is yewtu.be.</em></p>
    </dd>
</dl>

<h2 id="config-file">Configuration File <a class="anchor" href="#config-file" title="Permalink">¶</a></h2>

The configuration file is optional. It's a text file that follow these rules:

- Miniflux expects each line to be in `KEY=VALUE` format.
- Lines beginning with `#` are processed as comments and ignored.
- Blank lines are ignored.
- There is no variable interpolation.

Keys are the same as the environment variables described above.

Example:

```
DEBUG=on
WORKER_POOL_SIZE=20
```

Environment variables override the values defined in the config file.

To specify a configuration file, use the command line argument `-config-file` or `-c`.

You can also dump interpreted values with the flag `-config-dump` for debugging.

<p class="info">
Systemd uses the file <code>/etc/miniflux.conf</code> to populate environment variables.
</p>

<h2 id="dsn">Database Connection Parameters <a class="anchor" href="#dsn" title="Permalink">¶</a></h2>

Miniflux uses the Golang library [pq](https://github.com/lib/pq) to communicate with PostgreSQL.
The list of connection parameters are available on [this page](https://godoc.org/github.com/lib/pq#hdr-Connection_String_Parameters).

The default value for `DATABASE_URL` is `user=postgres password=postgres dbname=miniflux2 sslmode=disable`.

You could also use the URL format `postgres://postgres:postgres@localhost/miniflux2?sslmode=disable`.

<div class="warning">
Password that contains special characters like ^ might be rejected since Miniflux 2.0.3. <a href="https://go-review.googlesource.com/c/go/+/87038">Golang v1.10 is now validating the password</a> and will return this error: <code>net/url: invalid userinfo</code>.
To avoid this issue, do not use the URL format for <code>DATABASE_URL</code> or make sure the password is URL encoded.
</div>
