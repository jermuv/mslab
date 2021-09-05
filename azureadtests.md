Quick azure ad tests




Labconfig for ankkalinna

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'ANKKALINNA-' ; DomainNetbiosName="ANKKALINNA";DomainName="Corp.ANKKALINNA.com"; SwitchName = 'LabSwitch'; DCEdition='3' ; Internet=$true ;AdditionalNetworksConfig=@(); VMs=@()}

$LabConfig.VMs += @{ VMName = 'tools'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 2048MB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'aadconnect'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 4096MB ;AdditionalNetworks = $False; MGMTNICs = 1 }
```

Labconfig for hanhivaara

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'HANHIVAARA-' ; DomainNetbiosName="HANHIVAARA";DomainName="Corp.HANHIVAARA.com"; SwitchName = 'LabSwitch'; DCEdition='3' ; Internet=$true ;AdditionalNetworksConfig=@(); VMs=@()}

$LabConfig.VMs += @{ VMName = 'tools'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 2048MB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'aadconnect'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 4096MB ;AdditionalNetworks = $False; MGMTNICs = 1 }
```


add stuff for the tools machine
```

```

add stuff for the aadconnect machine
```

```





Download MSU packages: https://www.catalog.update.microsoft.com/Search.aspx?q=KB5004870
```
$tempDestination = "D:\MSUDOWNLOADS\WINDOWS2019"
Start-BitsTransfer -Source http://download.windowsupdate.com/c/msdownload/update/software/secu/2021/01/windows10.0-kb4535680-x64_4a6f51365ed7f4c9ad34986aa2f61005af267e24.msu -destination $tempDestination\windows10.0-kb4535680-x64_4a6f51365ed7f4c9ad34986aa2f61005af267e24.msu
Start-BitsTransfer -Source http://download.windowsupdate.com/c/msdownload/update/software/updt/2021/01/windows10.0-kb4589208-v2-x64_fa90a4bdc1da0f5758cdfa53c58187d9fc894fa0.msu -destination $tempDestination\windows10.0-kb4589208-v2-x64_fa90a4bdc1da0f5758cdfa53c58187d9fc894fa0.msu
Start-BitsTransfer -Source http://download.windowsupdate.com/d/msdownload/update/software/updt/2021/06/windows10.0-kb5004332-x64-ndp48_7d9404b45a7acdf861a74e1677281adc37e1097c.msu -destination $tempDestination\windows10.0-kb5004332-x64-ndp48_7d9404b45a7acdf861a74e1677281adc37e1097c.msu
Start-BitsTransfer -Source http://download.windowsupdate.com/d/msdownload/update/software/updt/2021/06/windows10.0-kb5004335-x64_7613753e77ad9cc37415a97b87f8ecb1babab07d.msu -destination $tempDestination\windows10.0-kb5004335-x64_7613753e77ad9cc37415a97b87f8ecb1babab07d.msu
```
