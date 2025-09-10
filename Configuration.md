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

## Setting Fail2Ban

> [!NOTE]
> IP Limit won't work correctly when using IP Tunnel.

### **For versions up to `v1.6.1`:**

The IP limit is built-in to the panel

### **For versions `v1.7.0` and newer:**

To enable the IP Limit functionality, you need to install `fail2ban` and its required files by following these steps:

1. Run the `x-ui` command in the terminal, then choose `IP Limit Management`.
2. You will see the following options:

   - **Change Ban Duration:** Adjust the duration of bans.
   - **Unban Everyone:** Lift all current bans.
   - **Check Logs:** Review the logs.
   - **Fail2ban Status:** Check the status of `fail2ban`.
   - **Restart Fail2ban:** Restart the `fail2ban` service.
   - **Uninstall Fail2ban:** Uninstall Fail2ban with configuration.

3. Add a path for the access log on the panel by setting `Xray Configs/log/Access log` to `./access.log` then save and restart xray.

- **For versions before `v2.1.3`:**
  - You need to set the access log path manually in your Xray configuration:

    ```sh
    "log": {
      "access": "./access.log",
      "dnsLog": false,
      "loglevel": "warning"
    },
    ```

- **For versions `v2.1.3` and newer:**
  - There is an option for configuring `access.log` directly from the panel.

---

# API Documentation

## Authentication

* **Endpoint**: `/login`
* **Method**: `POST`
* **Body**:

  ```json
  {
    "username": "",
    "password": ""
  }
  ```
* Use this endpoint to authenticate and receive a session.

---

## Inbounds API

Base path: **`/panel/api/inbounds`**

| Method | Path                             | Description                                  |
| :----: | -------------------------------- | -------------------------------------------- |
|  `GET` | `/list`                          | Get all inbounds                             |
|  `GET` | `/get/:id`                       | Get inbound by ID                            |
|  `GET` | `/getClientTraffics/:email`      | Get client traffics by email                 |
|  `GET` | `/getClientTrafficsById/:id`     | Get client traffics by inbound ID            |
| `POST` | `/add`                           | Add new inbound                              |
| `POST` | `/del/:id`                       | Delete inbound by ID                         |
| `POST` | `/update/:id`                    | Update inbound by ID                         |
| `POST` | `/clientIps/:email`              | Get client IP addresses                      |
| `POST` | `/clearClientIps/:email`         | Clear client IP addresses                    |
| `POST` | `/addClient`                     | Add client to inbound                        |
| `POST` | `/:id/delClient/:clientId`       | Delete client by clientId\*                  |
| `POST` | `/updateClient/:clientId`        | Update client by clientId\*                  |
| `POST` | `/:id/resetClientTraffic/:email` | Reset a client’s traffic usage               |
| `POST` | `/resetAllTraffics`              | Reset traffics for all inbounds              |
| `POST` | `/resetAllClientTraffics/:id`    | Reset traffics for all clients in inbound    |
| `POST` | `/delDepletedClients/:id`        | Delete depleted clients in inbound (-1: all) |
| `POST` | `/import`                        | Import inbound configuration                 |
| `POST` | `/onlines`                       | Get currently online clients (emails list)   |
| `POST` | `/lastOnline`                    | Get last online status of clients            |
| `POST` | `/updateClientTraffic/:email`    | Update traffic for specific client           |

\* **clientId mapping**:

* `client.id` → VMESS / VLESS
* `client.password` → TROJAN
* `client.email` → Shadowsocks

---

## Server API

Base path: **`/panel/api/server`**

