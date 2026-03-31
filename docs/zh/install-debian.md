# 安装 Debian 13 GNOME 桌面

## 下载官方 ISO

### 推荐镜像源（中国）

```
清华大学：https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current/arm64
中科大：https://mirrors.ustc.edu.cn/debian-cd/current/arm64
阿里云：https://mirrors.aliyun.com/debian-cd/current/arm64
```

### ISO 文件选择

| 文件 | 大小 | 说明 |
|------|------|------|
| `debian-13.4.0-arm64-netinst.iso` | ~700MB | 网络安装镜像（依赖在线安装） |
| `debian-13.4.0-arm64-DVD-1.iso` | ~4GB | 完整 DVD 镜像（包含桌面环境，可离线安装） |

> 💡 **建议**：当前 Debian 官方 ISO 最新为 13.4.0 版本，后续发布新版本可以使用新版本。

## 制作安装 U 盘

使用 **balenaEtcher**（支持 Windows/Linux/macOS）：

1. 下载并安装 balenaEtcher
2. 插入 U 盘（建议 8GB 以上）
3. 选择 ISO 文件 -> 选择 U 盘 -> 点击 "Flash"
4. 等待写入完成

## 安装 Debian

### 启动安装程序

1. 插入 U 盘，从 U 盘启动
2. 在启动菜单中选择 `Graphical install`（图形化安装）

### 安装步骤

| 步骤 | 操作 | 建议 |
|------|------|------|
| **语言/区域** | 选择中文/中国 | 按需选择 |
| **键盘布局** | 选择 `American English` 或 `Chinese` | 根据实际键盘 |
| **网络配置** | 配置主机名和网络 | 建议使用 DHCP |
| **用户设置** | 设置用户名和密码 | **建议**：root 密码留空，普通用户可使用 sudo |
| **分区** | 选择 `Guided - use entire disk` | 推荐自动分区 |
| **软件选择** | ✅ GNOME 桌面环境<br />✅ 标准系统工具 | 必选 |
| **GRUB 安装** | 安装到主硬盘 | 默认即可 |

### Root 密码设置建议

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

## 完成安装

1. 安装完成后重启系统
2. 移除 U 盘
3. 进入 Debian GNOME 桌面
4. 打开终端，准备配置 CIX PPA

