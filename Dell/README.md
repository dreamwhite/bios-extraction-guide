## Introduction

In order to extract BIOS payload from Dell BIOS upgrade package you need:

- BIOS Update package
- [Dell PFS Extract script](https://github.com/platomav/BIOSUtilities/blob/refactor/Dell_PFS_Extract.py)
- [UEFITool](https://github.com/LongSoft/UEFITool)
- [modGRUBShell.efi](https://github.com/datasone/grub-mod-setup_var/releases)
- [IFRExtract](https://github.com/LongSoft/Universal-IFR-Extractor)
- [ControlMsrE2.efi from OpenCorePkg EFI/OC/Tools folder](https://github.com/acidanthera/OpenCorePkg/releases/latest)


On Dell's motherboard, BIOS extracting guide is slightly different than other vendors. 

The following procedure was tested with success on Mac OS and Windows. On Linux I've found some compiling errors with PFSExtractor. Hope to find a workarond asap.


### Step 1: extract BIOS payload bin from EXE

From a terminal window write:

`python3 Dell_PFS_Extract.py <BIOS_UPGRADE.EXE>`

It will output a folder with already extracted `.hdr` file.

### Step 2: find the offset and tweak it accordingly

Read [README.md](/README.md#unlock-cfg-lock)