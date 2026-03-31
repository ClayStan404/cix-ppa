# Install Debian 13 GNOME Desktop

## Download Official ISO

### Recommended Mirrors (China)

```
Tsinghua University: https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current/arm64
USTC: https://mirrors.ustc.edu.cn/debian-cd/current/arm64
Aliyun: https://mirrors.aliyun.com/debian-cd/current/arm64
```

### ISO File Selection

| File | Size | Description |
|------|------|-------------|
| `debian-13.4.0-arm64-netinst.iso` | ~700MB | Network installation image (requires online installation) |
| `debian-13.4.0-arm64-DVD-1.iso` | ~4GB | Complete DVD image (includes desktop environment, supports offline installation) |

> 💡 **Tip**: The current official Debian ISO version is 13.4.0. Newer versions can be used when released.

## Create Installation USB Drive

Using **balenaEtcher** (supports Windows/Linux/macOS):

1. Download and install balenaEtcher
2. Insert USB drive (8GB or larger recommended)
3. Select ISO file -> Select USB drive -> Click "Flash"
4. Wait for writing to complete

## Install Debian

### Boot Installer

1. Insert USB drive and boot from it
2. Select `Graphical install` from the boot menu

### Installation Steps

| Step | Action | Recommendation |
|------|--------|----------------|
| **Language/Region** | Select Chinese/China | Choose as needed |
| **Keyboard Layout** | Select `American English` or `Chinese` | Based on actual keyboard |
| **Network Configuration** | Configure hostname and network | DHCP recommended |
| **User Setup** | Set username and password | **Recommended**: Leave root password blank, regular user can use sudo |
| **Partitioning** | Select `Guided - use entire disk` | Automatic partitioning recommended |
| **Software Selection** | ✅ Debian desktop environment (GNOME)<br />✅ Standard system utilities | Required |
| **GRUB Installation** | Install to main hard disk | Default is fine |

### Root Password Setup Recommendation

**Recommended**: Leave root password blank

```
During installation, when prompted to set root password, leave it blank and skip.
This automatically adds the regular user to the sudo group, allowing sudo commands.
```

**If root password already set**:

```bash
# After entering system, add regular user to sudo group
su -
usermod -aG sudo $USER
exit
```

## Complete Installation

1. Reboot system after installation completes
2. Remove USB drive
3. Enter Debian GNOME desktop
4. Open terminal and prepare to configure CIX PPA

