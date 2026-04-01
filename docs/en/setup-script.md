# Configure System with cix-repo-community.sh

## Get and Execute Script

```bash
# Install curl first if needed
sudo apt install curl

# One command to complete configuration
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

## Interactive Selection

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

> ⚠️ **Important**: If selecting option **2** (open-source kernel 7.0), you must **configure backports repository first** before running this script. See [Configure Backports Repository](#configure-backports-repository).

## Installation Output

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

## Reboot System

```bash
sudo reboot
```

After reboot, verify driver installation:

```bash
# Check kernel version
uname -r

# Settings -> System -> System Details -> About -> System Details should show GPU correctly identified as Mali-G720-Immortalis
```

## Configure Backports Repository { #configure-backports-repository }

If installing the **open-source driver package (`cix-debian13-k7.0-driver-opensource`)**, you must first configure Debian 13 backports repository and install latest firmware.

### Add Backports Repository

```bash
# Edit sources.list
sudo nano /etc/apt/sources.list

# Add the following line
deb http://deb.debian.org/debian trixie-backports main contrib non-free non-free-firmware
```

### Update and Install Backports Firmware

```bash
# Update package index
sudo apt update

# Install latest firmware and drivers from backports
sudo apt install firmware-misc-nonfree libgl1-mesa-dri -t trixie-backports
```

### Run CIX Configuration Script

After backports configuration, run CIX script and select option 2:

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

```
Please select the driver package version to install:
  1) cix-debian13-k6.6.89-driver  (Closed-source solution)
  2) cix-debian13-k7.0-driver-opensource  (Open-source solution) <- Select 2
  3) Uninstall CIX packages and configuration

Enter your choice [1-3, default 1]: 2
```

### Verify Installation

```bash
# Check kernel version (should show 7.0.0-rc5-generic)
uname -r

# Check firmware version
dpkg -l | grep firmware-misc-nonfree
```
