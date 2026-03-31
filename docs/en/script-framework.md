# Script Framework and Workflow

This section describes the script framework. Users do not need to run these operations manually.

## Script Architecture

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

## Core Function Descriptions

| Function | Purpose |
|----------|---------|
| `set_closed_source_install` | Set closed-source driver installation parameters (kernel 6.6) |
| `set_open_source_install` | Set open-source driver installation parameters (kernel 7.0) |
| `select_driver_package` | Interactive driver version selection |
| `configure_cix_grub` | Configure GRUB bootloader, set kernel boot parameters |
| `remove_named_cix_packages` | Remove all packages containing `cix` in name |
| `remove_repo_configuration` | Remove APT repository and GPG key |
| `reinstall_repo_replaced_packages` | Restore official packages replaced by CIX packages |

## GPG Key Verification Mechanism

```
1. Download public key
   curl -fsSL https://archive.cixtech.com/ppa-gpg-public-key.asc

2. Convert to APT format
   gpg --dearmor > /usr/share/keyrings/cix-deb-repo.gpg

3. Reference in sources.list
   deb [signed-by=/usr/share/keyrings/cix-deb-repo.gpg] https://... trixie main
```

## APT Repository Configuration

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

