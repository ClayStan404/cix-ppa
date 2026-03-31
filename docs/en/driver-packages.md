# Driver Packages Details

## Package Structure

CIX PPA provides the following packages:

### Core Driver Packages

| Package | Description |
|---------|-------------|
| `cix-debian13-k6.6.89-driver` | Closed-source driver package (kernel 6.6.89) |
| `cix-debian13-k7.0-driver-opensource` | Open-source driver package (kernel 7.0.0-rc5) |

### Complete Package List

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

## Install Drivers Individually

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

## Version Locking Mechanism

Chromium browser uses version locking to prevent automatic upgrades that may cause compatibility issues:

```bash
# Lock packages
apt-mark hold chromium chromium-common

# View locked packages
apt-mark showhold

# Unlock packages
apt-mark unhold chromium chromium-common
```

