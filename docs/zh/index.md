# CIX PPA 用户手册

> 使用 GitHub Pages 和 MkDocs 发布的 CIX PPA 官方文档站。

## 快速开始

```bash
# 如未安装 curl，先安装
sudo apt install curl

# 配置 CIX 软件源并安装驱动
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

## 手册内容

- [PPA 的优势](benefits.md)
- [系统要求](system-requirements.md)
- [安装 Debian 13 GNOME 桌面](install-debian.md)
- [使用 `cix-repo-community.sh` 配置系统](setup-script.md)
- [脚本框架与工作流程](script-framework.md)
- [驱动包详解](driver-packages.md)
- [日常使用与维护](daily-usage.md)
- [故障排除](troubleshooting.md)
- [附录](appendix.md)

## 什么是 PPA？

## PPA 简介

**PPA (Personal Package Archive)** 是一种为 Debian/Ubuntu 系统设计的软件分发机制。它允许第三方开发者或组织建立自己的软件仓库，用户通过简单的配置即可从这些仓库安装和更新软件。

## CIX PPA 是什么？

CIX PPA 是 CIX Technology 为旗下 ARM64 平台（Sky1/Sky1P SoC）专门构建的软件仓库，包含：

| 类别 | 内容 |
|------|------|
| 🖥️ **硬件驱动** | GPU、NPU、VPU、ISP、音频、WiFi、蓝牙等驱动程序 |
| 🔧 **系统组件** | 定制内核、固件、引导配置工具 |
| 📦 **多媒体库** | FFmpeg、GStreamer、Mesa、OpenCV 等优化版本 |
| 🌐 **应用软件** | CIX 定制的可硬件加速的 Chromium 浏览器 |

## 为什么需要 PPA？

Debian 官方仓库提供通用软件，但不包含 CIX 专有硬件的驱动和优化库。通过 CIX PPA，用户可以：

- ✅ 获得完整的硬件加速支持
- ✅ 享受与系统集成的无缝体验
- ✅ 通过 `apt` 命令轻松管理和更新

