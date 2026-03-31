# CIX PPA User Manual

## Table of Contents

1. [What is PPA?](#1-what-is-ppa)
2. [Benefits of PPA](#2-benefits-of-ppa)
3. [System Requirements](#3-system-requirements)
4. [Install Debian 13 GNOME Desktop](#4-install-debian-13-gnome-desktop)
5. [Configure System with cix-repo-community.sh](#5-configure-system-with-cix-repo-communitysh)
6. [Script Framework and Workflow](#6-script-framework-and-workflow)
7. [Driver Packages Details](#7-driver-packages-details)
8. [Daily Usage and Maintenance](#8-daily-usage-and-maintenance)
9. [Troubleshooting](#9-troubleshooting)
10. [Appendix](#10-appendix)

---

## 1. What is PPA?

### 1.1 Introduction to PPA

**PPA (Personal Package Archive)** is a software distribution mechanism designed for Debian/Ubuntu systems. It allows third-party developers or organizations to establish their own software repositories, from which users can install and update software through simple configuration.

### 1.2 What is CIX PPA?

CIX PPA is a software repository specifically built by CIX Technology for its ARM64 platforms (Sky1/Sky1P SoC), containing:

| Category | Content |
|----------|---------|
| 🖥️ **Hardware Drivers** | GPU, NPU, VPU, ISP, Audio, WiFi, Bluetooth drivers |
| 🔧 **System Components** | Custom kernel, firmware, bootloader configuration tools |
| 📦 **Multimedia Libraries** | Optimized versions of FFmpeg, GStreamer, Mesa, OpenCV |
| 🌐 **Application Software** | CIX custom hardware-accelerated Chromium browser |

### 1.3 Why PPA?

Debian official repositories provide general-purpose software but do not include drivers and optimized libraries for CIX proprietary hardware. Through CIX PPA, users can:

- ✅ Get complete hardware acceleration support
- ✅ Enjoy seamless integration with the system
- ✅ Easily manage and update via `apt` commands

---

## 2. Benefits of PPA

### 2.1 Value to Users

| Benefit | Description |
|---------|-------------|
| **One-Click Installation** | One command completes all configuration and driver installation |
| **Automatic Updates** | Get latest drivers via `apt update && apt upgrade` |
| **Dependency Management** | APT automatically handles package dependencies |
| **Version Locking** | Critical components (like Chromium) can be locked to prevent compatibility issues |
| **Security Verification** | GPG keys verify integrity and authenticity of all packages |
| **Easy Uninstallation** | Complete uninstallation options to restore system to original state |

### 2.2 Comparison with Traditional DD Image Method

Previously, CIX provided complete DD Image files that required users to flash the entire disk image. PPA offers these advantages over traditional DD Image:

| Feature | PPA Method | DD Image Flashing |
|---------|------------|-------------------|
| **Installation Complexity** | ⭐ Simple (one command) | ⭐⭐⭐ Moderate (requires full image flash) |
| **System Updates** | ✅ Online apt updates | ❌ Requires re-flashing entire image |
| **Personalized Configuration** | ✅ Retains user settings | ❌ Configuration overwritten |
| **Disk Space** | ✅ Partition on demand | ❌ Fixed partition size |
| **Upgrade Flexibility** | ✅ Can upgrade Debian version | ❌ Depends on new image release |
| **Installation Time** | ✅ Fast (download only needed packages) | ❌ Slow (flash several GB image) |

---

## 3. System Requirements

### 3.1 Hardware Requirements

- **SoC**: CIX Sky1 or Sky1P series chips
- **BIOS**: Latest daily version

### 3.2 Software Requirements

| Distribution | Version | Codename | Support Status |
|--------------|---------|----------|----------------|
| Debian | 13 | trixie | ✅ Fully Supported (Recommended) |

> ⚠️ **Note**: Currently only Debian 13 (trixie) is supported. Support for other distributions will be added in future releases.

### 3.3 Network Requirements

- Internet connection required to download packages
- Initial installation downloads approximately 500MB of data

---

## 4. Install Debian 13 GNOME Desktop

### 4.1 Download Official ISO

#### Recommended Mirrors (China)

```
Tsinghua University: https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current/arm64
USTC: https://mirrors.ustc.edu.cn/debian-cd/current/arm64
Aliyun: https://mirrors.aliyun.com/debian-cd/current/arm64
```

#### ISO File Selection

| File | Size | Description |
|------|------|-------------|
| `debian-13.4.0-arm64-netinst.iso` | ~700MB | Network installation image (requires online installation) |
| `debian-13.4.0-arm64-DVD-1.iso` | ~4GB | Complete DVD image (includes desktop environment, supports offline installation) |

> 💡 **Tip**: The current official Debian ISO version is 13.4.0. Newer versions can be used when released.

### 4.2 Create Installation USB Drive

Using **balenaEtcher** (supports Windows/Linux/macOS):

1. Download and install balenaEtcher
2. Insert USB drive (8GB or larger recommended)
3. Select ISO file → Select USB drive → Click "Flash"
4. Wait for writing to complete

### 4.3 Install Debian

#### 4.3.1 Boot Installer

1. Insert USB drive and boot from it
2. Select `Graphical install` from the boot menu

#### 4.3.2 Installation Steps

| Step | Action | Recommendation |
|------|--------|----------------|
| **Language/Region** | Select Chinese/China | Choose as needed |
| **Keyboard Layout** | Select `American English` or `Chinese` | Based on actual keyboard |
| **Network Configuration** | Configure hostname and network | DHCP recommended |
| **User Setup** | Set username and password | **Recommended**: Leave root password blank, regular user can use sudo |
| **Partitioning** | Select `Guided - use entire disk` | Automatic partitioning recommended |
| **Software Selection** | ✅ Debian desktop environment (GNOME)<br />✅ Standard system utilities | Required |
| **GRUB Installation** | Install to main hard disk | Default is fine |

#### 4.3.3 Root Password Setup Recommendation

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

### 4.4 Complete Installation

1. Reboot system after installation completes
2. Remove USB drive
3. Enter Debian GNOME desktop
4. Open terminal and prepare to configure CIX PPA

---

## 5. Configure System with cix-repo-community.sh

### 5.1 Get and Execute Script

```bash
# Install curl first if needed
sudo apt install curl

# One command to complete configuration
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

### 5.2 Interactive Selection

The script prompts for driver version selection:

```
Please select the driver package version to install:
  1) cix-debian13-k6.6.89-driver  (Closed-source solution)
  2) cix-debian13-k7.0-driver-opensource  (Open-source solution)
  3) Uninstall CIX packages and configuration

Enter your choice [1-3, default 1]:
```

| Option | Description | Use Case |
|--------|-------------|----------|
| **1** | Closed-source driver (kernel 6.6) | Production environment, stability priority (Recommended) |
| **2** | Open-source driver (kernel 7.0) | Development/testing, needs latest kernel features |
| **3** | Uninstall CIX drivers | When drivers need to be removed |

> ⚠️ **Important**: If selecting option **2** (open-source kernel 7.0), you must **configure backports repository first** before running this script. See [Section 5.5](#55-configure-backports-repository-required-for-open-source-kernel).

### 5.3 Installation Output

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

### 5.4 Reboot System

```bash
sudo reboot
```

After reboot, verify driver installation:

```bash
# Check kernel version
uname -r

# Settings -> System -> System Details -> About -> System Details should show GPU correctly identified as Mali-G720-Immortalis
```

### 5.5 Configure Backports Repository (Required for Open-Source Kernel)

If installing the **open-source driver package (cix-debian13-k7.0-driver-opensource)**, you must first configure Debian 13 backports repository and install latest firmware.

#### 5.5.1 Add Backports Repository

```bash
# Edit sources.list
sudo nano /etc/apt/sources.list

# Add the following line (Chinese users may prefer domestic mirrors)
deb http://mirrors.ustc.edu.cn/debian trixie-backports main contrib non-free non-free-firmware
```

#### 5.5.2 Update and Install Backports Firmware

```bash
# Update package index
sudo apt update

# Install latest firmware and drivers from backports
sudo apt install firmware-misc-nonfree libgl1-mesa-dri -t trixie-backports
```

#### 5.5.3 Run CIX Configuration Script

After backports configuration, run CIX script and select option 2:

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

```
Please select the driver package version to install:
  1) cix-debian13-k6.6.89-driver  (Closed-source solution)
  2) cix-debian13-k7.0-driver-opensource  (Open-source solution) ← Select 2
  3) Uninstall CIX packages and configuration

Enter your choice [1-3, default 1]: 2
```

#### 5.5.4 Verify Installation

```bash
# Check kernel version (should show 7.0.0-rc5-generic)
uname -r

# Check firmware version
dpkg -l | grep firmware-misc-nonfree
```

---

## 6. Script Framework and Workflow

This section describes the script framework. Users do not need to run these operations manually.

### 6.1 Script Architecture

```
cix-repo-community.sh
│
├── Initialization
│   ├── Default variable setup
│   ├── Color definitions
│   └── Helper functions (error_exit, warn, success)
│
├── System Detection
│   ├── Read /etc/os-release
│   ├── Identify distribution and codename
│   └── Set corresponding repository URL
│
├── Driver Package Selection
│   ├── Check open-source driver support
│   ├── Interactive menu selection
│   └── Set corresponding driver package name
│
├── Installation Process
│   ├── Install GPG key
│   ├── Configure APT repository
│   ├── Update package index
│   ├── Install driver packages
│   ├── Upgrade system packages
│   ├── Install Chromium (Debian 13)
│   └── Configure GRUB
│
└── Uninstallation Process
    ├── Remove CIX packages
    ├── Remove open-source kernel packages
    ├── Remove GRUB configuration
    ├── Remove repository configuration
    ├── Restore official packages
    └── Clean up dependencies
```

### 6.2 Core Function Descriptions

| Function | Purpose |
|----------|---------|
| `set_closed_source_install` | Set closed-source driver installation parameters (kernel 6.6) |
| `set_open_source_install` | Set open-source driver installation parameters (kernel 7.0) |
| `select_driver_package` | Interactive driver version selection |
| `configure_cix_grub` | Configure GRUB bootloader, set kernel boot parameters |
| `remove_named_cix_packages` | Remove all packages containing 'cix' in name |
| `remove_repo_configuration` | Remove APT repository and GPG key |
| `reinstall_repo_replaced_packages` | Restore official packages replaced by CIX packages |

### 6.3 GPG Key Verification Mechanism

```
1. Download public key
   curl -fsSL https://archive.cixtech.com/ppa-gpg-public-key.asc

2. Convert to APT format
   gpg --dearmor > /usr/share/keyrings/cix-deb-repo.gpg

3. Reference in sources.list
   deb [signed-by=/usr/share/keyrings/cix-deb-repo.gpg] https://... trixie main
```

### 6.4 APT Repository Configuration

Configuration file location: `/etc/apt/sources.list.d/cix-deb-repo.list`

```bash
# Debian 13 example
deb [signed-by=/usr/share/keyrings/cix-deb-repo.gpg] https://archive.cixtech.com/debian trixie main

# Field descriptions:
# deb              - Binary package repository
# [signed-by=...]  - Specify GPG key path
# https://...      - Repository URL
# trixie           - Distribution codename
# main             - Repository component
```

---

## 7. Driver Packages Details

### 7.1 Package Structure

CIX PPA provides the following packages:

#### Core Driver Packages

| Package | Description |
|---------|-------------|
| `cix-debian13-k6.6.89-driver` | Closed-source driver package (kernel 6.6.89) |
| `cix-debian13-k7.0-driver-opensource` | Open-source driver package (kernel 7.0.0-rc5) |

#### Complete Package List

**Driver Components:**
```
cix-vpu-driver-dkms       # VPU driver (DKMS version)
cix-npu-driver-dkms       # NPU driver (DKMS version)
cix-ai-engine             # AI engine
cix-alsa-conf             # ALSA audio configuration
cix-firmware              # Firmware files
cix-audio-dsp             # Audio DSP
cix-audio-sof             # SOF audio firmware
cix-dpu-ddk               # DPU development kit
cix-env                   # Environment configuration
cix-bt-driver             # Bluetooth driver
cix-wlan                  # WiFi driver
cix-gpu-driver            # GPU driver
cix-gpu-umd               # GPU user-mode driver
cix-gstreamer             # GStreamer streaming framework
cix-isp-driver-v4l2       # ISP driver (V4L2 interface)
cix-isp-driver            # ISP driver
cix-isp-umd               # ISP user-mode driver
cix-libcme                # CME library
```

**Multimedia Libraries:**
```
cix-libdrm                # DRM library
cix-libglvnd              # GLVND library
cix-mesa                  # Mesa 3D graphics library
cix-ffmpeg                # FFmpeg multimedia framework
cix-libavcodec61          # Libav codec library
cix-libavformat61         # Libav format library
cix-libavutil59           # Libav utility library
cix-mnn                   # MNN deep learning framework
cix-nnstreamer            # NNStreamer
cix-noe-umd               # NOE user-mode driver
cix-npu-driver            # NPU driver
cix-npu-onnxruntime       # ONNX Runtime
cix-npu-umd               # NPU user-mode driver
cix-vaapi                 # VAAPI video acceleration
cix-vpu-driver            # VPU video codec driver
```

**Kernel Packages:**
```
linux-image-6.6.89-cix-build-generic   # CIX custom kernel (6.6.89)
linux-image-7.0.0-rc5-generic          # CIX open-source kernel (7.0.0-rc5)
linux-headers-7.0.0-rc5-generic        # CIX open-source kernel headers
```

**System Libraries and Desktop Components:**
```
libva2                  # VAAPI core library
libva-x11-2             # VAAPI X11 support
libva-wayland2          # VAAPI Wayland support
libva-drm2              # VAAPI DRM support
libva-glx2              # VAAPI GLX support
firefox-esr             # Firefox ESR browser
libnm0                  # NetworkManager library
libmutter-16-0          # Mutter window manager library
mutter-common           # Mutter common files
mutter-common-bin       # Mutter binaries
network-manager         # NetworkManager network management
power-profiles-daemon   # Power profiles daemon
xwayland                # XWayland compatibility layer
chromium                # Chromium browser
chromium-common         # Chromium common files
```

**Tool Packages:**
```
cix-grub-config         # GRUB configuration tool
```

### 7.2 Install Drivers Individually

If you don't need the complete driver package, you can install individual components:

```bash
# Audio driver
sudo apt install cix-audio-dsp cix-audio-sof

# Bluetooth driver
sudo apt install cix-bt-driver

# WiFi driver
sudo apt install cix-wlan

# GPU driver
sudo apt install cix-gpu-driver cix-gpu-umd

# ISP driver
sudo apt install cix-isp-driver-v4l2 cix-isp-driver cix-isp-umd

# NPU driver
sudo apt install cix-npu-driver cix-npu-umd cix-mnn cix-npu-onnxruntime

# VPU driver
sudo apt install cix-vpu-driver

# CIX kernel
sudo apt install linux-image-6.6.89-cix-build-generic
```

### 7.3 Version Locking Mechanism

Chromium browser uses version locking to prevent automatic upgrades that may cause compatibility issues:

```bash
# Lock packages
apt-mark hold chromium chromium-common

# View locked packages
apt-mark showhold

# Unlock packages
apt-mark unhold chromium chromium-common
```

---

## 8. Daily Usage and Maintenance

### 8.1 System Updates

```bash
# Update package index
sudo apt update

# Upgrade all packages
sudo apt upgrade

# Full upgrade (may remove conflicting packages)
sudo apt full-upgrade
```

### 8.2 View Installed CIX Packages

```bash
# List all installed CIX packages
dpkg -l | grep cix

# Or using apt
apt list --installed | grep cix
```

### 8.3 Search Available Packages

```bash
# Search CIX-related packages
apt search cix-

# View package details
apt show cix-gpu-driver
```

### 8.4 Uninstall Drivers

Run the script again and select option `3`:

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

---

## 9. Troubleshooting

### 9.1 Get Help

- **Community Forum**: Visit CIX official community
- **GitHub Issues**: Submit bug reports

---

## 10. Appendix

### A. Quick Command Reference

| Command | Function |
|---------|----------|
| `curl -fsSL https://archive.cixtech.com/cix-repo-community.sh \| sudo sh` | Install/uninstall drivers |
| `sudo apt update && sudo apt upgrade` | Update system |
| `apt search cix-` | Search CIX packages |
| `dpkg -l \| grep cix` | View installed CIX packages |
| `apt-mark showhold` | View locked packages |
| `uname -r` | Check kernel version |

### B. Related File Paths

| File | Path |
|------|------|
| APT Repository Configuration | `/etc/apt/sources.list.d/cix-deb-repo.list` |
| GPG Key | `/usr/share/keyrings/cix-deb-repo.gpg` |
| GRUB Configuration | `/etc/cix/grub-config.env` |
| GRUB Refresh Tool | `/usr/libexec/cix-grub-config-refresh` |

### C. Version History

| Date | Version | Description |
|------|---------|-------------|
| 2026-03 | 1.0 | Initial release, Debian 13 (trixie) only |

---

**Document Version**: 1.0
**Last Updated**: March 31, 2026
**Applicable Distribution**: Debian 13 (trixie)
