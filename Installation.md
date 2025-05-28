## Docker (recommended)

1. Before installing Docker, install the following tools using your package manager: `curl`, `nano` (if necessary)

2. Install Docker using the official script

```bash
$ bash <(curl -sSL https://get.docker.com)
```

### Using Docker Compose

3. Create a `panel` folder and go to this folder

```bash
$ mkdir panel
$ cd panel
```

4. Create and edit the `compose.yml` file using `nano` or another editor. Insert the following contents into the file:

`compose.yml`
```yml
services:
  panel:
    image: ghcr.io/mhsanaei/3x-ui:latest
    container_name: 3xui_app
    volumes:
      - ~/panel/db/:/etc/x-ui/
      - ~/panel/cert/:/root/cert/
    environment:
      XRAY_VMESS_AEAD_FORCED: "false"
      XUI_ENABLE_FAIL2BAN: "true"
    tty: true
    network_mode: host
    restart: unless-stopped
```



5. Start the Docker container using the following command.

```bash
$ docker compose up -d
```

6. Open url `http://<your-ip>:2053` and log in to the panel. The credentials are as follows:

- 👤 Username: `admin`
- 🔑 Password: `admin`

> [!CAUTION]
> After logging in, **immediately** change the administrator credentials in the panel settings (`Panel Settings > Authentication`)

> [!TIP]
> It is also recommended to set up two-factor authentication and set up another panel web path for complete security

#### Update

If an update is needed, disable the container and push the new version of the image with the following commands

```bash
$ docker compose down
$ docker compose pull
$ docker compose up -d
```

#### Delete

If you want to delete a container, execute the following commands

```bash
$ docker compose down
$ docker system prune -a
$ rm panel -rf
```

### Using CLI

3. Execute the following command

```bash
$ docker run -itd \
   -e XRAY_VMESS_AEAD_FORCED=false \
   -e XUI_ENABLE_FAIL2BAN=true \
   -v $PWD/db/:/etc/x-ui/ \
   -v $PWD/cert/:/root/cert/ \
   --network=host \
   --restart=unless-stopped \
   --name 3x-ui \
   ghcr.io/mhsanaei/3x-ui:latest
```

#### Update

If an update is needed, disable the container and push the new version of the image with the following commands

```bash
$ docker ps -a
```

With this command, we find out the ID of the container on which the panel is running. Next, we stop our container and pull down the image:

```bash
$ docker container stop <container_id>
$ docker image pull ghcr.io/mhsanaei/3x-ui
```

After that, we execute the command from step 3.

#### Delete

If you want to delete a container, execute the following commands

```bash
$ docker container stop <container_id>
$ docker system prune -a
$ rm 3x-ui -rf
```

## Install in one-line

1. Install the necessary tools to run the script: `curl` (if needed)
2. Open shell and enter this command
```bash
$ bash <(curl -Ls https://raw.githubusercontent.com/MHSanaei/3x-ui/refs/tags/latest/install.sh)
```
3. Go through the panel setup
4. Once configured, go to `http://<your-ip>:<your-port>` and log in with the credentials that were issued by the panel after installation

## Manual installation

To download the latest version of the compressed package directly to your server, run the following command:

```bash
ARCH=$(uname -m)
case "${ARCH}" in
  x86_64 | x64 | amd64) XUI_ARCH="amd64" ;;
  i*86 | x86) XUI_ARCH="386" ;;
  armv8* | armv8 | arm64 | aarch64) XUI_ARCH="arm64" ;;
  armv7* | armv7) XUI_ARCH="armv7" ;;
  armv6* | armv6) XUI_ARCH="armv6" ;;
  armv5* | armv5) XUI_ARCH="armv5" ;;
  s390x) echo 's390x' ;;
  *) XUI_ARCH="amd64" ;;
esac

wget https://github.com/MHSanaei/3x-ui/releases/latest/download/x-ui-linux-${XUI_ARCH}.tar.gz
```

Once the compressed package is downloaded, execute the following commands to install or upgrade x-ui

```bash
ARCH=$(uname -m)
case "${ARCH}" in
  x86_64 | x64 | amd64) XUI_ARCH="amd64" ;;
  i*86 | x86) XUI_ARCH="386" ;;
  armv8* | armv8 | arm64 | aarch64) XUI_ARCH="arm64" ;;
  armv7* | armv7) XUI_ARCH="armv7" ;;
  armv6* | armv6) XUI_ARCH="armv6" ;;
  armv5* | armv5) XUI_ARCH="armv5" ;;
  s390x) echo 's390x' ;;
  *) XUI_ARCH="amd64" ;;
esac

cd /root/
rm -rf x-ui/ /usr/local/x-ui/ /usr/bin/x-ui
tar zxvf x-ui-linux-${XUI_ARCH}.tar.gz
chmod +x x-ui/x-ui x-ui/bin/xray-linux-* x-ui/x-ui.sh
cp x-ui/x-ui.sh /usr/bin/x-ui
cp -f x-ui/x-ui.service /etc/systemd/system/
mv x-ui/ /usr/local/
systemctl daemon-reload
systemctl enable x-ui
systemctl restart x-ui
```

## Install another version

> [!CAUTION]
> This method is not recommended. Always use the latest version.

1. Install the necessary tools to run the script: `curl` (if needed)

2. Open shell and enter this command
```bash
$ VERSION=v2.5.5 && bash <(curl -Ls "https://raw.githubusercontent.com/mhsanaei/3x-ui/$VERSION/install.sh") $VERSION
```

The required version is specified in the `VERSION` variable, e.g. `v2.5.5`.

3. Go through the panel setup