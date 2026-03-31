# 使用 cix-repo-community.sh 配置系统

## 获取并执行脚本

```bash
# 首次执行前安装 curl（如需要）
sudo apt install curl

# 一行命令完成配置
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

## 交互式选择

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

> ⚠️ **重要**：如果选择选项 **2**（开源内核 7.0），需要在安装 Debian 13 后**先配置 backports 源**，再运行此脚本。详见 [配置 backports 源](#configure-backports-repository)。

## 安装完成后的输出

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

## 重启系统

```bash
sudo reboot
```

重启后验证驱动安装：

```bash
# 检查内核版本
uname -r

# 设置 -> 系统 -> 系统详情 -> 关于 -> 系统详情 里看到显卡被正确地识别为 Mali-G720-Immortalis
```

## 配置 backports 源（开源内核用户必读） { #configure-backports-repository }

如果选择安装 **开源驱动包（`cix-debian13-k7.0-driver-opensource`）**，需要先配置 Debian 13 backports 源并安装最新固件。

### 添加 backports 源

```bash
# 编辑 sources.list
sudo nano /etc/apt/sources.list

# 添加以下行（建议国内用户使用国内镜像源）
deb http://mirrors.ustc.edu.cn/debian trixie-backports main contrib non-free non-free-firmware
```

### 更新并安装 backports 固件

```bash
# 更新软件包索引
sudo apt update

# 从 backports 安装最新固件和驱动
sudo apt install firmware-misc-nonfree libgl1-mesa-dri -t trixie-backports
```

### 运行 CIX 配置脚本

完成 backports 配置后，运行 CIX 配置脚本并选择选项 2：

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

```
Please select the driver package version to install:
  1) cix-debian13-k6.6.89-driver  (Closed-source solution)
  2) cix-debian13-k7.0-driver-opensource  (Open-source solution) <- 选择 2
  3) Uninstall CIX packages and configuration

Enter your choice [1-3, default 1]: 2
```

### 验证安装

```bash
# 检查内核版本（应显示 7.0.0-rc5-generic）
uname -r

# 检查固件版本
dpkg -l | grep firmware-misc-nonfree
```

