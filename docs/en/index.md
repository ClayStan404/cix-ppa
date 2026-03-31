# CIX PPA User Manual

> Published as the official CIX PPA documentation site with GitHub Pages and MkDocs.

## Quick Start

```bash
# Install curl first if needed
sudo apt install curl

# Configure the CIX repository and install drivers
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

## In This Manual

- [Benefits of PPA](benefits.md)
- [System Requirements](system-requirements.md)
- [Install Debian 13 GNOME Desktop](install-debian.md)
- [Configure System with `cix-repo-community.sh`](setup-script.md)
- [Script Framework and Workflow](script-framework.md)
- [Driver Packages Details](driver-packages.md)
- [Daily Usage and Maintenance](daily-usage.md)
- [Troubleshooting](troubleshooting.md)
- [Appendix](appendix.md)

## What is PPA?

## Introduction to PPA

**PPA (Personal Package Archive)** is a software distribution mechanism designed for Debian/Ubuntu systems. It allows third-party developers or organizations to establish their own software repositories, from which users can install and update software through simple configuration.

## What is CIX PPA?

CIX PPA is a software repository specifically built by CIX Technology for its ARM64 platforms (Sky1/Sky1P SoC), containing:

| Category | Content |
|----------|---------|
| 🖥️ **Hardware Drivers** | GPU, NPU, VPU, ISP, Audio, WiFi, Bluetooth drivers |
| 🔧 **System Components** | Custom kernel, firmware, bootloader configuration tools |
| 📦 **Multimedia Libraries** | Optimized versions of FFmpeg, GStreamer, Mesa, OpenCV |
| 🌐 **Application Software** | CIX custom hardware-accelerated Chromium browser |

## Why PPA?

Debian official repositories provide general-purpose software but do not include drivers and optimized libraries for CIX proprietary hardware. Through CIX PPA, users can:

- ✅ Get complete hardware acceleration support
- ✅ Enjoy seamless integration with the system
- ✅ Easily manage and update via `apt` commands

