# CIX PPA 用户手册

## 目录

1. [什么是 PPA](#1-什么是-ppa)
2. [PPA 的优势](#2-ppa-的优势)
3. [系统要求](#3-系统要求)
4. [安装 Debian 13 GNOME 桌面](#4-安装-debian-13-gnome-桌面)
5. [使用 cix-repo-community.sh 配置系统](#5-使用-cix-repo-communitysh-配置系统)
6. [脚本框架与工作流程](#6-脚本框架与工作流程)
7. [驱动包详解](#7-驱动包详解)
8. [日常使用与维护](#8-日常使用与维护)
9. [故障排除](#9-故障排除)
10. [附录](#10-附录)

---

## 1. 什么是 PPA？

### 1.1 PPA 简介

**PPA (Personal Package Archive)** 是一种为 Debian/Ubuntu 系统设计的软件分发机制。它允许第三方开发者或组织建立自己的软件仓库，用户通过简单的配置即可从这些仓库安装和更新软件。

### 1.2 CIX PPA 是什么？

CIX PPA 是 CIX Technology 为旗下 ARM64 平台（Sky1/Sky1P SoC）专门构建的软件仓库，包含：

| 类别 | 内容 |
|------|------|
| 🖥️ **硬件驱动** | GPU、NPU、VPU、ISP、音频、WiFi、蓝牙等驱动程序 |
| 🔧 **系统组件** | 定制内核、固件、引导配置工具 |
| 📦 **多媒体库** | FFmpeg、GStreamer、Mesa、OpenCV 等优化版本 |
| 🌐 **应用软件** | CIX 定制的可硬件加速的 Chromium 浏览器 |

### 1.3 为什么需要 PPA？

Debian 官方仓库提供通用软件，但不包含 CIX 专有硬件的驱动和优化库。通过 CIX PPA，用户可以：

- ✅ 获得完整的硬件加速支持
- ✅ 享受与系统集成的无缝体验
- ✅ 通过 `apt` 命令轻松管理和更新

---

## 2. PPA 的优势

### 2.1 对用户的价值

| 优势 | 说明 |
|------|------|
| **一键安装** | 一行命令自动完成所有配置和驱动安装 |
| **自动更新** | 通过 `apt update && apt upgrade` 即可获得最新驱动 |
| **依赖管理** | APT 自动处理软件包依赖关系 |
| **版本锁定** | 关键组件（如 Chromium）可锁定版本防止兼容性问题 |
| **安全验证** | 使用 GPG 密钥验证所有软件包的完整性和真实性 |
| **易于卸载** | 提供完整的卸载选项，恢复系统到原始状态 |

### 2.2 与传统 DD Image 方式对比

之前 CIX 提供完整的 DD Image 镜像，用户需要烧写整个磁盘镜像。PPA 方式相比传统 DD Image 的优势：

| 特性 | PPA 方式 | DD Image 烧写 |
|------|----------|---------------|
| **安装复杂度** | ⭐ 简单（一行命令） | ⭐⭐⭐ 中等（需烧写完整镜像） |
| **系统更新** | ✅ apt 在线更新 | ❌ 需重新烧写完整镜像 |
| **个性化配置** | ✅ 保留用户配置 | ❌ 配置被覆盖 |
| **磁盘空间** | ✅ 按需分区 | ❌ 固定分区大小 |
| **升级灵活性** | ✅ 可升级 Debian 版本 | ❌ 依赖新镜像发布 |
| **安装时间** | ✅ 快（仅下载所需包） | ❌ 慢（需烧写数 GB 镜像） |

---

## 3. 系统要求

### 3.1 硬件要求

- **SoC**: CIX Sky1 或 Sky1P 系列芯片
- **BIOS**: 最新 daily 版本

### 3.2 软件要求

| 发行版  | 版本 | 代号   | 支持状态            |
| ------- | ---- | ------ | ------------------- |
| Debian  | 13   | trixie | ✅ 完全支持（推荐） |

> ⚠️ **注意**：目前仅支持 Debian 13 (trixie)，其他发行版支持将在后续版本中添加。

### 3.3 网络要求

- 需要互联网连接以下载软件包
- 首次安装约需下载 500M 数据

---

## 4. 安装 Debian 13 GNOME 桌面

### 4.1 下载官方 ISO

#### 推荐镜像源（中国）

```
清华大学：https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current/arm64
中科大：https://mirrors.ustc.edu.cn/debian-cd/current/arm64
阿里云：https://mirrors.aliyun.com/debian-cd/current/arm64
```

#### ISO 文件选择

| 文件 | 大小 | 说明 |
|------|------|------|
| `debian-13.4.0-arm64-netinst.iso` | ~700MB | 网络安装镜像（依赖在线安装） |
| `debian-13.4.0-arm64-DVD-1.iso` | ~4GB | 完整 DVD 镜像（包含桌面环境，可离线安装） |

> 💡 **建议**：当前 Debian 官方 iso 最新为 13.4.0 版本，后续发布新版本可以使用新版本

### 4.2 制作安装 U 盘

使用 **balenaEtcher**（支持 Windows/Linux/macOS）：

1. 下载并安装 balenaEtcher
2. 插入 U 盘（建议 8GB 以上）
3. 选择 ISO 文件 → 选择 U 盘 → 点击 "Flash"
4. 等待写入完成

### 4.3 安装 Debian

#### 4.3.1 启动安装程序

1. 插入 U 盘，从 U 盘启动
2. 在启动菜单中选择 `Graphical install`（图形化安装）

#### 4.3.2 安装步骤

| 步骤 | 操作 | 建议 |
|------|------|------|
| **语言/区域** | 选择中文/中国 | 按需选择 |
| **键盘布局** | 选择 `American English` 或 `Chinese` | 根据实际键盘 |
| **网络配置** | 配置主机名和网络 | 建议使用 DHCP |
| **用户设置** | 设置用户名和密码 | **建议**：root 密码留空，普通用户可使用 sudo |
| **分区** | 选择 `Guided - use entire disk` | 推荐自动分区 |
| **软件选择** | ✅ GNOME 桌面环境<br />✅ 标准系统工具 | 必选 |
| **GRUB 安装** | 安装到主硬盘 | 默认即可 |

#### 4.3.3 Root 密码设置建议

**推荐方式**：root 密码留空

```
在安装过程中，当提示设置 root 密码时，直接留空跳过。
这样普通用户会自动加入 sudo 组，可使用 sudo 执行管理员命令。
```

**如果已设置 root 密码**：

```bash
# 进入系统后，添加普通用户到 sudo 组
su -
usermod -aG sudo $USER
exit
```

### 4.4 完成安装

1. 安装完成后重启系统
2. 移除 U 盘
3. 进入 Debian GNOME 桌面
4. 打开终端，准备配置 CIX PPA

---

## 5. 使用 cix-repo-community.sh 配置系统

### 5.1 获取并执行脚本

```bash
# 首次执行前安装 curl（如需要）
sudo apt install curl

# 一行命令完成配置
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

### 5.2 交互式选择

脚本会提示选择驱动版本：

```
Please select the driver package version to install:
  1) cix-debian13-k6.6.89-driver  (Closed-source solution)
  2) cix-debian13-k7.0-driver-opensource  (Open-source solution)
  3) Uninstall CIX packages and configuration

Enter your choice [1-3, default 1]:
```

| 选项 | 说明 | 适用场景 |
|------|------|----------|
| **1** | 闭源驱动（内核 6.6） | 生产环境，稳定性优先（推荐） |
| **2** | 开源驱动（内核 7.0） | 开发测试，需要最新内核特性 |
| **3** | 卸载 CIX 驱动 | 需要移除驱动时 |

> ⚠️ **重要**：如果选择选项 **2**（开源内核 7.0），需要在安装 Debian 13 后**先配置 backports 源**，再运行此脚本。详见 [5.5 节](#53-配置-backports-源开源内核用户必读)。

### 5.3 安装完成后的输出

```
🎉 Repository setup completed successfully!

➡ Installing CIX driver package
Installing cix-debian13-k6.6.89-driver...
✅ cix-debian13-k6.6.89-driver installed successfully

➡ Upgrading installed packages
✅ Package upgrade completed successfully

➡ Installing chromium version 143.0.7499.109-1~deb13u1
✅ chromium 143.0.7499.109-1~deb13u1 installed successfully
➡ Holding chromium packages to prevent automatic upgrades
✅ chromium packages marked as held (will not be auto-upgraded)

➡ Configuring cix-grub-config
✅ cix-grub-config refreshed successfully

═══════════════════════════════════════════════════
🔄 Reboot to complete driver installation
═══════════════════════════════════════════════════

Target kernel vmlinuz-6.6.89-cix-build-generic is now the first GRUB boot option
```

### 5.4 重启系统

```bash
sudo reboot
```

重启后验证驱动安装：

```bash
# 检查内核版本
uname -r

# 设置 -> 系统 -> 系统详情 -> 关于 -> 系统详情 里看到显卡被正确的识别为 Mali-G720-Immortalis
```

### 5.5 配置 backports 源（开源内核用户必读）

如果选择安装 **开源驱动包（cix-debian13-k7.0-driver-opensource）**，需要先配置 Debian 13 backports 源并安装最新固件。

#### 5.5.1 添加 backports 源

```bash
# 编辑 sources.list
sudo nano /etc/apt/sources.list

# 添加以下行（建议国内用户使用国内镜像源）
deb http://mirrors.ustc.edu.cn/debian trixie-backports main contrib non-free non-free-firmware
```

#### 5.5.2 更新并安装 backports 固件

```bash
# 更新软件包索引
sudo apt update

# 从 backports 安装最新固件和驱动
sudo apt install firmware-misc-nonfree libgl1-mesa-dri -t trixie-backports
```

#### 5.5.3 运行 CIX 配置脚本

完成 backports 配置后，运行 CIX 配置脚本并选择选项 2：

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

```
Please select the driver package version to install:
  1) cix-debian13-k6.6.89-driver  (Closed-source solution)
  2) cix-debian13-k7.0-driver-opensource  (Open-source solution) ← 选择 2
  3) Uninstall CIX packages and configuration

Enter your choice [1-3, default 1]: 2
```

#### 5.5.4 验证安装

```bash
# 检查内核版本（应显示 7.0.0-rc5-generic）
uname -r

# 检查固件版本
dpkg -l | grep firmware-misc-nonfree
```

---

## 6. 脚本框架与工作流程

本段内容仅介绍脚本框架，以下操作无需用户运行

### 6.1 脚本架构

```
cix-repo-community.sh
│
├── 初始化部分
│   ├── 默认变量设置
│   ├── 颜色定义
│   └── 辅助函数（error_exit, warn, success）
│
├── 系统检测
│   ├── 读取 /etc/os-release
│   ├── 识别发行版和版本代号
│   └── 设置对应的软件源 URL
│
├── 驱动包选择
│   ├── 检测是否支持开源驱动
│   ├── 交互式菜单选择
│   └── 设置对应的驱动包名称
│
├── 安装流程
│   ├── 安装 GPG 密钥
│   ├── 配置 APT 源
│   ├── 更新软件包索引
│   ├── 安装驱动包
│   ├── 升级系统包
│   ├── 安装 Chromium（Debian 13）
│   └── 配置 GRUB
│
└── 卸载流程
    ├── 移除 CIX 包
    ├── 移除开源内核包
    ├── 移除 GRUB 配置
    ├── 移除仓库配置
    ├── 恢复官方包
    └── 清理依赖
```

### 6.2 核心函数说明

| 函数名 | 功能 |
|--------|------|
| `set_closed_source_install` | 设置闭源驱动安装参数（内核 6.6） |
| `set_open_source_install` | 设置开源驱动安装参数（内核 7.0） |
| `select_driver_package` | 交互式选择驱动版本 |
| `configure_cix_grub` | 配置 GRUB 引导，设置内核启动参数 |
| `remove_named_cix_packages` | 移除所有名称含 `cix` 的包 |
| `remove_repo_configuration` | 移除 APT 源和 GPG 密钥 |
| `reinstall_repo_replaced_packages` | 恢复被 CIX 包替换的官方包 |

### 6.3 GPG 密钥验证机制

```
1. 下载公钥
   curl -fsSL https://archive.cixtech.com/ppa-gpg-public-key.asc

2. 转换为 APT 格式
   gpg --dearmor > /usr/share/keyrings/cix-deb-repo.gpg

3. 在 sources.list 中引用
   deb [signed-by=/usr/share/keyrings/cix-deb-repo.gpg] https://... trixie main
```

### 6.4 APT 源配置

配置文件位置：`/etc/apt/sources.list.d/cix-deb-repo.list`

```bash
# Debian 13 示例
deb [signed-by=/usr/share/keyrings/cix-deb-repo.gpg] https://archive.cixtech.com/debian trixie main

# 字段说明：
# deb              - 二进制包仓库
# [signed-by=...]  - 指定 GPG 密钥路径
# https://...      - 仓库 URL
# trixie           - 发行版代号
# main             - 仓库组件
```

---

## 7. 驱动包详解

### 7.1 驱动包结构

CIX PPA 提供以下软件包：

#### 核心驱动包

| 包名 | 说明 |
|------|------|
| `cix-debian13-k6.6.89-driver` | 闭源驱动包（内核 6.6.89） |
| `cix-debian13-k7.0-driver-opensource` | 开源驱动包（内核 7.0.0-rc5） |

#### 完整包列表

**驱动组件：**
```
cix-vpu-driver-dkms       # VPU 驱动（DKMS 版本）
cix-npu-driver-dkms       # NPU 驱动（DKMS 版本）
cix-ai-engine             # AI 引擎
cix-alsa-conf             # ALSA 音频配置
cix-firmware              # 固件文件
cix-audio-dsp             # 音频 DSP
cix-audio-sof             # SOF 音频固件
cix-dpu-ddk               # DPU 开发包
cix-env                   # 环境配置
cix-bt-driver             # 蓝牙驱动
cix-wlan                  # WiFi 驱动
cix-gpu-driver            # GPU 驱动
cix-gpu-umd               # GPU 用户模式驱动
cix-gstreamer             # GStreamer 流媒体框架
cix-isp-driver-v4l2       # ISP 驱动（V4L2 接口）
cix-isp-driver            # ISP 驱动
cix-isp-umd               # ISP 用户模式驱动
cix-libcme                # CME 库
```

**多媒体库：**
```
cix-libdrm                # DRM 库
cix-libglvnd              # GLVND 库
cix-mesa                  # Mesa 3D 图形库
cix-ffmpeg                # FFmpeg 多媒体框架
cix-libavcodec61          # Libav 编解码库
cix-libavformat61         # Libav 格式库
cix-libavutil59           # Libav 工具库
cix-mnn                   # MNN 深度学习框架
cix-nnstreamer            # NNStreamer
cix-noe-umd               # NOE 用户模式驱动
cix-npu-driver            # NPU 驱动
cix-npu-onnxruntime       # ONNX Runtime
cix-npu-umd               # NPU 用户模式驱动
cix-vaapi                 # VAAPI 视频加速
cix-vpu-driver            # VPU 视频编解码驱动
```

**内核包：**
```
linux-image-6.6.89-cix-build-generic   # CIX 定制内核（6.6.89）
linux-image-7.0.0-rc5-generic          # CIX 开源内核（7.0.0-rc5）
linux-headers-7.0.0-rc5-generic        # CIX 开源内核头文件
```

**系统库和桌面组件：**
```
libva2                  # VAAPI 核心库
libva-x11-2             # VAAPI X11 支持
libva-wayland2          # VAAPI Wayland 支持
libva-drm2              # VAAPI DRM 支持
libva-glx2              # VAAPI GLX 支持
firefox-esr             # Firefox ESR 浏览器
libnm0                  # NetworkManager 库
libmutter-16-0          # Mutter 窗口管理器库
mutter-common           # Mutter 通用文件
mutter-common-bin       # Mutter 二进制文件
network-manager         # NetworkManager 网络管理
power-profiles-daemon   # 电源配置守护进程
xwayland                # XWayland 兼容层
chromium                # Chromium 浏览器
chromium-common         # Chromium 通用文件
```

**工具包：**
```
cix-grub-config         # GRUB 配置工具
```

### 7.2 单独安装驱动

如果不需要安装完整驱动包，可单独安装所需组件：

```bash
# 音频驱动
sudo apt install cix-audio-dsp cix-audio-sof

# 蓝牙驱动
sudo apt install cix-bt-driver

# WiFi 驱动
sudo apt install cix-wlan

# GPU 驱动
sudo apt install cix-gpu-driver cix-gpu-umd

# ISP 驱动
sudo apt install cix-isp-driver-v4l2 cix-isp-driver cix-isp-umd

# NPU 驱动
sudo apt install cix-npu-driver cix-npu-umd cix-mnn cix-npu-onnxruntime

# VPU 驱动
sudo apt install cix-vpu-driver

# CIX 内核
sudo apt install linux-image-6.6.89-cix-build-generic
```

### 7.3 版本锁定机制

Chromium 浏览器使用版本锁定防止自动升级导致兼容性问题：

```bash
# 锁定包
apt-mark hold chromium chromium-common

# 查看已锁定的包
apt-mark showhold

# 解除锁定
apt-mark unhold chromium chromium-common
```

---

## 8. 日常使用与维护

### 8.1 系统更新

```bash
# 更新软件包索引
sudo apt update

# 升级所有软件包
sudo apt upgrade

# 完整升级（可能移除冲突包）
sudo apt full-upgrade
```

### 8.2 查看已安装的 CIX 包

```bash
# 列出所有已安装的 CIX 包
dpkg -l | grep cix

# 或使用 apt
apt list --installed | grep cix
```

### 8.3 搜索可用包

```bash
# 搜索 CIX 相关包
apt search cix-

# 查看包详情
apt show cix-gpu-driver
```

### 8.4 卸载驱动

再次运行脚本，选择选项 `3`：

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

---

## 9. 故障排除

### 9.1 获取帮助

- **社区论坛**：访问 CIX 官方社区
- **GitHub Issues**：提交问题报告

---

## 附录

### A. 快速命令参考

| 命令 | 功能 |
|------|------|
| `curl -fsSL https://archive.cixtech.com/cix-repo-community.sh \| sudo sh` | 安装/卸载驱动 |
| `sudo apt update && sudo apt upgrade` | 更新系统 |
| `apt search cix-` | 搜索 CIX 包 |
| `dpkg -l \| grep cix` | 查看已安装 CIX 包 |
| `apt-mark showhold` | 查看锁定的包 |
| `uname -r` | 查看内核版本 |

### B. 相关文件路径

| 文件 | 路径 |
|------|------|
| APT 源配置 | `/etc/apt/sources.list.d/cix-deb-repo.list` |
| GPG 密钥 | `/usr/share/keyrings/cix-deb-repo.gpg` |
| GRUB 配置 | `/etc/cix/grub-config.env` |
| GRUB 刷新工具 | `/usr/libexec/cix-grub-config-refresh` |

### C. 版本历史

| 日期 | 版本 | 说明 |
|------|------|------|
| 2026-03 | 1.0 | 初始版本，仅支持 Debian 13 (trixie) |

---

**文档版本**: 1.0
**最后更新**: 2026 年 3 月 31 日
**适用发行版**: Debian 13 (trixie)
