Quick azure ad tests




Labconfig for ankkalinna

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'WSLab-'; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$false ;AdditionalNetworksConfig=@(); VMs=@()}

$LabConfig.VMs += @{ VMName = 'tools'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 512MB }
$LabConfig.VMs += @{ VMName = 'aadconnect'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 4096MB }
```

Labconfig for hanhivaara

$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'WSLab-'; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$false ;AdditionalNetworksConfig=@(); VMs=@()}

$LabConfig.VMs += @{ VMName = 'tools'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 512MB }
$LabConfig.VMs += @{ VMName = 'aadconnect'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 4096MB }
```