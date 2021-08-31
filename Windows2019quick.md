


Labconfig

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'WSLab-'; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$false ;AdditionalNetworksConfig=@(); VMs=@()}

$LabConfig.VMs += @{ VMName = 'Win2016Core'     ; Configuration = 'Simple'; ParentVHD = 'Win2016Core_G2.vhdx' ; MemoryStartupBytes= 512MB }
$LabConfig.VMs += @{ VMName = 'Win2016Full'     ; Configuration = 'Simple'; ParentVHD = 'Win2016_G2.vhdx'     ; MemoryStartupBytes= 512MB }
$LabConfig.VMs += @{ VMName = 'Win2019Core'     ; Configuration = 'Simple'; ParentVHD = 'Win2019Core_G2.vhdx' ; MemoryStartupBytes= 512MB }
$LabConfig.VMs += @{ VMName = 'Win2019Full'     ; Configuration = 'Simple'; ParentVHD = 'Win2019_G2.vhdx'     ; MemoryStartupBytes= 512MB }
```