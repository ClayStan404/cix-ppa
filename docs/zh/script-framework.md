# 脚本框架与工作流程

本段内容仅介绍脚本框架，以下操作无需用户运行。

## 脚本架构

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

## 核心函数说明

| 函数名 | 功能 |
|--------|------|
| `set_closed_source_install` | 设置闭源驱动安装参数（内核 6.6） |
| `set_open_source_install` | 设置开源驱动安装参数（内核 7.0） |
| `select_driver_package` | 交互式选择驱动版本 |
| `configure_cix_grub` | 配置 GRUB 引导，设置内核启动参数 |
| `remove_named_cix_packages` | 移除所有名称含 `cix` 的包 |
| `remove_repo_configuration` | 移除 APT 源和 GPG 密钥 |
| `reinstall_repo_replaced_packages` | 恢复被 CIX 包替换的官方包 |

## GPG 密钥验证机制

```
1. 下载公钥
   curl -fsSL https://archive.cixtech.com/ppa-gpg-public-key.asc

2. 转换为 APT 格式
   gpg --dearmor > /usr/share/keyrings/cix-deb-repo.gpg

3. 在 sources.list 中引用
   deb [signed-by=/usr/share/keyrings/cix-deb-repo.gpg] https://... trixie main
```

## APT 源配置

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

