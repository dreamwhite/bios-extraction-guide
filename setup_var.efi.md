# Introduction

The aim of this guide is to help users with `setup_var.efi`.<br>
Please note that it's a rewritten version of `modGRUBShell.efi`, and in my opinion is way better than `RU.efi`.
Please note that if your BIOS is write-locked there's nothing to do here: blame the vendor.

# Disclaimer

* I am not responsible for bricked devices, dead SSD drives, thermonuclear war, 
* or you getting fired because your PC booting process failed. Please do some research 
* if you have any concerns about this procedure
* before using it! YOU are choosing to make these modifications, and if
* you blame me in any way for what happens to your device, I will laugh at you.
* BOOM! goes the dynamite

# Step 1: get the IFR and the offset

Read [ru.efi.md](/ru.efi.md#how-to-start-parse-the-internal-form-representation)

# Step 2: run `setup_var.efi`

You can both choose to run it from a UEFI shell (like `OpenShell.efi`) or run it directly from OpenCore menu (useful in case you need to tweak BIOS options rapidly).

In any case, the syntax of the program doesn't change:

`setup_var.efi <OFFSET> <VALUE> -n <VAR_NAME> -r`

where:

* `<OFFSET>` is the offset of the variable
* `<VALUE>` is the value of the variable
* `-n <VAR_NAME>` is the `VarStoreName`
* `-r` reboots the system once written the value

e.g. if my `CFG Lock` offset is `0x527` under `Setup` `VarStoreName` and needs to be set to `0x0` the resulting command will be:

`setup_var.efi 0x527 0x0 -n Setup -r`

#### OpenCore config.plist setup

In case you wanna use the tool from OpenCore, add the tool under `Misc/Tools` context in `config.plist`, and make sure `FullNvramAccess` key is enabled.

I'll leave the copy-and-paste code (make sure to edit `Arguments` with your own):

```xml
<dict>
	<key>Arguments</key>
	<string>0x527 0x00 -n Setup --write_on_demand</string>
	<key>Auxiliary</key>
	<true/>
	<key>Comment</key>
	<string>UEFI command-line tool for read/write access of variables - CFG Lock unlock</string>
	<key>Enabled</key>
	<true/>
	<key>Flavour</key>
	<string>Auto</string>
	<key>FullNvramAccess</key>
	<true/>
	<key>Name</key>
	<string>setup_var - CFG Unlock</string>
	<key>Path</key>
	<string>setup_var.efi</string>
	<key>RealPath</key>
	<true/>
	<key>TextMode</key>
	<true/>
</dict>
```

# Credits

- [1alessandro1](https://github.com/1alessandro1) for letting me discover this wonderful tool
- [datasone](https://github.com/datasone) for writing from scratch in RUST [`setup_var.efi`](https://github.com/datasone/setup_var.efi)