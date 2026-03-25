# 3x-ui-wiki

3X-UI 是一个高级的开源基于Web的控制面板，专为管理Xray-core服务器而设计。它为配置和监控各种VPN和代理协议提供了用户友好的界面。

![3x-ui](https://github.com/MHSanaei/3x-ui/raw/main/media/3x-ui-light.png)

## 项目简介

3X-UI 是原始 X-UI 项目的增强分支，提供改进的稳定性、更广泛的协议支持和其他功能。这个项目仅供个人使用和通信，请勿用于非法用途，请勿在生产环境中使用。

## 主要特性

- ✅ 系统状态监控
- ✅ 在所有入站和客户端中搜索
- ✅ 支持多用户和多协议
- ✅ 支持多种协议：`VMESS`、`VLESS`、`Trojan`、`Shadowsocks`、`Tunnel`、`Mixed`、`HTTP`、`Wireguard`
- ✅ 支持XTLS原生协议：`RPRX-Direct`、`Vision`、`REALITY`
- ✅ 流量统计、流量限制、过期时间限制
- ✅ 可自定义的Xray配置模板
- ✅ 支持HTTPS访问面板（自备域名+SSL证书）
- ✅ 支持一键SSL证书申请和自动续期
- ✅ 修复API路由（使用API创建用户设置）
- ✅ 支持通过面板中提供的不同项目更改配置
- ✅ 支持从面板导出/导入数据库
- ✅ 支持开箱即用的geofiles更新（geosite/geoip）
- ✅ 提供在Telegram中创建机器人来控制面板的机会

## 支持的操作系统

| 系统 | 状态 |
|------|------|
| Ubuntu | ✅ |
| Debian | ✅ |
| CentOS | ✅ |
| OpenEuler | ✅ |
| Fedora | ✅ |
| AlmaLinux | ✅ |
| Rocky Linux | ✅ |
| Oracle Linux | ✅ |
| Amazon Linux | ✅ |
| Virtuozzo Linux | ✅ |
| Windows | ✅ |
| Arch Linux | ✅ |
| Parch Linux | ✅ |
| Manjaro | ✅ |
| Armbian | ✅ |
| OpenSUSE | ✅ |

## 支持的架构

- **amd64** (推荐): 个人计算机和服务器的标准架构
- **x86 / i386**: 桌面和笔记本电脑广泛采用的架构
- **armv8 / arm64 / aarch64**: 现代移动和嵌入式设备（如树莓派4等）
- **armv7 / arm / arm32**: 较旧的移动和嵌入式设备
- **armv6 / arm / arm32**: 非常老的嵌入式设备
- **armv5 / arm / arm32**: 早期嵌入式系统相关的较老架构
- **s390x**: IBM大型机计算机常用的架构

## 支持的语言

- 🇸🇦 阿拉伯语
- 🇺🇸 英语
- 🇮🇷 波斯语
- 🇨🇳 繁体中文
- 🇨🇳 简体中文
- 🇯🇵 日语
- 🇷🇺 俄语
- 🇻🇳 越南语
- 🇪🇸 西班牙语
- 🇮🇩 印尼语
- 🇺🇦 乌克兰语
- 🇹🇷 土耳其语
- 🇧🇷 葡萄牙语（巴西）

## 项目结构

```
3x-ui-wiki/
├── Advanced.md                    # 高级配置文档
├── Common-questions-and-problems.md  # 常见问题和故障排除
├── Configuration.md               # 配置说明
├── Home.md                        # 项目主页
├── Installation.md                # 安装指南
└── _Sidebar.md                    # 侧边栏导航
```

## 快速开始

1. 查看 [Installation.md](Installation.md) 了解安装步骤
2. 查看 [Configuration.md](Configuration.md) 学习基本配置
3. 查看 [Advanced.md](Advanced.md) 了解高级功能
4. 参考 [Common-questions-and-problems.md](Common-questions-and-problems.md) 解决常见问题

## 贡献

欢迎提交Pull Request来改进这个文档项目。在提交之前，请先打开一个Issue进行讨论。

## 许可证

本项目仅供学习和个人使用。请确保在您的居住地遵守相关法律法规。

---

*此README.md由MiniMax Agent自动生成*