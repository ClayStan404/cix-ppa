# 系统要求

## 硬件要求

- **平台**: Radxa Orion O6
- **SoC**: CIX Sky1 或 Sky1P 系列芯片
- **BIOS**: 使用当前仓库 `bios/` 目录下的 `FlashUpdate.efi` 和 `cix_flash_all_O6.bin`

### BIOS 下载

- [下载 `FlashUpdate.efi`](https://github.com/ClayStan404/cix-ppa/raw/main/bios/FlashUpdate.efi)
- [下载 `cix_flash_all_O6.bin`](https://github.com/ClayStan404/cix-ppa/raw/main/bios/cix_flash_all_O6.bin)
- [查看仓库 `bios/` 目录](https://github.com/ClayStan404/cix-ppa/tree/main/bios)

> ⚠️ **说明**：当前 BIOS 文件处于测试阶段，请先备份所有重要数据后再刷写。官方版本预计会在几天内发布。

### BIOS 刷写说明

1. 将 U 盘格式化为 FAT32。
2. 将当前仓库 `bios/` 目录下的 `FlashUpdate.efi` 和 `cix_flash_all_O6.bin` 复制到 U 盘根目录。
3. 将 U 盘插入 Radxa Orion O6 开发板。
4. 给开发板插电开机，并在插电后按 `Escape` 键进入 BIOS 界面。
5. 在 BIOS 界面中选择 `Boot Manager`，然后选择 `UEFI Shell` 进入 UEFI Shell。
6. 进入 UEFI Shell 后，找到 U 盘所在的位置，通常会显示为 `fs0`、`fs1`、`fs2` 这类磁盘。
7. 假设 U 盘位于 `fs2`，输入以下命令进入该磁盘并检查文件：

```text
fs2:
ls
```

8. 如果当前目录下能看到 `FlashUpdate.efi` 和 `cix_flash_all_O6.bin`，执行以下命令开始刷写：

```text
FlashUpdate.efi -f cix_flash_all_O6.bin
```

9. 等待 BIOS 刷写完成并重启即可。

## 软件要求

| 发行版 | 版本 | 代号 | 支持状态 |
|--------|------|------|----------|
| Debian | 13 | trixie | ✅ 完全支持（推荐） |

> ⚠️ **注意**：目前仅支持 Debian 13 (trixie)，其他发行版支持将在后续版本中添加。

## 网络要求

- 需要互联网连接以下载软件包
- 首次安装约需下载 500M 数据
