# Assumptions

- There is no mslab installed
- There is no prebuild hydraded disks yet

## Note

- MSLAB works even with ASR rules set on!
- **IF** there is already hydraded disks, this step is unnecessary!
  - you can find hydraded disks based filename
    - OS version
    - G1 or G2 = BIOS / UEFI
    - **vhdx extension**

Search recursive

```
Get-ChildItem -Recurse | ? { $_.name -match ".vhdx" } | ? { $_.name -match "_G2." } | ? { $_.name -match "win" }

        Directory: C:\mslabs\ParentDisks


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a---        14/11/2024      0.13     8130658304 Win1020H1_G2.vhdx
-a---        13/11/2024     22.04    10186915840 Win2022_G2.vhdx
-a---        13/11/2024     22.20     6142558208 Win2022Core_G2.vhdx
```

# Building a "parent disk"

- unzip mslab.zip
  - C:\MSLABS\lab1 for example

It is important to keep folder structure the same if/when running the lab via mslab!

```
Get-ChildItem | Select-Object -Property Name, Length

Name                    Length
----                    ------
1_Prereq.ps1             44077
2_CreateParentDisks.ps1  86592
3_Deploy.ps1            114652
Cleanup.ps1              33977
LabConfig.ps1            45547
```

If you try to run *"2_createparentdisks.ps1"* without running *"1_prereq.ps1"* first, you will end up having an error.

## Prereqs

1. 1_prereq.ps1
   - there will be downloaded necessary DSC modules
   - note, there will be missing module *PSDesiredStateConfiguration*
     - Run following command to get it installed:
```     
Install-Module -Name PSDesiredStateConfiguration -Repository PSGallery -MaximumVersion 2.99
```

2. Under *ParentDisks* folder, there are now few additional powershell scripts
   - execute *CreateParentDisks.ps1*
     - Provide .iso image + .msu files (if you have any)