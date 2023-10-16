# Quick LAPS tests

This instructions are for the new LAPS

## Lab config

- Domain controller
- Workstations Windows 11 and Windows 10
- Server Windows 2022

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'LAPSDEMO-' ; DCEdition='4'; Internet=$true ; AdditionalNetworksConfig=@(); VMs=@()}

$LabConfig.VMs += @{ VMName = 'wks10'     ; Configuration = 'Simple'; ParentVHD = 'Win1020H1_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'wks11'     ; Configuration = 'Simple'; ParentVHD = 'Win1122H1_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
$LabConfig.VMs += @{ VMName = 'srv22'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 4GB ;AdditionalNetworks = $False; MGMTNICs = 1 }
```