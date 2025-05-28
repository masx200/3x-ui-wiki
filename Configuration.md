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