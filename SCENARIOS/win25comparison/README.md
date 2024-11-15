# Lab setup

| HOST | ROLE |
| DC | Domain Controller |
| Win22 | Windows Server 2022 Desktop Edition |
| Win22Core | Windows Server 2022 Core Edition |
| Win25 | Windows Server 2025 Desktop Edition |
| Win25Core | Windows Server 2025 Core Edition |

*labConfig.ps1*

```
$LabConfig=@{ DomainAdminName='LabAdmin'; AdminPassword='LS1setup!'; Prefix = 'win25demo-'; SwitchName = 'LabSwitch'; DCEdition='4' ; Internet=$false ;AdditionalNetworksConfig=@(); VMs=@()}

$LabConfig.VMs += @{ VMName = 'Win2022Core'     ; Configuration = 'Simple'; ParentVHD = 'Win2022Core_G2.vhdx' ; MemoryStartupBytes= 2048MB }
$LabConfig.VMs += @{ VMName = 'Win2022Full'     ; Configuration = 'Simple'; ParentVHD = 'Win2022_G2.vhdx'     ; MemoryStartupBytes= 2048MB }
$LabConfig.VMs += @{ VMName = 'Win2025Core'     ; Configuration = 'Simple'; ParentVHD = 'Win2025Core_G2.vhdx' ; MemoryStartupBytes= 2048MB }
$LabConfig.VMs += @{ VMName = 'Win2025Full'     ; Configuration = 'Simple'; ParentVHD = 'Win2025_G2.vhdx'     ; MemoryStartupBytes= 2048MB }
```