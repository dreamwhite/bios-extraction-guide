# Introduction

The aim of this guide is to help users modding their BIOS using the existing features provided by the BIOS itself, such as unlocking the CFG Lock, setting a custom DVMT value, undervolt etc.

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
- Other vendors may directly give the BIOS payload which can be opened directly from UEFITool (e.g. ASUS, MSI, Gigabyte).

## Unlock CFG Lock

##### Please note that this section is hackintosh-only related. If you're a Winblows/Leenux user skip it

### Step 1: finding PE32 image section CFG Lock offset

1. Open UEFITool
2. Drag the payload file (tip, it has the largest size among the other files) inside UEFITool window 
3. Press search key combination (`Ctrl + F`, if on Windows, or `Command + F`, if on Mac OS)
4. Select `Text` as search criteria and look for `CFG Lock`
5. If your BIOS supports CFG Lock functionality, you should have some results in the window as depicted below

![Courtesy of Dortania CFG Lock unlocking guide](https://dortania.github.io/OpenCore-Post-Install/assets/img/uefi-tool.5f61054a.png)

6. On the bottom side of UEFITool you'll find a message such as: `Unicode text "CFG Lock" found in PE32 image section at header-offset XXYYZZ`
7. Double-click on the result to go straight to the section in which it was found.
8. Right click on `PE32 image section`, select `Extract as is` and save the file with `.bin` extension. (e.g. `Section_PE32_image_Setup.bin`)

### Step 2: convert the `.bin` file in a `.txt` file

With [IFRExtractor](https://github.com/LongSoft/IFRExtractor-RS/releases/latest) you can convert a `.bin` file in a `.txt` file. 

On a terminal write: 

```bash
<PATH IFRExtractor> <PATH FILE.BIN> setup.txt
```

e.g. 

```bash
<PATH IFRExtractor> Section_PE32_image_Setup.bin Section_PE32_image_Setup.txt
```

### Step 3: finding the variable offset

Let's suppose we wanna find the variable offset which corresponds to the `CFG Lock` setting in BIOS.

With a text editor open the previously converted file and look for `CFG Lock`

If anything is found, you'll find something as

`CFG Lock, VarStoreInfo (VarOffset/VarName): 0xXYZ`

`0xXYZ` is the offset of  `CFG Lock` boolean bit.

Setting this variable value with `0x00` the `CFG Lock` will be disabled, granting access to MSR 0xE2 registry.

### Step 4: set the value to the offset

There are plenty different tools to set a certain value to a specific variable offset such as:

- [`modGRUBShell.efi`](/modGRUBShell.efi.md)
- [`RU.efi`](/ru.efi.md)
- [`setup_var.efi`](/setup_var.efi.md)

My favourite one - mainly because it's open source, has a good maintainer, and it's simple to use - is `setup_var.efi` but you're free to choose the tool you want. 
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



