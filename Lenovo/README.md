## Introduction

In order to extract BIOS payload from Lenovo BIOS upgrade package you need:

- BIOS Update package
- [InnoExtract](https://github.com/dscharrer/innoextract/releases/latest)

### Step 1: extract BIOS payload

From a terminal window `cd` onto the directory where you saved the BIOS update package (e.g. `6quj18us.exe`) and run:

`innoextract -e 6quj18us.exe` 

The output should be something like this:

```
Extracting "Lenovo BIOS Update Utility" - setup data version 6.0.0 (unicode) "app/BYCN31WW.exe" - overwritten "app/BYCN31WW.exe" - Done.
```

It'll create a folder named `app` with the content of the extracted BIOS package, including the BIOS images (usually `.flX` file extension; test with UEFITool by dragging them).

#### Step 1.1: cannot locate the BIOS image

In case you cannot locate the BIOS image, look for BIOS with `.fb` file extension and expand it using 7z:

`7z x <your_bios_file_name.fb>`

Once you do it, look for the largest file (usually named `.reloc`).

---
### Step 2: find the offset and tweak it accordingly

Read [README.md](/README.md#unlock-cfg-lock)