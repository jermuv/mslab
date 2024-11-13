# Assumptions

- there is no mslab installed
- there is no prebuild hydraded disks yet

# building "parent disk"

- unzip mslab.zip
-- c:\mslabs\lab1 for example

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

If you try to run "2_createparentdisks.ps1" without running "1_prereq.ps1" first, you will end up having an error.

## Prereqs

1. 1_prereq.ps1