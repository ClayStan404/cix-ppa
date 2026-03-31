# 驱动包详解

## 驱动包结构

CIX PPA 提供以下软件包：

### 核心驱动包

| 包名 | 说明 |
|------|------|
| `cix-debian13-k6.6.89-driver` | 闭源驱动包（内核 6.6.89） |
| `cix-debian13-k7.0-driver-opensource` | 开源驱动包（内核 7.0.0-rc5） |

### 完整包列表

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

## 单独安装驱动

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

## 版本锁定机制

Chromium 浏览器使用版本锁定防止自动升级导致兼容性问题：

```bash
# 锁定包
apt-mark hold chromium chromium-common

# 查看已锁定的包
apt-mark showhold

# 解除锁定
apt-mark unhold chromium chromium-common
```

