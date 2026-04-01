# System Requirements

## Hardware Requirements

- **Platform**: Radxa Orion O6
- **SoC**: CIX Sky1 or Sky1P series chips
- **BIOS**: Use `FlashUpdate.efi` and `cix_flash_all_O6.bin` from the repository `bios/` directory

### BIOS Downloads

- [Download `FlashUpdate.efi`](https://github.com/ClayStan404/cix-ppa/raw/main/bios/FlashUpdate.efi)
- [Download `cix_flash_all_O6.bin`](https://github.com/ClayStan404/cix-ppa/raw/main/bios/cix_flash_all_O6.bin)
- [Browse the repository `bios/` directory](https://github.com/ClayStan404/cix-ppa/tree/main/bios)

### BIOS File Checksums

- `FlashUpdate.efi`
  MD5: `0a4b30e07c8408abd84db076ec4ab7c9`
- `cix_flash_all_O6.bin`
  MD5: `dd034964c999bebd37d116f2ea8eb99e`

> ⚠️ **Note**: These BIOS files are currently for testing. Back up all important data before flashing. An official release is expected within the next few days.

### BIOS Flashing Instructions

1. Format a USB drive as FAT32.
2. Copy `FlashUpdate.efi` and `cix_flash_all_O6.bin` from the repository `bios/` directory to the root of the USB drive.
3. Insert the USB drive into the Radxa Orion O6 board.
4. Power on the board and press `Escape` after power-on to enter the BIOS screen.
5. In the BIOS screen, choose `Boot Manager`, then choose `UEFI Shell`.
6. In UEFI Shell, find the USB drive. It is usually exposed as `fs0`, `fs1`, or `fs2`.
7. If the USB drive is `fs2`, enter it and list the files:

```text
fs2:
ls
```

8. If you can see `FlashUpdate.efi` and `cix_flash_all_O6.bin`, run the flashing command:

```text
FlashUpdate.efi -f cix_flash_all_O6.bin
```

9. Wait for the BIOS flashing process to finish and reboot the board.

## Software Requirements

| Distribution | Version | Codename | Support Status |
|--------------|---------|----------|----------------|
| Debian | 13 | trixie | ✅ Fully Supported (Recommended) |

> ⚠️ **Note**: Currently only Debian 13 (trixie) is supported. Support for other distributions will be added in future releases.

## Network Requirements

- Internet connection required to download packages
- Initial installation downloads approximately 500MB of data
