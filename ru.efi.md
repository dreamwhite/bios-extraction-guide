# Introduction

The aim of this guide is to help users with RU.efi.<br>
Please note that most of the times, `modGRUBShell.efi` is still a valid alternative, but if cannot proceed via `modGRUBShell.efi` then this guide is for you!

# Disclaimer

* I am not responsible for bricked devices, dead SSD drives, thermonuclear war, 
* or you getting fired because your PC booting process failed. Please do some research 
* if you have any concerns about this procedure
* before using it! YOU are choosing to make these modifications, and if
* you blame me in any way for what happens to your device, I will laugh at you.
* BOOM! goes the dynamite

##### Thx xda-developers 


# How to start: parse the Internal Form Representation

Given that you should have already extracted your BIOS payload and got the right offset to edit (e.g. `CFG Lock 0x527`), you should do the following:


![CFG Lock offset](https://github.com/dreamwhite/dell-inspiron-5370-hackintosh/raw/master/.assets/docs/bios/images/cfg_lock.png)

1. Write down the value of `VarStore` variable (in this example `0x1`).

2. Scroll up to the top of the extracted txt till you find something like depicted below:

```
                         Internal Forms Representation
--------------------------------------------------------------------------------
Offset:		Instruction:
--------------------------------------------------------------------------------
0x1EA7C Form Set: Setup [7B59104A-C00D-4158-87FF-F04D6396A915], ClassGuid0 [93039971-8545-4B04-B45E-32EB8326040E] {0E A7 4A 10 59 7B 0D C0 58 41 87 FF F0 4D 63 96 A9 15 06 00 07 00 01 71 99 03 93 45 85 04 4B B4 5E 32 EB 83 26 04 0E}
0x1EAA3 	Guid: [0F0B1735-87A0-4193-B266-538C38AF48CE] {5F 15 35 17 0B 0F A0 87 93 41 B2 66 53 8C 38 AF 48 CE 03 01 00}
0x1EAB8 	Guid: [0F0B1735-87A0-4193-B266-538C38AF48CE] {5F 15 35 17 0B 0F A0 87 93 41 B2 66 53 8C 38 AF 48 CE 04 00 00}
0x1EACD 	Default Store: , DefaultId: 0x0 {5C 06 00 00 00 00}
0x1EAD3 	Default Store: , DefaultId: 0x1 {5C 06 02 00 01 00}
0x1EAD9 	VarStore: VarStoreId: 0x1 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x14FB, Name: Setup {24 1C 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 01 00 FB 14 53 65 74 75 70 00}
0x1EAF5 	VarStore: VarStoreId: 0x2 [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x2, Name: PlatformLang {24 23 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 02 00 02 00 50 6C 61 74 66 6F 72 6D 4C 61 6E 67 00}
0x1EB18 	VarStore: VarStoreId: 0x3 [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x2, Name: PlatformLangCodes {24 28 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 03 00 02 00 50 6C 61 74 66 6F 72 6D 4C 61 6E 67 43 6F 64 65 73 00}
0x1EB40 	VarStore: VarStoreId: 0x4 [E770BB69-BCB4-4D04-9E97-23FF9456FEAC], Size: 0x1, Name: SystemAccess {24 23 69 BB 70 E7 B4 BC 04 4D 9E 97 23 FF 94 56 FE AC 04 00 01 00 53 79 73 74 65 6D 41 63 63 65 73 73 00}
0x1EB63 	VarStore: VarStoreId: 0x5 [9CF0F18E-7C7D-49DE-B5AA-BBBAD6B21007], Size: 0x2, Name: AMICallback {24 22 8E F1 F0 9C 7D 7C DE 49 B5 AA BB BA D6 B2 10 07 05 00 02 00 41 4D 49 43 61 6C 6C 62 61 63 6B 00}
0x1EB85 	VarStore: VarStoreId: 0x6 [C811FA38-42C8-4579-A9BB-60E94EDDFB34], Size: 0x51, Name: AMITSESetup {24 22 38 FA 11 C8 C8 42 79 45 A9 BB 60 E9 4E DD FB 34 06 00 51 00 41 4D 49 54 53 45 53 65 74 75 70 00}
0x1EBA7 	VarStore: VarStoreId: 0x7 [B4909CF3-7B93-4751-9BD8-5BA8220B9BB2], Size: 0x2, Name: BootManager {24 22 F3 9C 90 B4 93 7B 51 47 9B D8 5B A8 22 0B 9B B2 07 00 02 00 42 6F 6F 74 4D 61 6E 61 67 65 72 00}
0x1EBC9 	VarStore: VarStoreId: 0x8 [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x2, Name: Timeout {24 1E 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 08 00 02 00 54 69 6D 65 6F 75 74 00}
0x1EBE7 	VarStore: VarStoreId: 0x9 [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x2, Name: BootOrder {24 20 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 09 00 02 00 42 6F 6F 74 4F 72 64 65 72 00}
0x1EC07 	VarStore: VarStoreId: 0xA [19D96D3F-6A6A-47D2-B195-7B2432DA3BE2], Size: 0x11C, Name: AddBootOption {24 24 3F 6D D9 19 6A 6A D2 47 B1 95 7B 24 32 DA 3B E2 0A 00 1C 01 41 64 64 42 6F 6F 74 4F 70 74 69 6F 6E 00}
0x1EC2B 	VarStore: VarStoreId: 0xB [F6C73719-F34C-479C-B32F-277FCBBCFE4F], Size: 0x2, Name: DelBootOption {24 24 19 37 C7 F6 4C F3 9C 47 B3 2F 27 7F CB BC FE 4F 0B 00 02 00 44 65 6C 42 6F 6F 74 4F 70 74 69 6F 6E 00}
0x1EC4F 	VarStore: VarStoreId: 0xC [A56074DB-65FE-45F7-BD21-2D2BDD8E9652], Size: 0x2, Name: LegacyDev {24 20 DB 74 60 A5 FE 65 F7 45 BD 21 2D 2B DD 8E 96 52 0C 00 02 00 4C 65 67 61 63 79 44 65 76 00}
0x1EC6F 	VarStore: VarStoreId: 0xD [A56074DB-65FE-45F7-BD21-2D2BDD8E9652], Size: 0x2, Name: LegacyGroup {24 22 DB 74 60 A5 FE 65 F7 45 BD 21 2D 2B DD 8E 96 52 0D 00 02 00 4C 65 67 61 63 79 47 72 6F 75 70 00}
0x1EC91 	VarStore: VarStoreId: 0xE [A56074DB-65FE-45F7-BD21-2D2BDD8E9652], Size: 0x2, Name: LegacyDevOrder {24 25 DB 74 60 A5 FE 65 F7 45 BD 21 2D 2B DD 8E 96 52 0E 00 02 00 4C 65 67 61 63 79 44 65 76 4F 72 64 65 72 00}
0x1ECB6 	VarStore: VarStoreId: 0xF [052E6EB0-F240-42C5-8309-45874545C6B4], Size: 0x2, Name: BootNowCount {24 23 B0 6E 2E 05 40 F2 C5 42 83 09 45 87 45 45 C6 B4 0F 00 02 00 42 6F 6F 74 4E 6F 77 43 6F 75 6E 74 00}
0x1ECD9 	VarStore: VarStoreId: 0x10 [C57AD6B7-0515-40A8-9D21-551652854E37], Size: 0x2, Name: Shell {24 1C B7 D6 7A C5 15 05 A8 40 9D 21 55 16 52 85 4E 37 10 00 02 00 53 68 65 6C 6C 00}
0x1ECF5 	VarStore: VarStoreId: 0x11 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x29, Name: SetupCpuFeatures {24 27 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 11 00 29 00 53 65 74 75 70 43 70 75 46 65 61 74 75 72 65 73 00}
0x1ED1C 	VarStore: VarStoreId: 0x12 [B08F97FF-E6E8-4193-A997-5E9E9B0ADB32], Size: 0xA, Name: CpuSetupVolatileData {24 2B FF 97 8F B0 E8 E6 93 41 A9 97 5E 9E 9B 0A DB 32 12 00 0A 00 43 70 75 53 65 74 75 70 56 6F 6C 61 74 69 6C 65 44 61 74 61 00}
0x1ED47 	VarStore: VarStoreId: 0x13 [90D93E09-4E91-4B3D-8C77-C82FF10E3C81], Size: 0x6, Name: CpuSmm {24 1D 09 3E D9 90 91 4E 3D 4B 8C 77 C8 2F F1 0E 3C 81 13 00 06 00 43 70 75 53 6D 6D 00}
0x1ED64 	VarStore: VarStoreId: 0x14 [5432122D-D034-49D2-A6DE-65A829EB4C74], Size: 0xA, Name: MeSetupStorage {24 25 2D 12 32 54 34 D0 D2 49 A6 DE 65 A8 29 EB 4C 74 14 00 0A 00 4D 65 53 65 74 75 70 53 74 6F 72 61 67 65 00}
0x1ED89 	VarStore: VarStoreId: 0x15 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x1, Name: SetupAmtFeatures {24 27 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 15 00 01 00 53 65 74 75 70 41 6D 74 46 65 61 74 75 72 65 73 00}
0x1EDB0 	VarStore: VarStoreId: 0x16 [64192DCA-D034-49D2-A6DE-65A829EB4C74], Size: 0x11, Name: IccAdvancedSetupDataVar {24 2E CA 2D 19 64 34 D0 D2 49 A6 DE 65 A8 29 EB 4C 74 16 00 11 00 49 63 63 41 64 76 61 6E 63 65 64 53 65 74 75 70 44 61 74 61 56 61 72 00}
0x1EDDE 	VarStore: VarStoreId: 0x17 [B63BF800-F267-4F55-9217-E97FB3B69846], Size: 0x2, Name: DynamicPageCount {24 27 00 F8 3B B6 67 F2 55 4F 92 17 E9 7F B3 B6 98 46 17 00 02 00 44 79 6E 61 6D 69 63 50 61 67 65 43 6F 75 6E 74 00}
0x1EE05 	VarStore: VarStoreId: 0x18 [0885F288-418C-4BE1-A6AF-8BAD61DA08FE], Size: 0x2, Name: DriverHlthEnable {24 27 88 F2 85 08 8C 41 E1 4B A6 AF 8B AD 61 DA 08 FE 18 00 02 00 44 72 69 76 65 72 48 6C 74 68 45 6E 61 62 6C 65 00}
0x1EE2C 	VarStore: VarStoreId: 0x19 [7459A7D4-6533-4480-BBA7-79E25A4443C9], Size: 0x2, Name: DriverHealthCount {24 28 D4 A7 59 74 33 65 80 44 BB A7 79 E2 5A 44 43 C9 19 00 02 00 44 72 69 76 65 72 48 65 61 6C 74 68 43 6F 75 6E 74 00}
0x1EE54 	VarStore: VarStoreId: 0x1A [58279C2D-FB19-466E-B42E-CD437016DC25], Size: 0x2, Name: DrvHealthCtrlCnt {24 27 2D 9C 27 58 19 FB 6E 46 B4 2E CD 43 70 16 DC 25 1A 00 02 00 44 72 76 48 65 61 6C 74 68 43 74 72 6C 43 6E 74 00}
0x1EE7B 	VarStore: VarStoreId: 0x1B [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0xA, Name: SetupPlatformData {24 28 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 1B 00 0A 00 53 65 74 75 70 50 6C 61 74 66 6F 72 6D 44 61 74 61 00}
0x1EEA3 	VarStore: VarStoreId: 0x1C [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x11, Name: NBPlatformData {24 25 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 1C 00 11 00 4E 42 50 6C 61 74 66 6F 72 6D 44 61 74 61 00}
0x1EEC8 	VarStore: VarStoreId: 0x1D [C0B4FB05-15E5-4588-9FE9-B3D39C067715], Size: 0x2, Name: DriverManager {24 24 05 FB B4 C0 E5 15 88 45 9F E9 B3 D3 9C 06 77 15 1D 00 02 00 44 72 69 76 65 72 4D 61 6E 61 67 65 72 00}
0x1EEEC 	VarStore: VarStoreId: 0x1E [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x2, Name: DriverOrder {24 22 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 1E 00 02 00 44 72 69 76 65 72 4F 72 64 65 72 00}
0x1EF0E 	VarStore: VarStoreId: 0x1F [C143929C-BF5D-423B-999B-0F2DD2B61FF7], Size: 0x2, Name: AmiGopPolicySetupData {24 2C 9C 92 43 C1 5D BF 3B 42 99 9B 0F 2D D2 B6 1F F7 1F 00 02 00 41 6D 69 47 6F 70 50 6F 6C 69 63 79 53 65 74 75 70 44 61 74 61 00}
0x1EF3A 	VarStore: VarStoreId: 0x20 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x2, Name: NBGopPlatformData {24 28 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 20 00 02 00 4E 42 47 6F 70 50 6C 61 74 66 6F 72 6D 44 61 74 61 00}
0x1EF62 	VarStore: VarStoreId: 0x21 [D1405D16-7AFC-4695-BB12-41459D3695A2], Size: 0x6, Name: NetworkStackVar {24 26 16 5D 40 D1 FC 7A 95 46 BB 12 41 45 9D 36 95 A2 21 00 06 00 4E 65 74 77 6F 72 6B 53 74 61 63 6B 56 61 72 00}
0x1EF88 	VarStore: VarStoreId: 0x22 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x4A, Name: SdioDevConfiguration {24 2B 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 22 00 4A 00 53 64 69 6F 44 65 76 43 6F 6E 66 69 67 75 72 61 74 69 6F 6E 00}
0x1EFB3 	VarStore: VarStoreId: 0x23 [7B59104A-C00D-4158-87FF-F04D6396A915], Size: 0x7, Name: SecureBootSetup {24 26 4A 10 59 7B 0D C0 58 41 87 FF F0 4D 63 96 A9 15 23 00 07 00 53 65 63 75 72 65 42 6F 6F 74 53 65 74 75 70 00}
0x1EFD9 	VarStore: VarStoreId: 0x24 [7B59104A-C00D-4158-87FF-F04D6396A915], Size: 0x5, Name: SecureVarPresent {24 27 4A 10 59 7B 0D C0 58 41 87 FF F0 4D 63 96 A9 15 24 00 05 00 53 65 63 75 72 65 56 61 72 50 72 65 73 65 6E 74 00}
0x1F000 	VarStore: VarStoreId: 0x25 [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x1, Name: VendorKeys {24 21 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 25 00 01 00 56 65 6E 64 6F 72 4B 65 79 73 00}
0x1F021 	VarStore: VarStoreId: 0x26 [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x1, Name: SetupMode {24 20 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 26 00 01 00 53 65 74 75 70 4D 6F 64 65 00}
0x1F041 	VarStore: VarStoreId: 0x27 [8BE4DF61-93CA-11D2-AA0D-00E098032B8C], Size: 0x1, Name: SecureBoot {24 21 61 DF E4 8B CA 93 D2 11 AA 0D 00 E0 98 03 2B 8C 27 00 01 00 53 65 63 75 72 65 42 6F 6F 74 00}
0x1F062 	VarStore: VarStoreId: 0x28 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x2, Name: UsbMassDevNum {24 24 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 28 00 02 00 55 73 62 4D 61 73 73 44 65 76 4E 75 6D 00}
0x1F086 	VarStore: VarStoreId: 0x29 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x10, Name: UsbMassDevValid {24 26 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 29 00 10 00 55 73 62 4D 61 73 73 44 65 76 56 61 6C 69 64 00}
0x1F0AC 	VarStore: VarStoreId: 0x2A [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x4, Name: UsbControllerNum {24 27 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 2A 00 04 00 55 73 62 43 6F 6E 74 72 6F 6C 6C 65 72 4E 75 6D 00}
0x1F0D3 	VarStore: VarStoreId: 0x2B [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x21, Name: UsbSupport {24 21 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 2B 00 21 00 55 73 62 53 75 70 70 6F 72 74 00}
0x1F0F4 	VarStore: VarStoreId: 0x2C [E224EAA0-4358-6AC8-3CCE-DAA44E54F638], Size: 0x1, Name: DellVar01 {24 20 A0 EA 24 E2 58 43 C8 6A 3C CE DA A4 4E 54 F6 38 2C 00 01 00 44 65 6C 6C 56 61 72 30 31 00}
0x1F114 	VarStore: VarStoreId: 0x2D [01368881-C4AD-4B1D-B631-D57A8EC8DB6B], Size: 0xD00, Name: DellPassword {24 23 81 88 36 01 AD C4 1D 4B B6 31 D5 7A 8E C8 DB 6B 2D 00 00 0D 44 65 6C 6C 50 61 73 73 77 6F 72 64 00}
0x1F137 	VarStore: VarStoreId: 0x2E [01368881-C4AD-4B1D-B631-D57A8EC8DB6B], Size: 0x2, Name: AmiTseMode {24 21 81 88 36 01 AD C4 1D 4B B6 31 D5 7A 8E C8 DB 6B 2E 00 02 00 41 6D 69 54 73 65 4D 6F 64 65 00}
0x1F158 	VarStore: VarStoreId: 0x2F [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x79, Name: SetupVolatileData {24 28 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 2F 00 79 00 53 65 74 75 70 56 6F 6C 61 74 69 6C 65 44 61 74 61 00}
```

3. Look for `VarStoreId: 0xXX` where `0xXX` is the `VarStore` you previously noted (in our example `0x1`).

```
0x1EAD9 	VarStore: VarStoreId: 0x1 [EC87D643-EBA4-4BB5-A1E5-3F3E36B20DA9], Size: 0x14FB, Name: Setup {24 1C 43 D6 87 EC A4 EB B5 4B A1 E5 3F 3E 36 B2 0D A9 01 00 FB 14 53 65 74 75 70 00}
```

4. Write down the value of `Name` (in our example `Setup`).

## Prepare the USB

Download the latest available version of `RU.efi` from [here](https://ruexe.blogspot.com).
At the time of writing, `RU 5.31.0410 BETA` is the latest one.

##### Please note that some versions of `RU.efi` may not work with your PC. In such cases, download the previous one, and iterate the process till you can boot `RU.efi`

1. Take a thumb drive
2. Format it in `FAT32` and make sure to use `GPT` partitioning scheme
3. Create the following directory tree:

```
EFI
└── BOOT
    └── BOOTx64.efi
```

where `BOOTx64.efi` is nothing more than `RU.efi` renamed.

4. Boot your PC through the thumb drive.


## How to use this weird engineer stuff?

I don't know which keyboard layout you use, neither I care about it, but switch to US layout ASAP!
Jokes aside: 

1. Press `Alt+=`
2. Scroll down till you reach the `VarStore Name` you previously found (in our example `Setup`)
3. A matrix like table will popup. Now we need to do some quick maths (Two plus two is four, minus one that's three, quick maths)

Given the offset we're looking for - in our example `CFG Lock` offset is `0x527` we'll decompose it in `0x520` and `0x7` (quick verify that the sum is `0x527` - at the time of writing I'm still in hangover ffs).

1. Using `PgDown` and `PgUp` and the `Up/Down arrow` keys of your keyboard, scroll till you find `0x520`
2. Once you located it, use `Left/Right arrow` keys of your keyboard, reach till you find `0x7`
3. Press `Enter` key and write the value you want to set (in our example, `CFG Lock` is a bool variable that accepts `0x1` or `0x0`, hence we'll write `00`)
4. Press `Enter` key again to confirm the edit
5. Press `Ctrl-W` to write the value on the BIOS
6. Turn off your PC using the power button
7. Enjoy

##### If any weird error pops up, such as `Write variable failed: 0x00000008` then your BIOS is write-locked and there's nothing you can do. Mostly found in Lenovo Thickpads

# Credits

- [James Wang](https://ruexe.blogspot.com) for writing this wonderful tool