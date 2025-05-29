<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/MHSanaei/3x-ui/raw/main/media/3x-ui-dark.png">
    <img alt="3x-ui" src="https://github.com/MHSanaei/3x-ui/raw/main/media/3x-ui-light.png">
  </picture>
</p>

## What's that?

**3X-UI** — advanced, open-source web-based control panel designed for managing Xray-core server. It offers a user-friendly interface for configuring and monitoring various VPN and proxy protocols.

> [!IMPORTANT]
> This project is only for personal using and communication, please do not use it for illegal purposes, please do not use it in a production environment. 

As an enhanced fork of the original X-UI project, 3X-UI provides improved stability, broader protocol support, and additional features.

## Features

* System Status Monitoring
* Search within all inbounds and clients
* Supports multi-user and multi-protocol
* Supports protocols, including `VMESS`, `VLESS`, `Trojan`, `Shadowsocks`, `Dokodemo-door`, `SOCKS5`, `HTTP`, `Wireguard`
* Supports XTLS native Protocols, including `RPRX-Direct`, `Vision`, `REALITY`
* Traffic statistics, traffic limit, expiration time limit
* Customizable Xray configuration templates
* Supports HTTPS access panel (self-provided domain name + SSL certificate)
* Supports One-Click SSL certificate application and automatic renewal
* For more advanced configuration items, please refer to the panel
* Fixes API routes (user setting will be created with API)
* Supports changing configs by different items provided in the panel.
* Supports export/import database from the panel
* Supports updating geofiles (geosite's/geoip's) out of the box
* Provides an opportunity to create a bot in Telegram to control the panel

## Supported OS

> [!TIP]
> If you don't have your OS here, you can use Docker

At the moment we support major operating systems, here is the list of OS on which we have tested the panel support

| System | Minimal Version |
| - | - |
| Ubuntu | 22.04 |
| Debian | 12 |
| CentOS | 8 |
| OpenEuler | 22.03 |
| Fedora | 36 |
| AlmaLinux | 9.5 |
| Rocky Linux | 9.5 |
| Oracle Linux | 8 |
| Amazon Linux | 2023 |
| Virtuozzo Linux | 8 |
| Windows | Server 2016 |
| Arch Linux | — |
| Parch Linux | — |
| Manjaro | — |
| Armbian | — |
| OpenSUSE | — |

## Supported architectures

Our platform offers compatibility with a diverse range of architectures and devices, ensuring flexibility across various computing environments. The following are key architectures that we support:

* `amd64` (recommended): This prevalent architecture is the standard for personal computers and servers, accommodating most modern operating systems seamlessly.

* `x86` / `i386`: Widely adopted in desktop and laptop computers, this architecture enjoys broad support from numerous operating systems and applications, including but not limited to Windows, macOS, and Linux systems.

* `armv8` / `arm64` / `aarch64`: Tailored for contemporary mobile and embedded devices, such as smartphones and tablets, this architecture is exemplified by devices like Raspberry Pi 4, Raspberry Pi 3, Raspberry Pi Zero 2/Zero 2 W, Orange Pi 3 LTS, and more.

* `armv7` / `arm` / `arm32`: Serving as the architecture for older mobile and embedded devices, it remains widely utilized in devices like Orange Pi Zero LTS, Orange Pi PC Plus, Raspberry Pi 2, among others.

* `armv6` / `arm` / `arm32`: Geared towards very old embedded devices, this architecture, while less prevalent, is still in use. Devices such as Raspberry Pi 1, Raspberry Pi Zero/Zero W, rely on this architecture.

* `armv5` / `arm` / `arm32`: An older architecture primarily associated with early embedded systems, it is less common today but may still be found in legacy devices like early Raspberry Pi versions and some older smartphones.

* `s390x`: This architecture is commonly used in IBM mainframe computers and offers high performance and reliability for enterprise workloads.

## Supported languages

We currently support the following languages

| Flag | Languages |
| - | - |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/sa.svg) | Arabic |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/us.svg) | English |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/ir.svg) | Persian |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/cn.svg) | Traditional Chinese |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/cn.svg) | Simplified Chinese |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/jp.svg) | Japanese |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/ru.svg) | Russian |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/vn.svg) | Vietnamese |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/es.svg) | Spanish |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/id.svg) | Indonesian |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/ua.svg) | Ukrainian |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/tr.svg) | Turkish |
| ![Flag](https://cdn.jsdelivr.net/gh/hampusborgos/country-flags@main/svg/br.svg) | Português (Brazil) |

## Screenshots

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/8fcdcb20-2b71-4f52-98a7-7b38218c886c">
  <img alt="3x-ui" src="https://github.com/user-attachments/assets/13f084e4-ebbc-4b47-a17f-aaf0841103d8">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/60227da1-672f-418f-8d44-3268e6081312">
  <img alt="3x-ui" src="https://github.com/user-attachments/assets/fcb31c3f-81f6-46de-98b0-372ea70b2825">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/MHSanaei/3x-ui/raw/main/media/03-add-inbound-dark.png">
  <img alt="3x-ui" src="https://github.com/MHSanaei/3x-ui/raw/main/media/03-add-inbound-light.png">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/MHSanaei/3x-ui/raw/main/media/04-add-client-dark.png">
  <img alt="3x-ui" src="https://github.com/MHSanaei/3x-ui/raw/main/media/04-add-client-light.png">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/6b91f432-e324-49bd-a533-f8ab27fa3acf">
  <img alt="3x-ui" src="https://github.com/user-attachments/assets/044bcdb5-82c9-403b-a54a-dc54631de374">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/52e82c08-1c73-409f-b1da-0dd40d95de90">
  <img alt="3x-ui" src="https://github.com/user-attachments/assets/7db91766-0e2b-4a84-8b86-649ef86d2262">
</picture>