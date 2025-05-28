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