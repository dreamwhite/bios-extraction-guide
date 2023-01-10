## Introduction

The aim of this guide is to help users extracting information about the BIOS of their machine.
Please note that most of the times the BIOS executable file is ready to be extracted using UEFITool, but others may require different ways to extract the BIOS payload.

## How to extract the BIOS payload for

- [Dell](/Dell/README.md)
- [Lenovo](/Lenovo/README.md)

# Unlock CFG Lock

### Step 1: finding PE32 image section CFG Lock offset

1. Drag the payload file (tip, it has the largest size among the other files) inside UEFITool window 
2. Press search key combination (`Ctrl + F`, if on Windows, or `Command + F`, if on Mac OS)
3. Select `Text` as search criteria and look for `CFG Lock`
4. If your BIOS supports CFG Lock functionality, you should have some results in the window as depicted below

![Courtesy of Dortania CFG Lock unlocking guide](https://dortania.github.io/OpenCore-Post-Install/assets/img/uefi-tool.5f61054a.png)

5. On the bottom side of UEFITool you'll find a message such as: `Unicode text "CFG Lock" found in PE32 image section at header-offset XXYYZZ`

6. Double-click on the result to go straight to the section in which it was found.

7. Right click on `PE32 image section`, select `Extract as is` and save the file with `.bin` extension.

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

#### modGRUBShell.efi

With a modified GRUB shell it's possible to change the value of the variable previously found.

Fire up the modGRUBShell.efi:

- from a UEFI shell and navigate the FS (with cd and ls basic UNIX navigation commands) and find the EFI partition where it's located modGRUBShell.efi.
- from OpenCore start the boot entry `modGRUBShell.efi` (by adding it to config.plist)

**Example**

`FS0:\EFI\EFI\OC\Tools\modGRUBShell.efi` starts the modGRUBShell.efi which is inside `FS0:\`. 

After starting the modGRUBShell.efi write firstly 

`setup_var 0xXYZ` to check the default value of the found offset. If it's `0x1` then you can proceed with the next command: 

`setup_var 0xXYZ 0x00` where `0xXYZ` is the offset previously found.

Please note that if you get an errore like `error: offset 0xXYZ is out of range`, use the following commands:

- `setup_var_2 0xXYZ` (if this doesn't work go with the next)
- `setup_var3 0xYXZ`

Once did it, turn off the PC and turn it on again, not rebooting.

#### RU.efi

Read [RU.efi.md](/ru.efi.md)

### Step 5: checking if CFG Lock is really unlocked

Repeat **Step 6** and instead of firing up `modGRUBShell.efi`, fire up `ControlMsrE2.efi`. It will produce an output such as:

- `This firmware has LOCKED MSR 0xE2 Register!` if CFG Lock isn't unlocked (aka `CFG Lock Offset` value is `0x1`)
- `This firmware has UNLOCKED MSR 0xE2 Register!` if CFG Lock is unlocked (aka `CFG Lock Offset` value is `0x0`)

If the message produced is like the last one, then it means that CFG Lock is unlocked and you can proceed disabling from `config.plist`:

- **OpenCore**: `config.plist/Kernel/Quirks/AppleCpuPmCfgLock` and/or `config.plist/Kernel/Quirks/AppleXcpmCfgLock`

as those patches maybe too instable and can cause sudden reboots on your rig.


# Issues

If you encounter any issue, please file a bugreport [here](https://github.com/dreamwhite/bugtracker/issues/new?assignees=dreamwhite&labels=bug&template=generic.md&title=).

Don't forget to attach your BIOS executable file!

# Credits

[Khronokernel](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock) for MSR 0xE2 unlocking guide

[theopolis](https://github.com/theopolis) for Python script

[Longsoft](https://github.com/Longsoft) for PFSExtractor

[platomav](https://github.com/platomav/BIOSUtilities) for Dell Extraction script utility

[MacOS86 forum](https://macos86.it) for giving us the italian repository. Check it out [here](https://macos86.github.io/Estrazione-BIOS-da-exe/)

[dreamwhite](https://github.com/dreamwhite) for testing on different MOBO vendors and for contributing on different parts of original guide

[A23SS4NDRO](https://www.macos86.it/profile/996-a23ss4ndro/) for writing firstly this guide



