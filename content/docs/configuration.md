---
title: Configuration Parameters
description: List of configuration parameters for Miniflux
url: /docs/configuration.html
---

Miniflux can use a configuration file and environment variables.

The configuration file is loaded first if specified. Environment variables take precedence over the options defined in the configuration file.

Boolean options accept the following values (case-insensitive): 1/0, yes/no, true/false, on/off.
For variables ending in <code>_FILE</code>, the value is a path to a file that contains the corresponding secret value.

- [Configuration Options](#options)
- [Configuration File](#config-file)

<h2 id="options">Configuration Options <a class="anchor" href="#options" title="Permalink">¶</a></h2>

<dl class="configs">
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
    <dt id="auth-proxy-header"><a href="#auth-proxy-header"><code>AUTH_PROXY_HEADER</code></a></dt>
    <dd>
        <p>Proxy authentication HTTP header.</p>
        <p>The option <code>TRUSTED_REVERSE_PROXY_NETWORKS</code> must be configured to allow the proxy to authenticate users.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="auth-proxy-user-creation"><a href="#auth-proxy-user-creation"><code>AUTH_PROXY_USER_CREATION</code></a></dt>
    <dd>
        <p>Set to 1 to create users based on proxy authentication information.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="base-url"><a href="#base-url"><code>BASE_URL</code></a></dt>
    <dd>
        <p>Base URL to generate HTML links and base path for cookies.</p>
        <p><em>Default is <code>http://localhost</code>.</em></p>
    </dd>
    <dt id="batch-size"><a href="#batch-size"><code>BATCH_SIZE</code></a></dt>
    <dd>
        <p>Number of feeds to send to the queue for each interval.</p>
        <p><em>Default is 100 feeds.</em></p>
    </dd>
    <dt id="cert-domain"><a href="#cert-domain"><code>CERT_DOMAIN</code></a></dt>
    <dd>
        <p>Use <a href="https://letsencrypt.org/">Let's Encrypt</a> to get automatically a certificate for the domain specified in <code>$CERT_DOMAIN</code>.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="cert-file"><a href="#cert-file"><code>CERT_FILE</code></a></dt>
    <dd>
        <p>Path to SSL certificate.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="cleanup-archive-batch-size"><a href="#cleanup-archive-batch-size"><code>CLEANUP_ARCHIVE_BATCH_SIZE</code></a></dt>
    <dd>
        <p>Number of entries to archive for each job interval.</p>
        <p><em>Default is 10000 entries.</em></p>
    </dd>
    <dt id="cleanup-archive-read-days"><a href="#cleanup-archive-read-days"><code>CLEANUP_ARCHIVE_READ_DAYS</code></a></dt>
    <dd>
        <p>Number of days after marking read entries as removed. Set to <code>-1</code> to keep all read entries.</p>
        <p><em>Default is 60 days.</em></p>
    </dd>
    <dt id="cleanup-archive-unread-days"><a href="#cleanup-archive-unread-days"><code>CLEANUP_ARCHIVE_UNREAD_DAYS</code></a></dt>
    <dd>
        <p>Number of days after marking unread entries as removed. Set to <code>-1</code> to keep all unread entries.</p>
        <p><em>Default is 180 days.</em></p>
    </dd>
    <dt id="cleanup-frequency-hours"><a href="#cleanup-frequency-hours"><code>CLEANUP_FREQUENCY_HOURS</code></a></dt>
    <dd>
        <p>Cleanup job frequency. Remove old sessions and archive entries.</p>
        <p><em>Default is 24 hours.</em></p>
    </dd>
    <dt id="cleanup-remove-sessions-days"><a href="#cleanup-remove-sessions-days"><code>CLEANUP_REMOVE_SESSIONS_DAYS</code></a></dt>
    <dd>
        <p>Number of days after removing old user sessions from the database.</p>
        <p><em>Default is 30 days.</em></p>
    </dd>
    <dt id="create-admin"><a href="#create-admin"><code>CREATE_ADMIN</code></a></dt>
    <dd>
        <p>Set to 1 to create an admin user from environment variables.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="database-connection-lifetime"><a href="#database-connection-lifetime"><code>DATABASE_CONNECTION_LIFETIME</code></a></dt>
    <dd>
        <p>Set the maximum amount of time in minutes a connection may be reused.</p>
        <p><em>Default is 5 minutes.</em></p>
    </dd>
    <dt id="database-max-conns"><a href="#database-max-conns"><code>DATABASE_MAX_CONNS</code></a></dt>
    <dd>
        <p>Maximum number of database connections.</p>
        <p><em>Default is <code>20</code>.</em></p>
    </dd>
    <dt id="database-min-conns"><a href="#database-min-conns"><code>DATABASE_MIN_CONNS</code></a></dt>
    <dd>
        <p>Minimum number of database connections.</p>
        <p><em>Default is <code>1</code>.</em></p>
    </dd>
    <dt id="database-url"><a href="#database-url"><code>DATABASE_URL</code></a></dt>
    <dd>
        <p>PostgreSQL connection parameters.</p>
        <p><em>Default is "<code>user=postgres password=postgres dbname=miniflux2 sslmode=disable</code>".</em></p>
    </dd>
    <dt id="database-url-file"><a href="#database-url-file"><code>DATABASE_URL_FILE</code></a></dt>
    <dd>
        <p>Path to a secret key exposed as a file, it should contain <code>$DATABASE_URL</code> value.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="disable-api"><a href="#disable-api"><code>DISABLE_API</code></a></dt>
    <dd>
        <p>Disable miniflux's API.</p>
        <p><em>Default is false (The API is enabled).</em></p>
    </dd>
    <dt id="disable-hsts"><a href="#disable-hsts"><code>DISABLE_HSTS</code></a></dt>
    <dd>
        <p>Disable HTTP Strict Transport Security header if <code>HTTPS</code> is set.</p>
        <p><em>Default is false (The HSTS is enabled).</em></p>
    </dd>
    <dt id="disable-http-service"><a href="#disable-http-service"><code>DISABLE_HTTP_SERVICE</code></a></dt>
    <dd>
        <p>Set the value to 1 to disable the HTTP service.</p>
        <p><em>Default is false (The HTTP service is enabled).</em></p>
    </dd>
    <dt id="disable-local-auth"><a href="#disable-local-auth"><code>DISABLE_LOCAL_AUTH</code></a></dt>
    <dd>
        <p>Disable local authentication.</p>
        <p>When set to true, the username/password form is hidden from the login screen, and the options to change username/password or unlink OAuth2 account are hidden from the settings page.</p>
        <p><em>Default is false.</em></p>
    </dd>
    <dt id="disable-scheduler-service"><a href="#disable-scheduler-service"><code>DISABLE_SCHEDULER_SERVICE</code></a></dt>
    <dd>
        <p>Set the value to 1 to disable the internal scheduler service.</p>
        <p><em>Default is false (The internal scheduler service is enabled).</em></p>
    </dd>
    <dt id="fetch-bilibili-watch-time"><a href="#fetch-bilibili-watch-time"><code>FETCH_BILIBILI_WATCH_TIME</code></a></dt>
    <dd>
        <p>Set the value to 1 to scrape video duration from Bilibili website and use it as a reading time.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="fetch-nebula-watch-time"><a href="#fetch-nebula-watch-time"><code>FETCH_NEBULA_WATCH_TIME</code></a></dt>
    <dd>
        <p>Set the value to 1 to scrape video duration from Nebula website and use it as a reading time.</p>
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
    <dt id="force-refresh-interval"><a href="#force-refresh-interval"><code>FORCE_REFRESH_INTERVAL</code></a></dt>
    <dd>
        <p>The minimum interval in minutes for manual refresh.</p>
        <p><em>Default is 30 minutes.</em></p>
    </dd>
    <dt id="http-client-max-body-size"><a href="#http-client-max-body-size"><code>HTTP_CLIENT_MAX_BODY_SIZE</code></a></dt>
    <dd>
        <p>Maximum body size for HTTP requests in Mebibyte (MiB).</p>
        <p><em>Default is 15 MiB.</em></p>
    </dd>
    <dt id="http-client-proxies"><a href="#http-client-proxies"><code>HTTP_CLIENT_PROXIES</code></a></dt>
    <dd>
        <p>Enable proxy rotation for outgoing requests by providing a comma-separated list of proxy URLs.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="http-client-proxy"><a href="#http-client-proxy"><code>HTTP_CLIENT_PROXY</code></a></dt>
    <dd>
        <p>Proxy URL to use when the "Fetch via proxy" feed option is enabled. For example: <code>http://127.0.0.1:8888</code>.</p>
        <p>If you prefer to have a proxy for all outgoing requests, use the environment variables <code>HTTP_PROXY</code> or <code>HTTPS_PROXY</code>, look at the official <a href="https://golang.org/pkg/net/http/#ProxyFromEnvironment">Golang documentation</a> for more details.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="http-client-timeout"><a href="#http-client-timeout"><code>HTTP_CLIENT_TIMEOUT</code></a></dt>
    <dd>
        <p>Time limit in seconds before the HTTP client cancels the request.</p>
        <p><em>Default is 20 seconds.</em></p>
    </dd>
    <dt id="http-client-user-agent"><a href="#http-client-user-agent"><code>HTTP_CLIENT_USER_AGENT</code></a></dt>
    <dd>
        <p>
            The default User-Agent header to use for the HTTP client. Can be overridden in per-feed settings.
            When empty, Miniflux uses a default User-Agent that includes the Miniflux version.
        </p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="http-server-timeout"><a href="#http-server-timeout"><code>HTTP_SERVER_TIMEOUT</code></a></dt>
    <dd>
        <p>Read, write, and idle timeout in seconds for the HTTP server.</p>
        <p><em>Default is 300 seconds.</em></p>
    </dd>
    <dt id="https"><a href="#https"><code>HTTPS</code></a></dt>
    <dd>
        <p>Forces cookies to use secure flag and send HSTS header.</p>
        <p><em>Default is disabled.</em></p>
    </dd>
    <dt id="icon-fetch-allow-private-networks"><a href="#icon-fetch-allow-private-networks"><code>ICON_FETCH_ALLOW_PRIVATE_NETWORKS</code></a></dt>
    <dd>
        <p>Set to 1 to allow downloading favicons that resolve to private or loopback networks.</p>
        <p><em>Disabled by default, private networks are refused.</em></p>
    </dd>
    <dt id="invidious-instance"><a href="#invidious-instance"><code>INVIDIOUS_INSTANCE</code></a></dt>
    <dd>
        <p>Set a custom invidious instance to use.</p>
        <p><em>Default is yewtu.be.</em></p>
    </dd>
    <dt id="key-file"><a href="#key-file"><code>KEY_FILE</code></a></dt>
    <dd>
        <p>Path to SSL private key.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="listen-addr"><a href="#listen-addr"><code>LISTEN_ADDR</code></a></dt>
    <dd>
        <p>Address to listen on. Use absolute path to listen on Unix socket (<code>/var/run/miniflux.sock</code>).</p>
        <p>Multiple addresses can be specified, separated by commas. For example: <code>127.0.0.1:8080, 127.0.0.1:8081</code>.</p>
        <p><em>Default is <code>127.0.0.1:8080</code>.</em></p>
    </dd>
    <dt id="log-date-time"><a href="#log-date-time"><code>LOG_DATE_TIME</code></a></dt>
    <dd>
        <p>Display the date and time in log messages.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="log-file"><a href="#log-file"><code>LOG_FILE</code></a></dt>
    <dd>
        <p>Supported values are <code>stderr</code>, <code>stdout</code>, or a file name.</p>
        <p><em>Default is <code>stderr</code>.</em></p>
    </dd>
    <dt id="log-format"><a href="#log-format"><code>LOG_FORMAT</code></a></dt>
    <dd>
        <p>Supported log formats are <code>text</code> or <code>json</code>.</p>
        <p><em>Default is <code>text</code>.</em></p>
    </dd>
    <dt id="log-level"><a href="#log-level"><code>LOG_LEVEL</code></a></dt>
    <dd>
        <p>Supported values are <code>debug</code>, <code>info</code>, <code>warning</code>, or <code>error</code>.</p>
        <p><em>Default is <code>info</code>.</em></p>
    </dd>
    <dt id="maintenance-message"><a href="#maintenance-message"><code>MAINTENANCE_MESSAGE</code></a></dt>
    <dd>
        <p>Define a custom maintenance message.</p>
        <p><em>Default is "Miniflux is currently under maintenance".</em></p>
    </dd>
    <dt id="maintenance-mode"><a href="#maintenance-mode"><code>MAINTENANCE_MODE</code></a></dt>
    <dd>
        <p>Set to 1 to enable maintenance mode.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="media-proxy-custom-url"><a href="#media-proxy-custom-url"><code>MEDIA_PROXY_CUSTOM_URL</code></a></dt>
    <dd>
        <p>Sets an external server to proxy media through.</p>
        <p>Default is empty, Miniflux does the proxying.</p>
    </dd>
    <dt id="media-proxy-http-client-timeout"><a href="#media-proxy-http-client-timeout"><code>MEDIA_PROXY_HTTP_CLIENT_TIMEOUT</code></a></dt>
    <dd>
        <p>Time limit in seconds before the media proxy HTTP client cancels the request.</p>
        <p><em>Default is 120 seconds.</em></p>
    </dd>
    <dt id="media-proxy-allow-private-networks"><a href="#media-proxy-allow-private-networks"><code>MEDIA_PROXY_ALLOW_PRIVATE_NETWORKS</code></a></dt>
    <dd>
        <p>Set to 1 to allow proxying media that resolves to private or loopback networks.</p>
        <p><em>Disabled by default, private networks are refused.</em></p>
    </dd>
    <dt id="media-proxy-resource-types"><a href="#media-proxy-resource-types"><code>MEDIA_PROXY_RESOURCE_TYPES</code></a></dt>
    <dd>
        <p>A comma-separated list of media types to proxify. Supported values are: <code>image</code>, <code>audio</code>, <code>video</code>.</p>
        <p><em>Default is <code>image</code>.</em></p>
    </dd>
    <dt id="media-proxy-mode"><a href="#media-proxy-mode"><code>MEDIA_PROXY_MODE</code></a></dt>
    <dd>
        <p>Possible values: <code>http-only</code>, <code>all</code>, or <code>none</code>.</p>
        <p><em>Default is <code>http-only</code>.</em></p>
    </dd>
    <dt id="media-proxy-private-key"><a href="#media-proxy-private-key"><code>MEDIA_PROXY_PRIVATE_KEY</code></a></dt>
    <dd>
        <p>Set a custom private key used to sign proxified media URLs.</p>
        <p>By default, a secret key is randomly generated during startup.</p>
    </dd>
    <dt id="metrics-allowed-networks"><a href="#metrics-allowed-networks"><code>METRICS_ALLOWED_NETWORKS</code></a></dt>
    <dd>
        <p>List of networks allowed to access the <code>/metrics</code> endpoint (comma-separated values).</p>
        <p><em>Default is <code>127.0.0.1/8</code>.</em></p>
    </dd>
    <dt id="metrics-collector"><a href="#metrics-collector"><code>METRICS_COLLECTOR</code></a></dt>
    <dd>
        <p>Set to 1 to enable metrics collector. Expose a <code>/metrics</code> endpoint for Prometheus.</p>
        <p><em>Disabled by default.</em></p>
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
    <dt id="metrics-refresh-interval"><a href="#metrics-refresh-interval"><code>METRICS_REFRESH_INTERVAL</code></a></dt>
    <dd>
        <p>Refresh interval in seconds to collect database metrics.</p>
        <p><em>Default is 60 seconds.</em></p>
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
    <dt id="oauth2-oidc-discovery-endpoint"><a href="#oauth2-oidc-discovery-endpoint"><code>OAUTH2_OIDC_DISCOVERY_ENDPOINT</code></a></dt>
    <dd>
        <p>OpenID Connect discovery endpoint.</p>
        <p>Note that the OIDC library automatically appends the <code>.well-known/openid-configuration</code>, this part has to be removed from the URL when setting <code>OAUTH2_OIDC_DISCOVERY_ENDPOINT</code>.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-oidc-provider-name"><a href="#oauth2-oidc-provider-name"><code>OAUTH2_OIDC_PROVIDER_NAME</code></a></dt>
    <dd>
        <p>Name to display for the OIDC provider.</p>
        <p><em>Default is "OpenID Connect".</em></p>
    </dd>
    <dt id="oauth2-provider"><a href="#oauth2-provider"><code>OAUTH2_PROVIDER</code></a></dt>
    <dd>
        <p>OAuth2 provider. Possible values are <code>google</code> or <code>oidc</code> for a generic OpenID Connect provider.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-redirect-url"><a href="#oauth2-redirect-url"><code>OAUTH2_REDIRECT_URL</code></a></dt>
    <dd>
        <p>OAuth2 redirect URL.</p>
        <p>This URL must be registered with the provider and is something like <code>https://miniflux.example.org/oauth2/oidc/callback</code>.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="oauth2-user-creation"><a href="#oauth2-user-creation"><code>OAUTH2_USER_CREATION</code></a></dt>
    <dd>
        <p>Set to 1 to authorize OAuth2 user creation.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="polling-frequency"><a href="#polling-frequency"><code>POLLING_FREQUENCY</code></a></dt>
    <dd>
        <p>Interval for the background job scheduler.</p>
        <p>Determines how often a batch of feeds is selected for refresh, based on their last refresh time.</p>
        <p><em>Default is 60 minutes.</em></p>
    </dd>
    <dt id="polling-limit-per-host"><a href="#polling-limit-per-host"><code>POLLING_LIMIT_PER_HOST</code></a></dt>
    <dd>
        <p>Limits the number of concurrent requests to the same hostname when polling feeds.</p>
        <p>This helps prevent overwhelming a single server during batch processing by the worker pool.</p>
        <p><em>Default is 0 (disabled).</em></p>
    </dd>
    <dt id="polling-parsing-error-limit"><a href="#polling-parsing-error-limit"><code>POLLING_PARSING_ERROR_LIMIT</code></a></dt>
    <dd>
        <p>
            The maximum number of parsing errors that the program will try before stopping polling a feed.
            Once the limit is reached, the user must refresh the feed manually. Set to 0 for unlimited.
        </p>
        <p><em>Default is 3.</em></p>
    </dd>
    <dt id="polling-scheduler"><a href="#polling-scheduler"><code>POLLING_SCHEDULER</code></a></dt>
    <dd>
        <p>
            Determines the strategy used to schedule feed polling.
            Supported values are <code>round_robin</code> and <code>entry_frequency</code>.
        </p>
        <p>
            - <code>round_robin</code>: Feeds are polled in a fixed, rotating order.
        </p>
        <p>
            - <code>entry_frequency</code>: The polling interval for each feed is based on the average update frequency over the past week.
        </p>
        <p>
            The number of feeds polled in a given period is limited by the <code>POLLING_FREQUENCY</code> and <code>BATCH_SIZE</code> settings.
            Regardless of the scheduler used, the total number of polled feeds will not exceed the maximum allowed per polling cycle.
        </p>
        <p><em>Default is <code>round_robin</code>.</em></p>
    </dd>
    <dt id="port"><a href="#port"><code>PORT</code></a></dt>
    <dd>
        <p>Override <code>LISTEN_ADDR</code> to <code>0.0.0.0:$PORT</code> (Automatic configuration for <abbr title="Platform as a service">PaaS</abbr>).</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="run-migrations"><a href="#run-migrations"><code>RUN_MIGRATIONS</code></a></dt>
    <dd>
        <p>Set to 1 to run database migrations.</p>
        <p><em>Disabled by default.</em></p>
    </dd>
    <dt id="scheduler-entry-frequency-factor"><a href="#scheduler-entry-frequency-factor"><code>SCHEDULER_ENTRY_FREQUENCY_FACTOR</code></a></dt>
    <dd>
        <p>Factor to increase refresh frequency for the entry frequency scheduler.</p>
        <p><em>Default is 1.</em></p>
    </dd>
    <dt id="scheduler-entry-frequency-max-interval"><a href="#scheduler-entry-frequency-max-interval"><code>SCHEDULER_ENTRY_FREQUENCY_MAX_INTERVAL</code></a></dt>
    <dd>
        <p>Maximum interval in minutes for the entry frequency scheduler.</p>
        <p><em>Default is 1440 minutes (24 hours).</em></p>
    </dd>
    <dt id="scheduler-entry-frequency-min-interval"><a href="#scheduler-entry-frequency-min-interval"><code>SCHEDULER_ENTRY_FREQUENCY_MIN_INTERVAL</code></a></dt>
    <dd>
        <p>Minimum interval in minutes for the entry frequency scheduler.</p>
        <p><em>Default is 5 minutes.</em></p>
    </dd>
    <dt id="scheduler-round-robin-max-interval"><a href="#scheduler-round-robin-max-interval"><code>SCHEDULER_ROUND_ROBIN_MAX_INTERVAL</code></a></dt>
    <dd>
        <p>
            Maximum interval in minutes for the round robin scheduler.
            This option is used when the values of the <code>Cache-Control</code> max-age and <code>Expires</code> headers are excessively high.
        </p>
        <p><em>Default is 1440 minutes (24 hours).</em></p>
    </dd>
    <dt id="trusted-reverse-proxy-networks"><a href="#trusted-reverse-proxy-networks"><code>TRUSTED_REVERSE_PROXY_NETWORKS</code></a></dt>
    <dd>
        <p>List of networks (CIDR notation) allowed to use the proxy authentication header, <code>X-Forwarded-For</code>, <code>X-Forwarded-Proto</code>, and <code>X-Real-Ip</code> headers.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="scheduler-round-robin-min-interval"><a href="#scheduler-round-robin-min-interval"><code>SCHEDULER_ROUND_ROBIN_MIN_INTERVAL</code></a></dt>
    <dd>
        <p>Minimum interval in minutes for the round robin scheduler.</p>
        <p><em>Default is 60 minutes.</em></p>
    </dd>
    <dt id="watchdog"><a href="#watchdog"><code>WATCHDOG</code></a></dt>
    <dd>
        <p>Enable or disable Systemd watchdog.</p>
        <p><em>Enabled by default.</em></p>
    </dd>
    <dt id="webauthn"><a href="#webauthn"><code>WEBAUTHN</code></a></dt>
    <dd>
        <p>Enable or disable WebAuthn/Passkey authentication.</p>
        <p>
            You must provide a username on the login page if you are using non-residential keys. However, this is not required for discoverable credentials.
        </p>
        <p><em>Default is disabled.</em></p>
    </dd>
    <dt id="worker-pool-size"><a href="#worker-pool-size"><code>WORKER_POOL_SIZE</code></a></dt>
    <dd>
        <p>Number of background workers.</p>
        <p><em>Default is 16 workers.</em></p>
    </dd>
    <dt id="youtube-api-key"><a href="#youtube-api-key"><code>YOUTUBE_API_KEY</code></a></dt>
    <dd>
        <p>YouTube API key for use with <code>FETCH_YOUTUBE_WATCH_TIME</code>. If nonempty, the duration will be fetched from the YouTube API. Otherwise, the duration will be fetched from the YouTube website.</p>
        <p><em>Default is empty.</em></p>
    </dd>
    <dt id="youtube-embed-url-override"><a href="#youtube-embed-url-override"><code>YOUTUBE_EMBED_URL_OVERRIDE</code></a></dt>
    <dd>
        <p>YouTube URL which will be used for embeds.</p>
        <p><em>Default is <code>https://www.youtube-nocookie.com/embed/</code>.</em></p>
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
LOG_LEVEL=debug
WORKER_POOL_SIZE=20
```

Environment variables override the values defined in the config file.

To specify a configuration file, use the command line argument `-config-file` or `-c`.

You can also dump interpreted values with the flag `-config-dump` for debugging.

<p class="info">
Systemd uses the file <code>/etc/miniflux.conf</code> to populate environment variables.
</p>
