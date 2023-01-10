## Introduction

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