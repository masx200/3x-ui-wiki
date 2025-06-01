## Setting Telegram bot

#### Usage

The web panel supports daily traffic, panel login, database backup, system status, client info, and other notification and functions through the Telegram Bot. 

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/6e918166-49c4-4bc6-9251-a330c1fdd796">
  <img alt="3x-ui" src="https://github.com/user-attachments/assets/5d7c7d06-97b3-46bc-b314-375eea4bfe7b">
</picture>

To use the bot, you need to set the bot-related parameters in the panel, including:

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

![Botfather](https://github.com/user-attachments/assets/b2d11d8f-62db-474a-8a3b-42b57e873dc6)

- Create a new Bot using /newbot command: It will ask you 2 questions, A name and a username for your bot. Note that the username has to end with the word "bot".

![Create new bot](https://github.com/user-attachments/assets/c1a49368-3747-44e7-ae72-0d78b192c7e3)

- Start the bot you've just created. You can find the link to your bot here.

![token](https://github.com/MHSanaei/3x-ui/raw/main/media/token.png)

- Enter your panel and config Telegram bot settings like below:

![Panel Config](https://github.com/user-attachments/assets/5a79e1d9-52e9-412f-93b2-506e23b4da60)

Enter your bot token in input field number 3.
Enter the user ID in input field number 4. The Telegram accounts with this id will be the bot admin. (You can enter more than one, Just separate them with ,)

#### How to get Telegram user ID?
 
Use this [bot](https://t.me/useridinfobot), Start the bot and it will give you the Telegram user ID.

![User ID Info bot](https://github.com/user-attachments/assets/93fccdca-c3e1-4fb2-8e4e-16aa7fb009aa)

## Setting Cloudflare WARP

### What is it?

Cloudflare WARP is a free service developed by Cloudflare that enhances internet security and performance by masking traffic between your server and the internet. WARP is required for the following cases:
- When you need to hide the IP address of a server.
- When some services do not open.

### Setup

1. Open the panel and go to `Xray Setting`.
2. Open `Outbounds` tab, click `WARP` and `Create` button.

![image](https://github.com/user-attachments/assets/6fdeee5a-f0f4-424b-99e5-68c29bc01611)

We have created a WARP profile, but not set `WARP` outbound. To create an outbound to which we will redirect traffic, click `Add outbound` and close the dialog box

### Redirect trafic to WARP

1. Open the panel and go to `Xray Setting`.
2. Open `Routing Rules` and click `Add Rule`.
3. Add the following rule

![image](https://github.com/user-attachments/assets/3570af28-d958-449d-bf0b-0122ece82851)

In this rule, we redirect all traffic from Reddit and Google (and all its services) to WARP outbound

## Setting TOR proxy

### What is it?

Tor, short for The Onion Router, is a free and open-source software designed to enable anonymous communication over the internet. It achieves this by routing your internet traffic through a global network of volunteer-operated servers, known as relays or nodes, thereby concealing your location and usage from surveillance and traffic analysis.

### Setup

> [!TIP]
> Before you start, make sure you have Docker installed. If it is not installed, see [here](https://github.com/MHSanaei/3x-ui/wiki/Installation#docker-recommended) for the installation script

1. Open terminal and run this command

```bash
$ docker run --restart unless-stopped -i -t -p 9050:9050 dperson/torproxy
```

This will start a SOCKS5 proxy with TOR, now it needs to be configured in the panel

2. Go to panel and open `Xray Settings`
3. Go to `Outbound` tab and click `Add Outbound`
4. Fill out the form as follows and click `Add Outbound`

![Outbound Modal](https://github.com/user-attachments/assets/ef36ec1c-ee6c-4661-85a4-a66835ccc4a7)

### Redirect traffic

Same [thing](https://github.com/MHSanaei/3x-ui/wiki/Advanced#redirect-trafic-to-warp) as with WARP, but instead of `warp` outbound, we use `tor`. 
