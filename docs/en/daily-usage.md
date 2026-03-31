# Daily Usage and Maintenance

## System Updates

```bash
# Update package index
sudo apt update

# Upgrade all packages
sudo apt upgrade

# Full upgrade (may remove conflicting packages)
sudo apt full-upgrade
```

## View Installed CIX Packages

```bash
# List all installed CIX packages
dpkg -l | grep cix

# Or using apt
apt list --installed | grep cix
```

## Search Available Packages

```bash
# Search CIX-related packages
apt search cix-

# View package details
apt show cix-gpu-driver
```

## Uninstall Drivers

Run the script again and select option `3`:

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

