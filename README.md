# my-ssh-linux

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Actions Build](https://github.com/yingfing/my-ssh-linux/actions/workflows/build.yml/badge.svg)](https://github.com/yingfing/my-ssh-linux/actions/workflows/build.yml)

一个极度精简的、可启动的 Linux 系统，启动后自动开启 SSH 服务，方便你快速获得一个远程命令行环境。

## ✨ 特点

- **体积小巧** – 完整 ISO 约 30 MB
- **快速启动** – 从 U 盘启动到获得 SSH 服务只需几秒
- **自动网络** – 通过 DHCP 自动获取 IP 地址（支持有线网卡）
- **内置 SSH** – 集成 Dropbear 轻量级 SSH 服务器
- **全自动构建** – 通过 GitHub Actions 一键构建，无需本地编译环境
- **双启动支持** – 同时支持 BIOS 和 UEFI 启动

## 📦 下载

从 [Releases](https://github.com/yingfing/my-ssh-linux/releases) 页面下载最新的 `custom-system.iso`。

## 🚀 快速开始

### 1. 制作启动盘

- **推荐：Ventoy** – 直接把 ISO 复制到 Ventoy U 盘即可
- **Linux (dd)** – `sudo dd if=custom-system.iso of=/dev/sdX bs=4M status=progress`
- **Windows** – 使用 Rufus 以 DD 模式写入

### 2. 启动系统

将 U 盘插入目标电脑，从 U 盘启动。系统启动后会显示：
Starting DHCP on eth0...
udhcpc: lease of 192.168.1.100 obtained
=== SSH system ready. root password is 'root' ===
/ #

text

### 3. SSH 登录

在另一台电脑上执行：

```bash
ssh root@<显示的IP地址>
密码：root（登录后请立即用 passwd 修改）

🔧 自定义构建
如果你想修改内核配置、添加驱动或调整启动脚本，可以 Fork 本仓库并触发 GitHub Actions：

进入仓库的 Actions 页面

选择 Build Minimal SSH System ISO (DHCP)

点击 Run workflow，填写 Release 标签

等待构建完成，ISO 会自动发布到 Releases

你可以在 .github/workflows/build.yml 中调整：

Linux 内核版本

BusyBox / Dropbear 版本

内核配置（如添加更多网卡驱动）

启动脚本 (rcS)

📂 项目结构
text
my-ssh-linux/
├── .github/workflows/
│   └── build.yml         # GitHub Actions 构建脚本
├── README.md             # 本文件
└── LICENSE               # MIT 许可证
注意：所有源码和工具都在构建时自动下载，仓库本身不存储二进制文件。

📋 技术详情
组件	版本/说明
Linux 内核	6.6.30（自定义配置）
用户态工具	BusyBox 1.36.1（静态编译）
SSH 服务	Dropbear 2024.85
网络配置	DHCP (udhcpc)
支持的网卡	Realtek R8169 (有线), RTL8188CE (无线)
启动方式	BIOS + UEFI 混合 ISO
⚠️ 注意事项
默认 root 密码为 root，请务必在首次登录后修改。

无线网卡需要额外配置 WPA（本项目未集成），推荐使用有线网络。

本系统仅用于学习和实验，请勿用于生产环境。

🤝 贡献
欢迎提出 Issue 或 Pull Request。如果你希望增加对其他网卡的支持或改进构建流程，请参考 .github/workflows/build.yml 中的步骤。

📄 许可证
本项目采用 MIT 许可证。
各组件（Linux 内核、BusyBox、Dropbear 等）均遵循其原始许可证。

🙋 常见问题
Q: 启动后无法获取 IP？
A: 确保网线已插入，且路由器开启了 DHCP。可以手动执行 udhcpc -i eth0 重试。

Q: 如何连接 WiFi？
A: 目前系统未集成 wpa_supplicant，建议使用有线网络。如需 WiFi，可在 rcS 中添加 iwconfig 命令（需要自行编译并集成）。

Q: 如何保存文件？
A: 本系统运行在内存中（initramfs），重启后所有修改都会丢失。如需持久化，可挂载硬盘或 U 盘分区。

Q: 构建失败怎么办？
A: 查看 GitHub Actions 的日志，常见原因是网络问题导致源码下载失败。可以重新触发工作流。

Happy Hacking! 🎉
