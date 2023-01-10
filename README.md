# Introduction

The aim of this guide is to help users modding their BIOS using the existing features provided by the BIOS itself, such as unlocking the CFG Lock, setting a custom DVMT value, undervolt etc.

Please note that most of the times the BIOS executable file is ready to be extracted using UEFITool, but others may require different ways to extract the BIOS payload.

# Disclaimer

* I am not responsible for bricked devices, dead SSD drives, thermonuclear war, 
* or you getting fired because your PC booting process failed. Please do some research 
* if you have any concerns about this procedure
* before using it! YOU are choosing to make these modifications, and if
* you blame me in any way for what happens to your device, I will laugh at you.
* BOOM! goes the dynamite

##### Thx xda-developers 
# How to extract the BIOS payload for

- [Dell](/Dell/README.md)
- [Lenovo](/Lenovo/README.md)

## Unlock CFG Lock

### Step 1: finding PE32 image section CFG Lock offset

1. Drag the payload file (tip, it has the largest size among the other files) inside UEFITool window 
2. Press search key combination (`Ctrl + F`, if on Windows, or `Command + F`, if on Mac OS)
3. Select `Text` as search criteria and look for `CFG Lock`
4. If your BIOS supports CFG Lock functionality, you should have some results in the window as depicted below

![Courtesy of Dortania CFG Lock unlocking guide](https://dortania.github.io/OpenCore-Post-Install/assets/img/uefi-tool.5f61054a.png)

5. On the bottom side of UEFITool you'll find a message such as: `Unicode text "CFG Lock" found in PE32 image section at header-offset XXYYZZ`

6. Double-click on the result to go straight to the section in which it was found.

7. Right click on `PE32 image section`, select `Extract as is` and save the file with `.bin` extension.

### Step 2: convert the `.bin` file in a `.txt` file

With `IFRExtract` you can convert a `.bin` file in a `.txt` file. 

On a terminal write: 

`<PATH IFRExtract> <PATH FILE.BIN> setup.txt`

### Step 3: finding the variable offset

Let's suppose we wanna find the variable offset which corresponds to the `CFG Lock` setting in BIOS.

With a text editor open the previously converted file and look for `CFG Lock`

If anything is found, you'll find something as

`CFG Lock, VarStoreInfo (VarOffset/VarName): 0xXYZ`

`0xXYZ` is the offset of  `CFG Lock` boolean bit.

Setting this variable value with `0x00` the `CFG Lock` will be disabled, granting access to MSR 0xE2 registry.

### Step 4: set the value to the offset

There are plenty different ways to set a certain value to a specific variable offset such as:

- [`modGRUBShell.efi`](/modGRUBShell.efi.md)
- [`RU.efi`](/ru.efi.md)
- [`setup_var.efi`](/setup_var.efi.md)

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

- [Khronokernel](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock) for MSR 0xE2 unlocking guide
- [theopolis](https://github.com/theopolis) for Python script
- [Longsoft](https://github.com/Longsoft) for PFSExtractor
- [platomav](https://github.com/platomav/BIOSUtilities) for Dell Extraction script utility
- [MacOS86 forum](https://macos86.it) for giving us the italian repository. Check it out [here](https://macos86.github.io/Estrazione-BIOS-da-exe/)
- [dreamwhite](https://github.com/dreamwhite) for testing on different MOBO vendors and for contributing on different parts of original guide
- [A23SS4NDRO](https://www.macos86.it/profile/996-a23ss4ndro/) for writing firstly this guide



