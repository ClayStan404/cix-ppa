# Appendix

## A. Quick Command Reference

| Command | Function |
|---------|----------|
| `curl -fsSL https://archive.cixtech.com/cix-repo-community.sh \| sudo sh` | Install or uninstall drivers |
| `sudo apt update && sudo apt upgrade` | Update system |
| `apt search cix-` | Search CIX packages |
| `dpkg -l \| grep cix` | View installed CIX packages |
| `apt-mark showhold` | View locked packages |
| `uname -r` | Check kernel version |

## B. Related File Paths

| File | Path |
|------|------|
| APT Repository Configuration | `/etc/apt/sources.list.d/cix-deb-repo.list` |
| GPG Key | `/usr/share/keyrings/cix-deb-repo.gpg` |
| GRUB Configuration | `/etc/cix/grub-config.env` |
| GRUB Refresh Tool | `/usr/libexec/cix-grub-config-refresh` |

## C. Version History

| Date | Version | Description |
|------|---------|-------------|
| 2026-03 | 1.0 | Initial release, Debian 13 (trixie) only |

**Document Version**: 1.0
**Last Updated**: March 31, 2026
**Applicable Distribution**: Debian 13 (trixie)
