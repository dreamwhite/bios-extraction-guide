## Introduction

In order to extract BIOS payload from Dell BIOS upgrade package you need:

- BIOS Update package
- [Dell PFS Extract script](https://github.com/platomav/BIOSUtilities/blob/refactor/Dell_PFS_Extract.py)

On Dell's motherboard, BIOS extracting guide is slightly different than other vendors. 

The following procedure was tested with success on Mac OS and Windows. On Linux I've found some compiling errors with PFSExtractor. Hope to find a workarond asap.


### Step 1: extract BIOS payload bin from EXE

From a terminal window run the following command:

```bash
python3 Dell_PFS_Extract.py <BIOS_UPGRADE.EXE>
```

It will create a folder with a bunch of `.bin` files. The BIOS payload binary is usually the largest among all the files and is usually named `System BIOS blablabla.bin`.

### Step 2: find the offset and tweak it accordingly

Read [README.md](/README.md#unlock-cfg-lock)