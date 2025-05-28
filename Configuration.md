## Getting SSL

### ACME

To manage SSL certificates using ACME:

1. Ensure your domain is correctly resolved to the server.
2. Run the `x-ui` command in the terminal, then choose `SSL Certificate Management`.
3. You will be presented with the following options:

   - **Get SSL:** Obtain SSL certificates.
   - **Revoke:** Revoke existing SSL certificates.
   - **Force Renew:** Force renewal of SSL certificates.
   - **Show Existing Domains:** Display all domain certificates available on the server.  
   - **Set Certificate Paths for the Panel:** Specify the certificate for your domain to be used by the panel. 

### Certbot

To install and use Certbot:

```sh
apt-get install certbot -y
certbot certonly --standalone --agree-tos --register-unsafely-without-email -d yourdomain.com
certbot renew --dry-run
```

### Cloudflare

The management script includes a built-in SSL certificate application for Cloudflare. To use this script to apply for a certificate, you need the following:

- Cloudflare registered email
- Cloudflare Global API Key
- The domain name must be resolved to the current server through Cloudflare

**How to get the Cloudflare Global API Key:**

1. Run the `x-ui` command in the terminal, then choose `Cloudflare SSL Certificate`.
2. Visit the link: [Cloudflare API Tokens](https://dash.cloudflare.com/profile/api-tokens).
3. Click on "View Global API Key" (see the screenshot below):

![](https://github.com/MHSanaei/3x-ui/raw/main/media/APIKey1.PNG)

4. You may need to re-authenticate your account. After that, the API Key will be shown (see the screenshot below):

![](https://github.com/MHSanaei/3x-ui/raw/main/media/APIKey2.png)

When using, just enter your `domain name`, `email`, and `API KEY`. The diagram is as follows:

![](https://github.com/MHSanaei/3x-ui/raw/main/media/DetailEnter.png)

## Available environment variables

### `XUI_LOG_LEVEL`
* **Description**: Default log level
* **Type**: `string`
* **Acceptable values**: `debug` | `info` | `warn` | `error`
* **Default value**: `info`

### `XUI_DEBUG`
* **Description**: Whether debug mode should be enabled
* **Type**: `boolean`
* **Default value**: `false`

### `XUI_BIN_FOLDER`
* **Description**: Path to the folder with xray-core, geosite & geoip databases
* **Type**: `string`
* **Default value**: `bin`

### `XUI_DB_FOLDER`
* **Description**: Path to the 3x-ui database
* **Type**: `string`
* **Default value**: `/etc/x-ui`

### `XUI_LOG_FOLDER`
* **Description**: Path to the logs
* **Type**: `string`
* **Default value**: `/var/log`

### `XUI_ENABLE_FAIL2BAN`
* **Description**: Should [fail2ban](https://github.com/fail2ban/fail2ban) be working
* **Type**: `boolean`
* **Default value**: `true`

## Reverse Proxy

### Nginx

To configure the reverse proxy, add the following paths to your nginx config

```nginx
location / {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Range $http_range;
    proxy_set_header If-Range $http_if_range; 
    proxy_redirect off;
    proxy_pass http://127.0.0.1:2053;
}
```

> [!NOTE]
> The URL in the panel settings needs to end with /.

For the subscriptions

```nginx
location /sub {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Range $http_range;
    proxy_set_header If-Range $http_if_range; 
    proxy_redirect off;
    proxy_pass http://127.0.0.1:2053;
}
```

> [!NOTE]
> Ensure that the "URI Path" in the /sub panel settings is the same.

### Caddy

> [!IMPORTANT]
> A huge thanks to [@Gill-Bates](https://github.com/Gill-Bates) for providing the config

> [!IMPORTANT]
> This configuration will work when the “WebSocket” transport is set inbound

Before configuring `caddyfile`, make sure that the following parameters are set in the panel setup

![Screenshot](https://github.com/user-attachments/assets/c1914df0-ec9f-47ad-8f56-fb872be75fa0)

After customizing the panel, modify the caddyfile as follows

```caddyfile
vpn.example.com {

    encode gzip

    # TLS 1.3 mandatory!
    tls {
        protocols tls1.3
    }

    # Protect your GUI with Basic Auth
    route /admin* {
        basic_auth {
            admin ******
        }
        reverse_proxy xx.xx.xx.xx:2053
    }

    # Obfuscate the Endpoint
    route /api/v1* {
        @websockets {
            header Connection *Upgrade*
            header Upgrade websocket
        }
        reverse_proxy @websockets xx.xx.xx.xx:54321
        respond "Forbidden" 403
    }

    # Security Header
    header {
        header_up Authorization { >Authorization }
        header_up Content-Type { >Content-Type }
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        X-Content-Type-Options nosniff
        X-Frame-Options SAMEORIGIN
        Referrer-Policy strict-origin-when-cross-origin
        -Server
        -X-Powered-By
    }

    # Fallback
    respond "Not found!" 404
}
```

The following data must be replaced in the config:

* `vpn.example.com` -> your domain.
* `admin *****` -> replace the asterisks with your password.

If you do not need HTTP Auth, remove the following line

```caddyfile
basic_auth {
    admin ******
}
```

* `reverse_proxy xx.xx.xx.xx` -> replace the `xx.xx.xx.xx` with your IP
* `reverse_proxy @websockets xx.xx.xx.xx:54321` -> replace `54321` with your inbound port