| Method | Path                       | Description                                   |
| :----: | -------------------------- | --------------------------------------------- |
|  `GET` | `/status`                  | Get server status                             |
|  `GET` | `/getXrayVersion`          | Get available Xray versions                   |
|  `GET` | `/getConfigJson`           | Download current config.json                  |
|  `GET` | `/getDb`                   | Download database file (`x-ui.db`)            |
|  `GET` | `/getNewUUID`              | Generate new UUID                             |
|  `GET` | `/getNewX25519Cert`        | Generate new X25519 certificate               |
|  `GET` | `/getNewmldsa65`           | Generate new ML-DSA-65 certificate            |
|  `GET` | `/getNewmlkem768`          | Generate new ML-KEM-768 key pair              |
|  `GET` | `/getNewVlessEnc`          | Generate new VLESS encryption keys            |
| `POST` | `/stopXrayService`         | Stop Xray service                             |
| `POST` | `/restartXrayService`      | Restart Xray service                          |
| `POST` | `/installXray/:version`    | Install/Update Xray to given version          |
| `POST` | `/updateGeofile`           | Update GeoIP/GeoSite data files               |
| `POST` | `/updateGeofile/:fileName` | Update specific Geo file                      |
| `POST` | `/logs/:count`             | Get system logs (with `level`, `syslog`)      |
| `POST` | `/xraylogs/:count`         | Get Xray logs (with filters)                  |
| `POST` | `/importDB`                | Import database                               |
| `POST` | `/getNewEchCert`           | Generate new ECH certificate (requires `sni`) |

---

## Extra API

Base path: **`/panel/api`**

| Method | Path             | Description                               |
| :----: | ---------------- | ----------------------------------------- |
|  `GET` | `/backuptotgbot` | Backup DB/config and send to Telegram Bot |

---

[<img src="https://run.pstmn.io/button.svg" alt="Run In Postman" style="width: 128px; height: 32px;">](https://app.getpostman.com/run-collection/5146551-dda3cab3-0e33-485f-96f9-d4262f437ac5?action=collection%2Ffork&source=rip_markdown&collection-url=entityId%3D5146551-dda3cab3-0e33-485f-96f9-d4262f437ac5%26entityType%3Dcollection%26workspaceId%3Dd64f609f-485a-4951-9b8f-876b3f917124)

## Geosites

### What is it?

The Geosites in Xray-core play a key role in traffic routing, enabling flexible control over traffic distribution based on the geographical location of IP addresses and domains. Here are their main files:

* `geoip.dat` contains a database of IP addresses classified by country (e.g., `geoip:cn` for China or `geoip:private` for private networks). This allows:

  * Redirecting traffic for specific countries (e.g., Chinese IPs via direct, others via proxy).

  * Blocking or allowing access to IPs from certain regions.

* `geosite.dat` includes domain lists grouped by categories (e.g., `geosite:cn` for Chinese domains, `geosite:google` for Google services). This is used for:

  * Granular traffic control (e.g., ad domains → block, streaming → proxy).

### Sources

3X-UI uses multiple geofiles sources for flexible traffic routing:

| Repository | Files | Available geosites |
| - | - | - |
| [Loyalsoldier/v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat) | `geoip.dat` <br /> `geosite.dat` | [View](https://github.com/v2fly/domain-list-community/tree/master/data) |
| [🇮🇷 chocolate4u/Iran-v2ray-rules](https://github.com/chocolate4u/Iran-v2ray-rules) | `geoip_IR.dat` <br /> `geosite_IR.dat` | [View](https://github.com/chocolate4u/Iran-v2ray-rules?tab=readme-ov-file#page_with_curl-categories) |
| [🇷🇺 runetfreedom/russia-v2ray-rules-dat](https://github.com/runetfreedom/russia-v2ray-rules-dat) | `geoip_RU.dat` <br /> `geosite_RU.dat` | [View](https://github.com/runetfreedom/russia-v2ray-rules-dat?tab=readme-ov-file#%D0%BA%D0%B0%D0%BA%D0%B8%D0%B5-%D0%BA%D0%B0%D1%82%D0%B5%D0%B3%D0%BE%D1%80%D0%B8%D0%B8-%D1%81%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%81%D1%8F-%D0%B2-%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%D1%85) |

### Updating geofiles

1. Open panel and click to Xray version

![image](https://github.com/user-attachments/assets/1f86d21b-c1a7-4268-9031-1cb5179dc38d)

2. Open `Geofiles` dropdown and update the needed geofile

![image](https://github.com/user-attachments/assets/6765b54d-3858-4fd3-b75b-17b5bc2b983f)