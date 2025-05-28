## Setting Telegram bot

#### Usage

The web panel supports daily traffic, panel login, database backup, system status, client info, and other notification and functions through the Telegram Bot. To use the bot, you need to set the bot-related parameters in the panel, including:

- Telegram Token
- Admin Chat ID(s)
- Notification Time (in cron syntax)
- Expiration Date Notification
- Traffic Cap Notification
- Database Backup
- CPU Load Notification


**Reference syntax:**

- `30 \* \* \* \* \*` - Notify at the 30s of each point
- `0 \*/10 \* \* \* \*` - Notify at the first second of each 10 minutes
- `@hourly` - Hourly notification
- `@daily` - Daily notification (00:00 in the morning)
- `@weekly` - weekly notification
- `@every 8h` - Notify every 8 hours

### Telegram Bot Features

- Report periodic
- Login notification
- CPU threshold notification
- Threshold for Expiration time and Traffic to report in advance
- Support client report menu if client's telegram username added to the user's configurations
- Support telegram traffic report searched with UUID (VMESS/VLESS) or Password (TROJAN) - anonymously
- Menu-based bot
- Search client by email (only admin)
- Check all inbounds
- Check server status
- Check depleted users
- Receive backup by request and in periodic reports
- Multi-language bot

### Setting up Telegram bot

- Start [Botfather](https://t.me/BotFather) in your Telegram account:

![Botfather](https://github.com/MHSanaei/3x-ui/raw/main/media/botfather.png)

- Create a new Bot using /newbot command: It will ask you 2 questions, A name and a username for your bot. Note that the username has to end with the word "bot".

![Create new bot](https://github.com/MHSanaei/3x-ui/raw/main/media/newbot.png)

- Start the bot you've just created. You can find the link to your bot here.

![token](https://github.com/MHSanaei/3x-ui/raw/main/media/token.png)

- Enter your panel and config Telegram bot settings like below:

![Panel Config](https://github.com/MHSanaei/3x-ui/raw/main/media/panel-bot-config.png)

Enter your bot token in input field number 3.
Enter the user ID in input field number 4. The Telegram accounts with this id will be the bot admin. (You can enter more than one, Just separate them with ,)

#### How to get Telegram user ID?
 
Use this [bot](https://t.me/useridinfobot), Start the bot and it will give you the Telegram user ID.

![User ID](https://github.com/MHSanaei/3x-ui/raw/main/media/user-id.png)