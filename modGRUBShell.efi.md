## Introduction

With a modified GRUB shell it's possible to change the value of the variable previously found.

Fire up `modGRUBShell.efi`:

- from a UEFI shell and navigate the FS (with `cd` and `ls` basic UNIX navigation commands) and find the `EFI partition` where it's located `modGRUBShell.efi`.
- from OpenCore start the boot entry `modGRUBShell.efi` (by adding it to `config.plist` under `Misc/Tools` context)

You'll end up in a custom shell

## Step 1: read the current value of the offset

There are different ways to interact with the desired offset, by using different commands.
I'll list them below:

### `setup_var(2/_3)` command

The syntax of the command is as it follows: `setup_var <OFFSET>`

Run it but in case you get an error like `error: offset 0xXYZ is out of range` use the following commands:

- `setup_var2 0xXYZ` (if this doesn't work go with the next)
- `setup_var_3 0xYXZ`


The difference between each of these commands is the `VarStore` name where the offset are looked in:

- `setup_var` looks inside `Setup`
- `setup_var2` looks inside `Custom` (usually shitty `InsydeH2O` BIOS may have it)
- `setup_var3` looks inside `Setup` but discards variables that are too small (currently the threshold is `0x10` bytes), which allows skipping dummy varstores

### `setup_var_cv` command

Recent firmwares often stores BIOS settings into multiple varstores, and sometimes most of them are not in default "Setup" name. setup_var_cv allows accessing varying size variables in varstore with the given name.

The usage is as it follows:

`setup_var_cv nameOfVarStore offsetInVarStore [optional variable size] [optional value to write]`

e.g. `CFG Lock` offset is `0x527` under `Setup` `VarStoreName`, with size of `1` and `0x0` as the value to write in the offset.

The resulting command will be `setup_var_cv CpuSetup 0x43 0x01 0x00`

## Which command should I use first?

I'd recommend starting from `setup_var(2/_3)` and in case you have issues use `setup_var_cv`.


## Conclusions

Once changed the offset value **turn off your PC** and **turn it on again**. Soft-reboot are not valid, therefore you need a cold reboot.

# Credits

- [datasone](https://github.com/datasone/) for [mogGRUBShell.efi](https://github.com/datasone/grub-mod-setup_var)