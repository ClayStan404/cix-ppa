# 附录

## A. 快速命令参考

| 命令 | 功能 |
|------|------|
| `curl -fsSL https://archive.cixtech.com/cix-repo-community.sh \| sudo sh` | 安装或卸载驱动 |
| `sudo apt update && sudo apt upgrade` | 更新系统 |
| `apt search cix-` | 搜索 CIX 包 |
| `dpkg -l \| grep cix` | 查看已安装 CIX 包 |
| `apt-mark showhold` | 查看锁定的包 |
| `uname -r` | 查看内核版本 |

## B. 相关文件路径

| 文件 | 路径 |
|------|------|
| APT 源配置 | `/etc/apt/sources.list.d/cix-deb-repo.list` |
| GPG 密钥 | `/usr/share/keyrings/cix-deb-repo.gpg` |
| GRUB 配置 | `/etc/cix/grub-config.env` |
| GRUB 刷新工具 | `/usr/libexec/cix-grub-config-refresh` |

## C. 版本历史

| 日期 | 版本 | 说明 |
|------|------|------|
| 2026-03 | 1.0 | 初始版本，仅支持 Debian 13 (trixie) |

**文档版本**: 1.0
**最后更新**: 2026 年 3 月 31 日
**适用发行版**: Debian 13 (trixie)
