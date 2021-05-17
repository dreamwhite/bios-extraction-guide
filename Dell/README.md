## Introduction

In order to extract BIOS payload from Dell BIOS upgrade package you need:

- BIOS Update package
- [Dell PFS Extract script](https://github.com/platomav/BIOSUtilities/blob/master/Dell%20PFS%20BIOS%20Extractor/Dell_PFS_Extract.py)
- [UEFITool](https://github.com/LongSoft/UEFITool)
- [modGRUBShell.efi](https://github.com/datasone/grub-mod-setup_var/releases)
- [IFRExtract](https://github.com/LongSoft/Universal-IFR-Extractor)
- [VerifyMsrE2.efi from OpenCorePkg EFI/OC/Tools folder](https://github.com/acidanthera/OpenCorePkg/releases/latest)


On Dell's motherboard, BIOS extracting guide is slightly different than other vendors. 

The following procedure was tested with success on Mac OS and Windows. On Linux I've found some compiling errors with PFSExtractor. Hope to find a workarond asap.


### Step 1: extract BIOS payload bin from EXE

From a terminal window write:

`python3 Dell_PFS_Extract.py <BIOS_UPGRADE.EXE>`

It will output a folder with already extracted `.hdr` file.

# Unlock CFG Lock

### Step 1: finding PE32 image section CFG Lock offset

Drag the ~~`.payload`~~ file which is named as `System BIOS with BIOS Guard vX.X.X` file inside UEFITool window and by pressing `Ctrl + F`, if on Windows, or `Command + F`, if on Mac OS, and selecting `Text` as search criteria, look for `CFG Lock`.

On the bottom side of UEFITool you'll find a message such as 

`Unicode text "CFG Lock" found in PE32 image section at offset XXYYZZ`

Right click on `PE32 image section`, select `Extract as is` and save the file with `.bin` extension.

### Step 2: convert .bin file in .txt file

With `IFRExtract` you can convert a `.bin` file in a  `.txt` file. 

On a terminal write: 

`<PATH IFRExtract> <PATH FILE.BIN> setup.txt`

### Step 3: finding CFG Lock offset 

With a text editor open the previously converted file and look for `CFG Lock`

If anything is found, you'll find something as

`CFG Lock, VarStoreInfo (VarOffset/VarName): 0xXYZ`

`0xXYZ` is the offset of  `CFG Lock` boolean bit.

Setting this variable value with `0x00` the `CFG Lock` will be disabled, granting access to MSR 0xE2 registry.

### Step 4: setting CFG Lock to 0x00

With a modified GRUB shell it's possible to change the value of the variable previously found.

Fire up the modGRUBShell.efi with Clover UEFI shell:

- from Clover fire up UEFI shell and navigate the FS (with cd and ls basic UNIX navigation commands) and find the EFI partition where it's located modGRUBShell.efi.
- from OpenCore start the boot entry `modGRUBShell.efi` (by adding it to config.plist)

**Example**

`FS0:\EFI\EFI\CLOVER\tools\modGRUBShell.efi` starts the modGRUBShell.efi which is inside `FS0:\`. 

After starting the modGRUBShell.efi write firstly 
`setup_var 0xXYZ` to check the default value of the found offset. If it's `0x1` then you can proceed with the next command: 

`setup_var 0xXYZ 0x00` where `0xXYZ` is the offset previously found.

Once did it, turn off the PC and turn it on again, not rebooting.

### Step 5: checking if CFG Lock is really unlocked

Repeat **Step 6** and instead of firing up `modGRUBShell.efi`, fire up `VerifyMsrE2.efi`. It will produce an output such as:

- `This firmware has LOCKED MSR 0xE2 Register!` if CFG Lock isn't unlocked (aka `CFG Lock Offset` value is `0x1`)
- `This firmware has UNLOCKED MSR 0xE2 Register!` if CFG Lock is unlocked (aka `CFG Lock Offset` value is `0x0`)

If the message produced is like the last one, then it means that CFG Lock is unlocked and you can proceed disabling from `config.plist`:

- **Clover**: `KernelPM` and/or `KernelXCPM`
- **OpenCore**: `AppleCpuPmCfgLock` and/or `AppleXcpmCfgLock`

as those patches maybe too instable and can cause sudden reboots on your rig.


# Credits

[Khronokernel](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock) for MSR 0xE2 unlocking guide

[theopolis](https://github.com/theopolis) for Python script

[Longsoft](https://github.com/Longsoft) for PFSExtractor

[platomav](https://github.com/platomav/BIOSUtilities) for Dell Extraction script utility

[MacOS86 forum](https://macos86.it) for giving us the italian repository. Check it out [here](https://macos86.github.io/Estrazione-BIOS-da-exe/)

[dreamwhite](https://github.com/dreamwhite) for testing on different MOBO vendors and for contributing on different parts of original guide

[A23SS4NDRO](https://www.macos86.it/profile/996-a23ss4ndro/) for writing firstly this guide



