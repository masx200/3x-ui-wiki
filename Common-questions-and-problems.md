## 1.  Server Update

Usually, the first step on a Linux server is to update and upgrade the server.

One-line server update command:

```sh 
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
```

**Fixing Server Update Errors:**

Sometimes you can solve update problems by changing the sources links.

```sh
nano /etc/apt/sources.list
```

Get the appropriate repository from the links below.

[Ubuntu 20.04](https://gist.github.com/ishad0w/788555191c7037e249a439542c53e170) | [Ubuntu 22.04](https://gist.github.com/hakerdefo/9c99e140f543b5089e32176fe8721f5f)

If your connection is interrupted during the update, and you receive an error when updating again, the following commands will solve the problem:

```sh
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock*

dpkg --configure -a

sudo dpkg --configure -a
```

After executing these commands, perform the update and upgrade again.

## 2. Changing Default SSH Port

The default SSH port is 22. Changing it can enhance server security by reducing automated attacks and unauthorized access attempts. Here's how to change it:

1. Edit the SSH configuration file:
```sh
sudo nano /etc/ssh/sshd_config
```

2. Find the line that says `#Port 22` and change it to your desired port number (e.g., `Port 2222`). Make sure to remove the # symbol.

3. Save the file (Ctrl+X, then Y, then Enter)

4. Restart the SSH service:
```sh
sudo systemctl restart sshd
```

5. Before closing your current SSH session, test the new port in a new terminal window to ensure it works:
```sh
ssh -p 2222 username@your_server_ip
```

6. If the new port works, update your firewall to allow the new port:
```sh
sudo ufw allow 2222/tcp
```

7. Optionally, you can remove the old port from the firewall:
```sh
sudo ufw deny 22/tcp
```

Important notes:
- Choose a port number between 1024 and 65535
- Remember to update your SSH client configurations to use the new port
- Keep the new port number secure and don't share it publicly
- Make sure to test the new connection before closing your current session

## 3. Root Access to Server

For some servers, login is done with a non-root user.
You can change the user to root with the command `sudo -i` or `sudo su`.

You can also allow root user login with the following command and log in directly as root:

```sh
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && sudo systemctl restart ssh && sudo passwd
```

## 4. Changing DNS Server

Changing the DNS server can improve server performance and bypass some restrictions.
[DNS Server Configuration Tutorial](https://github.com/hiddify/hiddify-config/wiki/%D8%A2%D9%85%D9%88%D8%B2%D8%B4-%D8%AA%D9%86%D8%B8%DB%8C%D9%85-DNS-%D8%B3%D8%B1%D9%88%D8%B1#%D8%AA%D9%86%D8%B8%DB%8C%D9%85-dns-%D8%A7%D8%B2-%D8%B7%D8%B1%DB%8C%D9%82-ssh)

a) **Recommended DNS**

```
nameserver 1.1.1.1
nameserver 1.0.0.1
```

For Iranian servers, you need to change DNS to Shecan to install sanctioned tools.

```
nameserver 178.22.122.100
nameserver 185.51.200.2
```

## 5. Enabling Server Firewall

Enabling the firewall is useful and effective for server security and preventing blocking. Also, in some servers, the firewall is open by default, and you need to enable the ports you need.

First, enable the ports you need in the firewall with the following command. These ports can include SSH port, panel port, config ports, etc.

```sh
sudo ufw allow <port>/tcp
```

Then turn on the firewall with the following command:

```sh
sudo ufw enable
```

Check your firewall status with the command:
```sh
sudo ufw status`
```

You can also easily enable the server firewall using Firewall Management in the x-ui menu.

## 6. What settings do you recommend for Reality?

According to [xray developer](https://github.com/XTLS/Xray-core/issues/1769#issuecomment-1464821647) and experts, TCP REALITY VISION combination is the best choice. Of course, you can use GRPC and H2 which have lower ping, but usually TCP has been more successful in speed tests. Additionally, in [this tutorial](https://telegra.ph/RealityTCPHeaderVless-05-29), REALITY with HTTP Camouflage and several SNIs are introduced. It is recommended to use [RealiTLScanner](https://github.com/XTLS/RealiTLScanner) to find SNIs with TLS 1.3 H2 characteristics.

It is strongly recommended to put all configs on one inbound or port.

## 7. What should I do if someone wants to log into my panel?

If you notice through the bot or server logs that your panel is being brute-forced, you have several solutions.

1. Change panel port and path.
2. Block the attacker's IP. If only one IP is trying to access your panel, you can block their IP with the following command if the firewall is active:

```sh
sudo ufw deny from <ip-address> to any
```

3. Limit panel access to your IP. By not leaving the panel port open in the firewall, you can only log in with the config and IP of the same server. Or if you have another IP, you can allow it with the following command if the firewall is active:

```sh
sudo ufw allow from <ip-address> to any port <panel-port> proto tcp
```

Enabling HTTPS for the panel is also an [important](https://t.me/projectXtls/165) security point.

## 8. How do I disable IP table tunneling?

```sh
sysctl net.ipv4.ip_forward=0
```

First, execute the above command, then replace A- with D- in all commands you used to enable IP table tunneling and execute the commands.

## 9. How do I specify that the bot should send backups at a certain time?

It's explained [here](https://github.com/MHSanaei/3x-ui#telegram-bot), and to specify precisely, you can use [this](https://crontab.guru/examples.html) website.

## 10. Does IP limit work? How do I use it in tunneling?

Yes; from version 1.7.0+, you can use it by installing fail2ban in the x-ui menu and specifying the limit for each client. If a user connects with an unauthorized number of IPs, they will be blocked for the time you specified. From version 1.7.9+, you need to enable access log in xray settings as follows:

```json
"log": {
    "access": "./access.log",
    "dnsLog": false,
    "error": "./error.log",
    "loglevel": "warning"
},
```

Or in version 2.1.3+, set Xray Configs -> Basics -> General -> Access Log to access.log/.

You cannot use IP limit for direct tunneling because only your Iranian server's IP reaches the foreign server. A simple solution is [panel-to-panel](https://github.com/rahgozar94725/freedom/blob/main/domestic-server/p-to-p.md) [outbound](https://t.me/panel3xui/65) tunneling and using IP limit on the Iranian server.

IP limit doesn't work by default for CDN configs because the IPs of CDN edge servers reach our server. To send the user's IP to xray, you can create a reverse proxy using nginx by setting the X-Forwarded-For header for WS and X-Real-IP for GRPC, and enable acceptProxyProtocol in your inbound settings.

## 11. Some sites don't open for me. What is WARP? How do I enable it? How do I route all traffic through WARP?

WARP routes your server's traffic through Cloudflare's network using WireGuard, changing your server's IP to a Cloudflare IP. Since WARP IPs are whitelisted in most services, you can access sites that give forbidden or 403 errors. However, your server's IP may not be fixed.

You can enable WARP this way:
```
Xray Configs -> Outbounds -> Create -> Add Outbound
```

In the Basics column, check platforms like Google and Spotify

Or add your desired site this way:
```
Xray Configs -> Routing Rules -> Add Rule
```

Select warp as Outbound Tag and add your domains to Domain as follows:

```
bing.com,yahoo.com,geosite:instagram,geosite:meta
```

You can view existing geosites from [this link](https://github.com/v2fly/domain-list-community/tree/master/data).

**To route all traffic through WARP**, you can modify the WARP route code in the Advanced column as follows:

```json
{
    "type": "field",
    "outboundTag": "warp",
    "network": "tcp,udp"
},
```

Additionally, if you encounter a 403 error in Google, installing WARP is not mandatory, and you can solve the problem by enabling the Use IPv4 for Google option in xray panel settings.

For Spotify, sometimes besides WARP, you also need to enable Fake DNS on the client side.

## 12. What should I do with Hetzner abuse?

Receiving abuse from datacenters is not related to the panel.

Routing all traffic through [WARP](https://github.com/MHSanaei/3x-ui/wiki/Common-questions-and-problems#11---%D8%A8%D8%B9%D8%B6%DB%8C-%D8%B3%D8%A7%DB%8C%D8%AA%D8%A7-%D8%A8%D8%B1%D8%A7%D9%85-%D8%A8%D8%A7%D8%B2-%D9%86%D9%85%DB%8C%D8%B4%D9%87-%D9%88%D8%A7%D8%B1%D9%BE-%DA%86%DB%8C%D9%87-%DA%86%D8%AC%D9%88%D8%B1%DB%8C-%D9%81%D8%B9%D8%A7%D9%84%D8%B4-%DA%A9%D9%86%D9%85-%DA%86%D8%AC%D9%88%D8%B1%DB%8C-%DA%A9%D9%84-%D8%AA%D8%B1%D8%A7%D9%81%DB%8C%DA%A9-%D8%B1%D9%88-%D8%A7%D8%B2-%D9%88%D8%A7%D8%B1%D9%BE-%D8%B1%D8%AF-%DA%A9%D9%86%D9%85) can prevent some abuses. Not running and installing unknown scripts is also an effective security measure. Also, make sure BitTorrent and Private IPs are blocked in Xray settings.

Some warnings don't cause your account to be blocked and are just security warnings and recommendations from BSI. To prevent these warnings, enable the Security Shield option in Xray settings. This setting prevents users from accessing malware links.

Finally, if this problem persists, monitor your users.

## 13. How does the panel subscription link work?

The advantage of the subscription link is that if there are changes in inbound settings, you don't need to send a new link to the user, and the user receives the new config by updating from the client side.

To use this section, first enable the Enable Subscription Service option in Subscription settings.
Then you can specify a link for each client.
You can select one link for multiple clients.

In version 1.7.1+, traffic amount and remaining time are added to the config name.

Also, it's recommended to enable HTTPS by selecting your domain's certificate files.

## 14. My server's hard drive is filling up. How do I prevent panel logs from being saved?

If you're not using IPlimit, you can disable access log by modifying the log section in Xray config's Advanced part as follows:

```json
"log": {
    "error": "./error.log",
    "loglevel": "warning"
},
```

Or in version 2.1.3+, set Xray Configs -> Basics -> General -> Access Log to none.

To clear this log file, use the following command:

```sh
echo "" > /usr/local/x-ui/access.log
```

## 15. What should I do to prevent continuous high CPU usage?

First, check which process is causing the CPU increase with the `top` and `htop` commands.

If this problem is from the panel or xray, perform these actions:

- Update the panel.
- Install fail2ban from the x-ui menu.
- Get server resources appropriate for your bandwidth consumption.

## 16. Why is all users' consumption zero and the panel not calculating traffic usage?

This problem usually has three main reasons:

- Error in xray config where you should press the Reset to Default Configuration button in the panel once and save.
- You or the bot you're using may not have entered complete user information like email.
- If you've moved the database and have this problem, update the panel once.
- The api rule must be the first rule in the Routing Rules section.

## 17. How do I solve the "database is locked" error?

Usually, this error is due to your server's weak hard drive. To solve it, [disable access log](https://github.com/MHSanaei/3x-ui/wiki/Common-questions-and-problems#14---%D9%87%D8%A7%D8%B1%D8%AF-%D8%B3%D8%B1%D9%88%D8%B1%D9%85-%D9%BE%D8%B1-%D9%85%DB%8C%D8%B4%D9%87-%DA%86%D8%AC%D9%88%D8%B1%DB%8C-%D8%AC%D9%84%D9%88%DB%8C-%D8%B0%D8%AE%DB%8C%D8%B1%D9%87-%D8%B4%D8%AF%D9%86-%D9%84%D8%A7%DA%AF-%D9%BE%D9%86%D9%84-%D8%B1%D9%88-%D8%A8%DA%AF%DB%8C%D8%B1%D9%85).

## 18. How do Direct Country Configs settings work?

This setting is only suitable for panel-to-panel outbound tunneling, and by enabling it on your Iranian server, you can open internal sites with your Iranian server's IP.

## 19. How do I solve the problem of not downloading from GitHub when installing or updating the panel?

If it stops at `Resolving raw.githubusercontent.com` during GitHub download or you receive the `Connection timed out` error, and this problem only exists for GitHub, it can be solved in two ways.

**First method**: Download the script with IPv4:

```sh
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh --ipv4)
```

**Second method**: You can change GitHub's domain IP as follows:

```sh
sudo nano /etc/hosts
```

Add this line to the end of the file and save:

```
185.199.110.133 raw.githubusercontent.com
```

## 20. Why doesn't the panel search show any results?

Search works fine in the latest updates, but note that no client's email should contain spaces.

## 21. What kind of panel did you make! It keeps giving 400 errors!

If you encounter the `Error: Request failed with status code 400` error, enable SSL for your panel.

If SSL is enabled, perform the actions in [this link](https://www.hostinger.com/tutorials/how-to-fix-400-bad-request-error).

## 22. Why does a user remain online and consume more than specified even after their traffic is used up?

In this panel, traffic is checked every 10 seconds, and if the volume is used up, it gives xray the command to disconnect. If the user is still shown as online, it might be due to the server's hard drive and changes not being saved or being saved late. More importantly, if xray doesn't disconnect the user, it's not an x-ui panel bug because according to [our evaluations](https://github.com/alireza0/x-ui/issues/771#issuecomment-1861822323), when a user is downloading with high bandwidth, it takes about two minutes (this time is not exact) for all user sessions in xray to end. During this time, user consumption is recorded in the panel.

## 23. Why doesn't my panel open after updating or transferring?

This problem might be due to an error in xray or panel SSL issue.

It's usually solved by Reset Settings in the x-ui menu.

## 24. How do I use fragment for my config?

Fragment is easily configurable in the Freedom column of Outbounds for panel-to-panel tunneling.

But for direct config, fragment [cannot be received as a link](https://github.com/XTLS/Xray-core/discussions/716#discussioncomment-6273317); and must be used as json.

You can use [this tool](https://ircfspace.github.io/fragment/) to easily create fragment json. 